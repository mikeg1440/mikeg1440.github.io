---
layout: post
title: Scraping data with Selenium and Python
date: 2020-06-14 21:57 -0400
---
Selenium is web based automation tool that allows you to automate the usage of a browser.  It can be useful for alot of things like testing and data scraping.  This post is just going to be a short introduction to get you started using selenium in python.

For these examples we are using [Python 2.7](https://www.python.org/download/releases/2.7/) and the [selenium webdriver](http://pypi.python.org/pypi/selenium) on Ubuntu Linux.  

## Starting the Webdriver

So to start everything off we need to import the proper package, if you don't already have selenium installed then run `pip install selenium` before following the next few steps.

```
from selenium import webdriver

driver = webdriver.Firefox()
```

This imports the webdriver which is like the controller for selenium.  We then start up a instance of Firefox(), you should note that there are other options like using Chrome if its installed and even running as a headless browser meaning there is no actually visible browser while the script is running.  

Next we have to give the driver a url to navigate to, we are just going to use rotten tomatoes as our site to scrape.
```
driver.get('https://www.rottentomatoes.com/browse/in-theaters/')
```

Now we have our webpage we take a look at what elements make up the data we want to grab.  This is were your browers Dev Tools really come in handy.  On most browsers you can right click on a element you want to inspect and select the `Inspect Element` option to view all the HTML code and styles.

<div class='text-center'>
  <img src='https://i.imgur.com/DpWVB56.png' width='100%' alt='screenshot of rotten tomatoes' />
</div>

Here you can see that each movie element has a class called `mb-movie` so we can use that collect all those elements using selenium.  
```
posters = driver.find_elements_by_class_name('poster')

for poster in posters:
  print poster.text
```

With this code we grab all the elements that have their class as `mb-movie` then we iterate over them and print out the text found within each element

<div class='text-center'>
  <img src='https://i.imgur.com/74dwCkp.png' width='100%' alt='screenshot of rotten tomatoes' />
</div>


## Extract the info

Now we can go about grabbing the specific info we want to scrape, so we are going for the title of the movie to start off.  Using our Dev Tools again we can see that each poster has a movie title inside a H3 tag with a class of `movieTitle`.

<div class='text-center'>
  <img src='https://i.imgur.com/lLWBlqZ.png' width='100%' alt='screenshot of rotten tomatoes title element' />
</div>

```
movies = []

posters = driver.find_elements_by_class_name('poster')

for poster in posters:
  title = poster.find_elements_by_class_name('movieTitle')

  movies.append(title.text)
  print title.text
```

You can do this for the all the rest of the data you want to grab.  So for example you wanted the URL for each movie with the title you can do it like this.

```
for poster in posters:
  title = poster.find_elements_by_class_name('movieTitle')[0].text
  url = poster.find_elements_by_tag_name('a')[0].get_attribute('href')
  movies.append({title: url})
  print "Title: {} ==> {}\n".format(title, url)
```

<div class='text-center'>
  <img src='https://i.imgur.com/yMoOrFO.png' width='100%' alt='screenshot of rotten tomatoes movies and their url printed in the terminal' />
</div>


## Cleanup

After you finished with all the testing or scraping you want to do don't forget to close down the driver so it kills the browser process so if you run this script multiple times it wont bog down your system with unneeded processes.

```
driver.close()
```
