---
layout: post
title: Single Page Application with a Rails API backend and JavaScript frontend
date: 2020-01-19 20:46 -0500
---

## Intro
  I recently had to create a [Single Page Application(SPA)](https://en.wikipedia.org/wiki/Single-page_application) that uses a Rails back end for handling and storing data and JavaScript for the front end.  I chose to create a simple poll app that allows users to create a poll that other users can then take and everyone can view the results.  So the examples will be using names based around that kind of application.

## Getting started

  #### Requirements
   - Ruby on Rails
   - Postgresql
   - A simple server like Node live-server or just Python's simple server


  You will need to configure PostgreSQL before hand so if you have not done so you cant google it or if you are on Ubuntu for a OS you can follow [this guide here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04).  Once that is done you can create your rails back end API with the following line


  ### Create the app
  ```
  rails new <APP NAME> --api --database=postgresql
  ```

  This will create a new rails API app with a PostgreSQL database.  Once you run that command you should configure the PostgreSQL user and password in a secure way using the rails built in [encrypted credentials](https://www.viget.com/articles/storing-secret-credentials-in-rails-5-2-and-up/) or the [dotenv-rails gem](https://rubygems.org/gems/dotenv-rails/versions/2.1.1).  

  ### Add some Gems

  Here we need to enable the cors gem by uncommenting `gem 'rack-cors'` in the Gemfile for handling cross origin resource sharing.  Also I added some other gems I find useful like `gem 'active_model_serializers'` for easy setting up json return values and for debugging and whatnot so you should add these as well
   - `gem 'byebug'`
   - `gem 'pry-moves'`
   - `gem "better_errors"`
   - `gem 'faker'` - for seeding database with filler data
   

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
