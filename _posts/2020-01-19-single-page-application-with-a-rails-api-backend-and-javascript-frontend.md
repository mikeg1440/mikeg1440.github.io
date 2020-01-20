---
layout: post
title: Single Page Application with a Rails API backend and JavaScript frontend
date: 2020-01-19 20:46 -0500
---

## Getting started

  #### Requirements
   - Ruby on Rails
   - Postgresql
   - A simple server like Node live-server or just Python's simple server


  You will need to configure PostgreSQL before hand so if you have not done so you cant google it or if you are on Ubuntu for a OS you can follow [this guide here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04).  Once that is done you can create your rails back end API with the following line
  ```
  rails new <APP NAME> --api --database=postgresql
  ```

  This will create a new rails API app with a PostgreSQL database.  Once you run that command you should configure the PostgreSQL user and password in a secure way using the rails built in [encrypted credentials](https://www.viget.com/articles/storing-secret-credentials-in-rails-5-2-and-up/) or the [dotenv-rails gem]https://rubygems.org/gems/dotenv-rails/versions/2.1.1()
