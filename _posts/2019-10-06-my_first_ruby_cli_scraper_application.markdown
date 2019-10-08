---
layout: post
title:      "My First Ruby CLI Scraper Application"
date:       2019-10-07 00:12:41 +0000
permalink:  my_first_ruby_cli_scraper_application
---


I'm currently at the end of my fourth week at Flatiron Coding School and the project for this week was to create a CLI Data Gem as a portfolio project.  I ended creating two different apps but only because I wanted to make something a little bit more useful after finishing most of the first program.  This is a short walkthrough of what I did to start the application, hopeful you can pick something new up or least learn from my frustrations.
![FreelancerFinder Menu](https://i.ibb.co/N6v8ffq/freelancer-finder-menu.png)
This is the menu of the application I created last but first I will go through the first application.  I will be adding to this post over time so it may be incomplete at the time of reading but I wanted to get some usefull info out there that I will be able to refer back to myself.

## General Requirements  
 - Provide a CLI interface
 - Provide data from a site on the internet
 - Data that goes one level deep ( user must be able to choose something and get more details about it)
 - Good Object Oriented Design

## Specific Requirements
- A README.md file with the following
  - Short Description
  - Install Instructions
  - Contributors Guide
  - Link to License for the code
- CLI Essentials
  - CLI for interfacing with application
  - Pulls data from a external source
  - Has both list and detail views
  - Go one level deep
  - Follow DRY(Dont Repeat Yourself) principle


### Planning

This is were I made my mistake, I wanted to get started coding so I just got started with the first halfway decent idea I had.  The idea was to use the site [https://10times.com](https://10times.com) which displays tech expo events around the world and allows you to click a event and see the details of it.  I thought it was perfect because it was somewhat programming related that I might use once and a while.  So I started out by creating a outline for the program and came up with four classes that I would need.  A CLI class to interact with the user, a Scraper class to scrape the data, a Event class to hold the events and I ended up adding a Location class after starting to code so that it would be easier to find events by their location.  

### Creating the application structure

Next I started by creating a new repo on Github and then getting the general structure I would be using from Bundler by running this command `bundle gem event_finder`.  It creates the files/folders [shown here](https://drive.google.com/open?id=1DJmMK86bSK0-XoYZ2SJDZg33hjgscrMi).

![Default File Structure](https://i.ibb.co/bBFMKyB/bundle-gem-structure.png)

### Initialize git

  ```git init```

This will initialize a git folder and files

  ```git remote add origin https://github.com/git-repo-her```

This will add the repo you created as the remote origin for the code to push to

### Create the files we will need

Next I created my class files in `/lib` and executable file in the `/bin` directory

```touch lib/event_finder/cli.rb```

```touch lib/event_finder/event.rb```

```touch lib/event_finder/scraper.rb```

```touch lib/event_finder/location.rb```

```touch bin/event_finder```

Then make out executable actually executable and add a few lines

```chmod +x bin/event_finder```

And inside of `bin/event_finder`

```
 #!/usr/bin/env ruby

require_relative '../lib/event_finder.rb

EventFinder::CLI.new.call
```

### Set up our environment file

Next we need to include our class files in the environment file `lib/event_finder.rb`

```
require 'bundler'
require 'open-uri'
Bundler.require(:default, :developement)

module EventFinder
  require_relative '../lib/event_finder/cli'
  require_relative '../lib/event_finder/event'
  require_relative '../lib/event_finder/scraper'
  require_relative '../lib/event_finder/location'
end
```

### Add our dependencies

Because we are using Bundler we have a .gemspec file that we need to fill with some information and include our dependencies in as well.  Anything that has a TODO or a FIX will need to be edited to run our program at all so go ahead and fill out all the relevant information.  Next we add the dependencies at the end of the file I added these.

```
spec.add_development_dependency "pry"

spec.add_dependency "nokogiri"
```

At this point we should be able to run `bundle install` to install the correct gems and allow us to run the executable.  If we comment out the `EventFinder::CLI.new.call` line and put a simple `puts "hello world` in our `lib/event_finder/cli.rb` file then run `./bin/event_finder` we should see "hello world" in the terminal output.

### Creating the classes

I started out by just creating the methods that I wanted to be able to call and how I wanted the flow of the program
to go from start to finish as I imagined the finished program.  I did this for each class, creating methods like
`show_menu` and `scrape_events`.  Then I started with the smaller classes and thought of all the attributes the class could have and created attribute accessors for them and created the base initialize methods.  From here I coded the starting logic of the CLI class and pry'ed into the the program after I scraped the document and while looking at the sites HTML using Chrome's Dev Tools I found the CSS selectors for each of the pieces of info I wanted from the main page.  Using the information I created a instance of event class for each event scraped.



### TL;DR
The gist of this article is just how to get started on a ruby CLI gem using Bundler and I will be adding more to the post as I have time.  Next up we will be looking at starting to code our classes as well as then what makes a good website to scrape data from and then how to go about scraping it.
