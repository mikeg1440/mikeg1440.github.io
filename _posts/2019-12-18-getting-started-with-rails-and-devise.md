# Setting up a Ruby on Rails with OAuth + Devise gem for authentication and PostgreSQL

##### For a portfolio project I created while attending Flatiron coding bootcamp required that we use the authentication gem [Devise](https://github.com/plataformatec/devise) for authentication and add the ability to login using oauth.  I decided to go with Github for a OAuth login option and figured it would be helpful to others to document the process

## General App creation

  I decided to make a invoice generator app that would allow users to login, create products, clients, and accounts that would then let them generate invoices that could sent to clients via email.  The clients could then accept or reject the invoice after receiving it and the user would be able to view the status in their dashboard.  First thing I did was try to map out the relationships for my models using [DBDiagram.io](https://dbdiagram.io/) and came up with these models.

###### Models
- User
- Account
- Invoice
- InvoiceProduct
- Product

#### This is the diagram for my models
![Picture of database schema](https://i.imgur.com/u89GruO.png)

### Set up database

  If you have never used a PostgreSQL database before you will need to do a few things first to get started, you can take a look at a guide like this one for [installing PostgreSQL on Ubuntu](https://tecadmin.net/install-postgresql-server-on-ubuntu/).  After installing you will need to set a new password for the database user, you can follow the steps here if you like.
    - run `sudo su postgres`
    - then `psql`
    - next type `\password` to set a new password

  ### Create you Rails application


  The next part assumes you know how to get a rails app started using `rails generate` so you can do so now if you following along for you own app.  The only caveat is you must specify that you are using a PostgreSQL database when you create the app using the `-d=postgresql` argument.  If you are not familiar with the commands then you would get started with these commands assuming the app name is Invoicer and models are as I said before.
   - `rails new Invoicer -d=postgresql` to create the app directory
   - run `bundle`
   - `rails g model Product name price description:text` to create a product model with attributes name price and description(they default to string and text is just for holding more characters that string)
   - `rails g controller products new edit show index`

  First we need to add the postgres username and password to our application, I ended up using the DotENV gem to do this.  The gem gives you a easy way to manage environmental variables from a local `.env` file so you can put sensitive data in there while developing.  All you have to do is add `gem 'dotenv-rails', groups: [:development, :test]` to your gemfile and then `bundle install`.  Next add the following to your `config/application.rb` file before `Bundler.require`

```
Bundler.require(*Rails.groups)
Dotenv::Railtie.load
```
  Now we have to create a `.env` file in root directory of the application which you can to by `touch .env` and the next part is very import if your pushing up to a repo and it's to add the filename to the `.gitignore` file so it doesn't get uploaded because that's where we put our credentials.  You can just add environmental variables like you normally would so you can name them anything you want, so for example you might use `DB_USER=postgres` for the database user and `DB_PASSWORD=<password>`.

  Next you have to modify the `config/database.yml` file to work with our database and make the following changes
   - set the user and password to get the env variables like `<%= ENV['DB_USER'] %>`
   - set `host: localhost` for the dev and test entries


- Add `gem 'devise'` to Gemfile
- run `bundle install` to install gems
- `rails g devise:install` to install devise
- `rails g devise user` to create user migration and model(take a second to look through the migration file and uncomment any attributes you want to add to the user model)
- `rails g migrate` to run
- `rails g devise:views` to create the devise views (this creates all devise views)


Now you can go ahead and create the database and run migrations, I made this short task to facilitate the process.
```
  namespace :db do
    desc "Rebuild database, this drops, creates, migrates and seeds the db"
    task dcms: :environment do
      raise "Not allowed to run on production" if Rails.env.production?
      Rake::Task['db:drop'].execute
      Rake::Task['db:create'].execute
      Rake::Task['db:migrate'].execute
      Rake::Task['db:seed'].execute
      end
  end
 ```
 Put that in a file in `lib/tasks/` named anything and then you can run `rake db:dcms` which will drop the current databases, create new ones, run migrations and seed the db.  

 ## Add Oauth

  First to add OAuth capability to the app we need the oauth-github gem, so add `gem 'omniauth-github'` and yet again running `bundle`.

  Next we need to get access to the [GitHub API](https://developer.github.com/) by logging into your Github account and going to settings -> Developer Settings -> [OAuth Apps](https://github.com/settings/developers).  Then create a new OAuth App with the following settings
  - Homepage URL - `http://localhost:3000/`
  - Authorization callback URL - http://localhost:3000/app/github/callback
  After creating the App you need to get the Client ID and Client Secret and put those in the `.env` file then add this line to `config/initializers/devise.rb`
  ```
  config.omniauth :github, ENV['GITHUB_APP_ID'], ENV['GITHUB_APP_SECRET'], scope: 'user:email'
  ```


### Add strong parameters

If you want to add addition fields to the login or sign up that what the default devise views have you have create a class that inherits from `Devise::RegistrationsController` file with whatever parameters you want to accept like this as well as add any attributes you want to save.

  ```
  private
  def sign_up_params
    params.require(:user).permit(:first_name, :last_name, :email, :password, :password_confirmation)
  end
  def account_update_params
    params.require(:user).permit(:first_name, :last_name, :email, :password, :password_confirmation, :current_password)
  end
  ```

  Then in the `config/routes.rb` file put `devise_for :users, controllers: { registrations: ‘registrations’, omniauth_callbacks: ‘users/omniauth_callbacks’ }` which tells devise to look for the controller named registrations for registration handling, and callbacks for handling OAuth  you can do this with sessions, or any other devise module.

  Once you have done that you should be all set to go the sign in page and a option at the bottom should say `Sign in with Github` and if you click it you should see something like this.

  ![https://i.imgur.com/K1kEqRD.png](https://i.imgur.com/K1kEqRD.png)

### And thats it, you have connected your app to Github via OAuth and users can now sign in your app using their Github authentication.
