---
layout: post
title: A Look at Ruby on Rails Scopes
date: 2019-12-22 19:31 -0500
---
## What is scope?

  [Named Scopes](https://api.rubyonrails.org/classes/ActiveRecord/Scoping/Named/ClassMethods.html#method-i-scope) or scopes are just methods that you can use to define custom queries inside your Rails models with the `scope` method.  They can be used to grab a subset of objects from your database and are a great way to reduce redundant code as well as long and frequently used queries.  Let's look at an example of a scope method with a name of `most_recent` in a model called `Event`.

  `/app/models/event.rb`
  ```
  scope :most_recent, ->(limit){ order("created_at desc").limit(limit) }
  ```
  This example shows a simple way to get the most recently created Event objects and allows a limit to be passed in as an argument in order to specify how many records we want returned.  To get the most recent event objects created you would be able to call it like this `@recently_created_events = Event.most_recent(5)`.  

## But why use scope?
  There are a few reasons why you would want to use scopes:
   - They result in cleaner code
   - They are easier to read and stand out more than regular methods
   - They should be used for a single purpose so it's easier to read, edit and understand

  It's true that you can do essentially the same thing by creating a [class method](https://www.geeksforgeeks.org/ruby-class-method-and-variables/), for instance you could implement the previous example as a class method like this
  ```
def self.most_recent(limit)
    order("created_at desc").limit(limit)
  end

  ```

## So what's the point of scope methods?
  You may be asking what is the point if you can just get away with using class methods?  Well, you can chain them with other methods and implement certain code in a much more [DRY](https://dzone.com/articles/software-design-principles-dry-and-kiss) way.  An example of something like this is chaining scopes with other methods and not having to worry about edge cases breaking things, like when you get a `nil` value returned back from a method call.  Take this example where we want to be able to search for all of the records created since a certain date/time, and if we don't provide a date at all then the method simply returns all the records.
  ```
  scope :created_since, -> (time){where("events.created_at > ?", time) if time.present?}
  ```
  This is much more DRY than the alternative class method which in this case would look similar to this example
  ```
    def self.created_since(time)
      if time_present?
        self.where("events.created_at > ?", time)
      else      
        self.all
      end
    end
  ```
  So that way we can just call `Event.created_since(7.days.ago)` to all event objects created within the last 7 days.

  Another example could be if we wanted to get all the events that take place inside the US then we could create a named scope like this
  ```
    scope :in_usa, -> {where(country: "US")}
  ```
  So then we can call it like `Event.in_usa` to get all the events taking place in the US, or to improve this a bit and abstract one more level we can code it like this
  ```
    scope :location(country), ->(country){where(country: country)}
  ```
  Now we can pass whatever country in as a argument and we can get the events by country.  And if we had the previous example scope defined too we could [chain](https://www.sitepoint.com/dynamically-chain-scopes-to-clean-up-large-sql-queries/) them together like this `Event.location("US").created_since(5.days.ago).limit(5)` without having to worry about getting a `nil` response from any one of the methods called potentially preventing a lot of head aches.


## Scoping for efficiency
  A situation where scope becomes very useful is in the case where you have a database of users that does a `soft-delete` when a user deletes their account and instead of deleting the data it just changes the user status to 'inactive' and the database still holds the account record.  So now as your user database grows it can take significantly more time to search through ALL the user records, when in realistically for most tasks you only really want to be searching through the currently 'active' users.  So you could improve the speed of your app by defining some scopes like this within a user model.

  `/app/models/user.rb`
  ```
  scope :active, -> {where(status: 'active')}
  scope :inactive, -> {where(status: 'inactive')}
  ```
  So then when you want to do a search within the active user set you would use this code `User.all.active` and inversely `User.all.inactive` to use the inactive user set.

  Another good reason to use them is because scopes express intent, it's a good idea to use them whenever your chaining simple scopes that are already [built in](https://apidock.com/rails/ActiveRecord/QueryMethods) to rails like `limit` or `where` into more complicated scopes.  This is essentially what scope was designed for.

##### There are a few caveats regarding the use of scope
   - If you need to preload scopes then build them in with [active record associations](https://guides.rubyonrails.org/association_basics.html) instead.
   - If you use more than the built in scopes then you should use a class method instead
   - Don't use a [default scope](https://apidock.com/rails/ActiveRecord/Base/default_scope/class) (scope auto applied to your model) because they can be hard to debug later in the apps development.

  So in review you should use scopes when your chaining the built in methods of selecting, sorting, joining, and filtering code.  Default scopes can be useful, but be careful because of the possible issues debugging them in the future.  You can DRY out your code, make it easier to understand and possibly increase your apps efficiency if used in the proper way.  It's just another great tool that Ruby on Rails gives you to make your code better and your life easier, but you have to understand it first to really take advantage of all that named scopes can offer.
