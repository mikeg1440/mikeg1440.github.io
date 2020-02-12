---
layout: post
title: Configure Rails with PostgreSQL using Encrypted Credentials
date: 2020-02-11 23:39 -0500
---
[PostgreSQL](https://www.postgresql.org/) is a open-source relational database that management system that is designed to handle a range of workloads from running locally on your laptop to data warehouses with multiple concurrent users.  This makes it an excellent choice for a database setup in any Ruby on Rails application.  This is a short walk-through on how to get it set up initially because it can be a little tricky and the documentation is a bit hazy.  

## First Steps

You need PostgreSQL installed on the system your using, if your not sure whether or not it is already installed you can run `psql --version` and if you see something like `psql (PostgreSQL) 10.10 (Ubuntu 10.10-0ubuntu0.18.04.1)` then you can skip this next part.  I personally use Ubuntu/Debian but if you run a different OS then you can take a look at [this post for Windows](https://www.guru99.com/download-install-postgresql.html) and [this post for Mac](https://www.robinwieruch.de/postgres-sql-macos-setup)

Ubuntu/Debian users run the following command to install the packages we need, in a terminal run `sudo apt update && sudo apt install postgresql postgresql-contrib`.  The postgresql-contrib package has some additional utilities that and functionalities that are not part of the core package and is a good idea to have installed as well but is not needed to run the database.    

## Configure User Roles

PostgreSQL uses a concept called "Roles" to manage access permissions to the database.  Meaning we have to set up a Role for a specific user to grant access to do things like create, edit and delete tables within the database.  For an example we will be creating a user called `DB_USER` but you can make it whatever you want.  By default PostgreSQL creates a default user `postgres` that has all rights relating to the database so we have to make our changes as that user.  So to creating a new user looks like this `sudo -u postgres createuser DB_USER -P`.  Make sure to include the `-P` option to set a new password as there is no default, and you will be prompted to enter a new password twice.  This is only for the new user role we just created and is only valid within the database environment.  Make sure to remember this username because we will need to connect give Rails access to the database.


## Setting up the App

The next thing you need to do is set the PostgreSQL as the database when creating the Rails app from the start.  It is possible to switch a previously set up app that was created using a different database but that's outside of the scope of this post.  So just as an example we call the name of our app `Doom` so the command to run would look like `rails new Doom -d=postgresql`.  That would scaffold a new app generating the files you need to connect to the PostgreSQL database.  

## Encrypted Credentials

Since Rails 5.2 we have the ability to use built in Encrypted Credentials for managing passwords and other sensitive information securely.  You do this by running the following command, note you can use any editor you want to do this like VIM or Nano, we will just use VIM because its installed on most systems by default. `EDITOR=vim rails credentials:edit` then inside the file that opens enter this (we are using a password of `sassafras` for an example)
```
# aws:
#   access_key_id: 123
#   secret_access_key: 345

postgres:
  password: sassafras

# Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
secret_key_base: a730e23s98dfs89df7jlksdf25c4b2a91c2311e5daa5dd3c484e20604c24446b9cf2e800610a00ecedfaf900f9e36565bs09dflkjf3f293c689
```
If unfamiliar with vim to exit you hit `ESC + ':' + 'wq'` to write to the file and save it.  The file is then encrypted and cannot be read without the key found inside `config/master.key`.

## Config File

Now we just need to let Rails know how to access the database by editing the newly created `config/database.yml` file within projects main directory.  There are 3 different sections within this file that allow you set different credentials for different environments (Development, Test and Production).  Because your going to be developing the app first we will just look at the default section but if you put the the same lines in the other sections it will default to those creds.  So find the part of the file that looks like this
```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>  
```
Add this line after the `pool:` entry
```
password: <%= Rails.application.credentials.postgres.dig(:password) %>
```

And thats it! You should now be able to run `rails db:create` and see that a new table was created in the database.  You can check this by running `sudo -u postgres psql` to get the interactive terminal for working with the database.  Entering `\l` will list all the tables in the database and you should see a table with name of your app with development added on because its the development DB.  
