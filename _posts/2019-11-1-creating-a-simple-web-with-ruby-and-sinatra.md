---
layout: post
title: 'Setup for a simple web app with Ruby and Sinatra'
date: '2019-11-1 15:12:11 -0400'
categories: ruby, sinatra
---
<head>
  <style>
    a{font-weight: bold;}
    .highlighter-rouge{background-color: darkgrey; color: black; padding: 1px;}
    .highlight{color: aliceblue; background:black; padding: 10px;}
  </style>
</head>
<p align="center">
  <img src="https://d1avok0lzls2w.cloudfront.net/img_uploads/sinatra-logo.jpg">
</p>

<h1 align="center">
Setup for a simple web app with Ruby and Sinatra
</h1>

<h2 align="center">Part 1</h2>

  Sinatra is a DSL (Domain Specific Language) in Ruby that can be used to simplify the process of creating web applications, cutting down on the time it takes to get the general base of a web app set up and take care of the more repetitive tasks.  Sinatra is a more lightweight and flexible Web Application Framework than some others WAF's such as Rails, giving the developer just the bare minimum so its a perfect way for new developers to get a good feel for how things are actually working under the hood of more advanced frameworks like Rails.  

  We will be setting up a simple todo web app that uses active record and Sinatra to allow users to create, edit, update and delete tasks.  We will also create a user model to allow users to login and out plus create new users.  The main models here will be tasks and users and we will use [Active Record Associations](https://guides.rubyonrails.org/association_basics.html) to create the relationships between the models.

  To facilitate the creation of our file structure and to get the project started we will be using a gem called [Corneal](https://thebrianemory.github.io/corneal/).  Corneal is defined as "A Ruby gem that is a Sinatra app generator with Rails-like simplicity. I built this to help fellow Flatiron School students with their Sinatra assessments" and is built by a fellow Flatiron student to ease the tedious setup part.  Make sure to familiarize yourself with the files and setup that we get from Corneal as it will help your understanding later on.

  ##### Requirements
  - Ruby
  - Sinatra
  - Corneal
  - IDE or Text Editor



  ### Creating your project directory

  Setting up your application with Corneal is as easy as `corneal new APP-NAME`.  Of couse your would replace 'APP-NAME' with your apps name so in this case we would run `corneal new todo-app`, while you can always change your apps name later on be aware that corneal will be inserting the app name given in multiple files so all will need to edited if you do change the app name.  This will give you the following file structure.

  ![Corneal App Structure](https://i.imgur.com/1lcHv2H.png)

  ### Initialize git
  Next you should initialize and commit to git by running `git init` and then `git add .` to stage all the files and finally `git commit -m "Add initial project creating files with Corneal"` to commit the changes and include a descriptive message.  No we are ready to start planning how we want the app to work.

  ### App planning
Just to make your life easier and help others who may want to contribute to the project I created a `outline.md` file to list out the features and plan the flow of the application so that I can better organize my thoughts and refer to this I code.

To start off I wanted to get the main features of the project working, kind of like a minimum viable product, basically the bare bones for our app.  We want to be able to
  1. Create new tasks with a title and description
  2. Be able to edit and update a task
  3. Be able to delete a task
  4. Be able to view all tasks that belong to user

After we that part is complete we want to add the following features as well.
  1. Have a sign up page for users to create a account
  1. Make usernames unique so we can use the username as the identifier for looking up users
  2. Have a login page for users
  3. Be able to logout users
  4. Tasks are only visible to the user that created them
  5. Users can't edit tasks that they did not create

Finally if we have the time will can also add the following features.
  1. Allow users to set a due time for tasks
  2. Allow users to set a priority level for tasks
  3. Allow users to sort tasks by priority
  4. Allow users to sort tasks by due time


### Start coding
Now that we have our basic outline we can started on actually coding.  We already know what are going to need a User table and and a Tasks table to store our data so that our app is persistent.  Before anything else make sure to run `bundle install` to install the required gems from our Gemfile, if the Gemfile.lock is cause a error then just delete it run bundle again.  To create our tables we can either use the gem `Rake` or `Corneal` to help us create migrations, following convention you should make the name of the migration descriptive of what you are doing.  

##### Using Corneal
Corneal streamlines the process a little bit for us if we want, knowing we will need both user and task models we can run `corneal model user` and `corneal model task` which will create the migration file for use automatically as well as our model files.  Its important your use the name of the model in its singular form because corneal with make the migrations plural as displayed in the picture below.

![Corneal Model Creation](https://i.imgur.com/O9Htwic.png)

##### Using Rake
So using `Rake` to start creating our users table we can do something like this `rake db:migrate NAME=create_users` which will add a file called `XXXXXX_create_users.rb` inside of the folder `db/migrate/` for us, the `XXXXXX` part will be a time stamp so rake knows which migrations to run first.  Within that file there is a change method already set up for us to use where we would put the following code

##### Coding Migrations
```
def change
  create_table :users do |t|
  t.string :username
  t.string :email
  t.integer :password_digest
end
```

Note the `:password_digest` part because we are going to use BCrypt with active record to make secure password authentication and storage.  The documentation calls for a attribute `XXX_digest` where `XXX` is the desired name of the attribute, but you need to have the `_digest` for it to work properly.

Next we create the tasks migration by running `rake db:create_migration NAME=create_tasks` and put the following code inside the file.
```
def change
  create_table :tasks do |t|
    t.string :title
    t.text :description
    t.boolean :complete, default: false
    t.integer :user_id
  end
end
```
This gives our tasks a title, description and completion attributes as well as a `:user_id` attribute to store the id of the user that created the task.  Now we can create our models and associations.

#### Models and Associations
This is where I really love active record for its simplicity and power, we are going to create a file called `user.rb` and `task.rb` within the directory `app/models/` where the associations will go.  This what our file structure should look like now.

![App Files after Model & Migrations](https://i.imgur.com/bskseJA.png)

Inside of the `app/models/user.rb` file we will make our association of users having many tasks like this.
```
class User < ActiveRecord::Base
  has_secure_password
  has_many :tasks
  validates :username, :email, uniqueness: true
end
```
Notice the `validates` part we have to set the email and username attributes to be unique so that we can't end up with duplicate usernames and if a user has already signed up with a email we want to alert them of this, active record will ensure that these constraints are met and all we had to do was add two lines of code.  Also we need to include `has_secure_password` for our secure password setup to function correctly.

Inside of tasks looks like this.
```
class Task < ActiveRecord::Base
  belongs_to :user
  validates :title, presence: true
end
```
This indicates that each task belongs to a single user and validates that we must create a task with a title.

#### Controllers
Now we are on the [controller](https://guides.rubyonrails.org/v2.3.11/action_controller_overview.html) files for the application, these files are found in the `app/controllers/` folder and container most of the logic for the routes we want to use.  Naming convention is to use the name of the model the controller is for then `_controller` so in this case we would want to have a `app/controllers/users_controller.rb` and a `app/controllers/tasks_controller.rb` in addition the the main applicaiton_controller created by Corneal.  Again the Corneal app can facilitate the process for use and carry our a number of steps.  By running `corneal controller tasks` the gem will do the following
  1. Create the files
    - app/controllers/tasks_controller.rb
    - app/views/tasks
    - app/views/tasks/edit.html.erb
    - app/views/tasks/index.html.erb
    - app/views/tasks/new.html.erb
    - app/views/tasks/show.html.erb
  2. Insert the line `use UsersController` in the file `config.ru` so we have access to the code in the new files

Do the same thing for users `corneal controller users`.

#### Routes and Views
Now we can start the actual coding for the logic of the app.  Try to keep the [routes](https://guides.rubyonrails.org/routing.html) that are related to the views within their respective controllers so dont put the `/tasks` route in the `users_controller.rb`.  We want to have the ability to view all tasks, create, edit, update, and delete tasks so we want the following routes to render the corresponding views.
  ```
  GET             /tasks              index
  GET             /tasks/:id          show
  GET             /tasks/new          new
  GET             /tasks/:id/edit     edit
  PATCH           /tasks/:id          update
  POST            /tasks              create
  DELETE          /tasks/:id          destroy
  ```
Create routes and views for each of the actions listed above for both tasks and users.

#### Sessions
To make cookies work and be able to keep track of user sessions we need to add a few lines of code in two files.  The first thing we need to do is add the line `use Rack::Session::Cookie` to the file `config.ru` and then add
```
enable :sessions
set :session_secret, {ENV['SESSION_SECRET']} {SecureRandom.hex(64)}
```
The session_secret is looking for a environment variable called `SESSION_SECRET` so we can either set that using the following line `SESSION_SECRET=$(ruby -e "require 'securerandom'; puts SecureRandom.hex(64)")` or just generate a new secret everytime the app starts up but doing it that way will invalidate any pre-existing sessions.

#### Part 1 Conclusion

So at this point we have created a foundation for our to do task application and can start on the actual application routes, features and interfaces that will make the program do what we laid out in the `outline.md` file and part 2 will cover the implementation of the models and views to complete the program.  
