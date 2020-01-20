---
layout: post
title: Single Page Application with a Rails API backend and JavaScript frontend
date: 2020-01-19 20:46 -0500
---

## Intro
  I recently had to create a [Single Page Application(SPA)](https://en.wikipedia.org/wiki/Single-page_application) that uses a Rails back end for handling and storing data and JavaScript for the front end.  I chose to create a simple poll app that allows users to create a poll that other users can then take and everyone can view the results.  So the examples will be using names based around that kind of application. You can see the code from this project [here](https://github.com/mikeg1440/PollerApp)

## Getting started

  #### Requirements
   - Ruby on Rails
   - Postgresql
   - A simple server like Node live-server or just Python's simple server


  You will need to configure PostgreSQL before hand so if you have not done so you cant google it or if you are on Ubuntu for a OS you can follow [this guide here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04).  Once that is done you can create your rails back end API with the following line


  ### Create the app
  ```
  rails new PollAppBackEnd --api --database=postgresql
  ```

  This will create a new rails API app with a PostgreSQL database.  Once you run that command you should configure the PostgreSQL user and password in a secure way using the rails built in [encrypted credentials](https://www.viget.com/articles/storing-secret-credentials-in-rails-5-2-and-up/) or the [dotenv-rails gem](https://rubygems.org/gems/dotenv-rails/versions/2.1.1).  

  ### Add some Gems

  Here we need to enable the cors gem by uncommenting `gem 'rack-cors'` in the Gemfile for handling cross origin resource sharing.  Also I added some other gems I find useful like `gem 'active_model_serializers'` for easy setting up json return values and for debugging and whatnot so you should add these as well
   - `gem 'byebug'`
   - `gem 'pry-moves'`
   - `gem "better_errors"`
   - `gem 'faker'` - for seeding database with filler data

  After adding the gems run `bundle install` and then we need to uncomment the lines
  ```
  Rails.application.config.middleware.insert_before 0, Rack::Cors do
    allow do
      origins '*'

      resource '*',
        headers: :any,
        methods: [:get, :post, :put, :patch, :delete, :options, :head]
    end
  end
  ```
  From the file

  ### Create/Run Migrations
  After doing that you start creating your migrations, for this example we will use the names polls, answers, and submissions for tables.  So we can generate them like this, specifing the name of the table then attributes(we don't need to specify the datatype for some because the default of string is fine).
  ```
  rails g migration polls title author question answer_ids:integer
  rails g migration answer content poll_id:integer
  rails g migration submissions poll_id answer_id
  ```
  Now run your migrations with `rails db:migrate`, you may have to run `rails g db:create` first if you have not already.   

  ### Create Models

  To connect our database via code we have to create the corresponding models.  So to generate those you can run
  ```
  rails g model poll
  rails g model answer
  rails g model submission
  ```

  ### Create Controllers

  Here we create our controllers with you guessed it, generators
  ```
  rails g controller Polls
  rails g controller Submissions
  ```
  For this example we really only the two here because we are going to create the answers with nested attributes in the polls model/controller and then return the answers along with the poll for the polls_controller show route.

  ### Model Relationships and Nested Attributes  

  So we still need to put the relationships in our models and add the nested attributes for statement, so for the poll model
  ##### app/models/poll.rb
  ```
  class Poll < ApplicationRecord
    has_many :answers
    has_many :submissions

    accepts_nested_attributes_for :answers
  end
  ```
  ##### app/models/answer.rb
  ```
  class Answer < ApplicationRecord
    belongs_to :poll
    has_many :submissions
  end

  ```
  ##### app/models/submission.rb
  ```
  class Submission < ApplicationRecord
    belongs_to :poll
    belongs_to :answer

  end
  ```

  ### Serializers

  To get our API to return only the attributes we want we can use rails serializers.  Since we already added the Serializers gem and ran `bundle install` we can simple create them with generators again.
  ```
  rails g serializer  polls
  rails g serializer submissions
  ```
  And then we specify the attributes we want to use within the newly generated files, and we can specify the polls to return the answers along with each poll when its requested so the polls_serializer would look like this.
  ```
  class PollSerializer < ActiveModel::Serializer
    attributes :id, :title, :author, :question, :answers
  end
  ```
  And the submissions serializer
  ```
  class SubmissionSerializer < ActiveModel::Serializer
    attributes :id, :poll_id, :answer_id
  end
  ```

  ### Add routes and controller methods
  So now we have to specify the routes, we can do this easily with by adding the following the `app/config/routes.rb` file and because we don't need all the routes we can specify the ones we want with the `only: []` line
  ```
  resources :polls, only: [:index, :create, :show] do
    resources :submissions, only: [:index, :create]
  end
  ```
  When you put a route within another route like this it means that url will be nested so to reach the polls routes it would be something like `http://localhost:3000/polls` and for the index route and for submissions or a specific poll it would look like `http://localhost:3000/polls/1/submissions` for the index route showing all submissions for the poll with ID of 1.

  Next you would create the corresponding controller methods, I'm not going to review that here but if you want to see an example you can take a look at the [back end repo](https://github.com/mikeg1440/PollAppBackend) to get an idea of what you can do.

  ### Run the local server
  After creating the controller methods we should have a working API that you can use by running the server with `rails server` and then test with something like [Postman](https://www.getpostman.com/downloads/) which is a great free tool to helping you build and test API's.
