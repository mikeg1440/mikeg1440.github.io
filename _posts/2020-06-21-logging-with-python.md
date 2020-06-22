---
layout: post
title: Logging with python
date: 2020-06-21 23:27 -0400
---
<div class='text-center'>
  <img src='' alt='' />
</div>

<div class='text-center'>
  <img src='https://lh3.googleusercontent.com/proxy/fipEKgyX6UZVhxmBVVDMhycEd8__YAnvWLtiD3HZ1l0Ulih0zhEwnTui63-c6eHCNzSmdRxMgROdicBRdlGiGXcldIFfTW6V6DIddmWtDceTgoaGXOGVLA5fvij2GY5ISd2ngRvTI4SJrOLS-2jrAZJDOA' alt='Logging your python apps' />
</div>

When I first started writing code I was using the print statement to display data or notifications so I knew the program was running properly.  As I progressed as a coder I became aware of the tools that are at your disposal giving you a better way to than using the print statement for all your debugging and diagnostics.  Python has actually had a logging module built in as far back as version 2.3 and is extremely useful for providing a view of the behavior of a running program.  This post is going to take look at how to use the logging module in python and how you might put it to use.

By default you have 5 logging levels you can use to indicate the severity of the message, their names make the severity level fairly obvious.  These are the 5 levels
- [ ] Critial
- [ ] Error
- [ ] Warning
- [ ] Info
- [ ] Debug

These different levels look like the following with the default configuration automatically loaded with the module.  A simple example would look like this.

```
import logging

logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical message')
```

<div class='text-center'>
  <img src='https://i.imgur.com/ZwitIYD.png' alt='Screenshot of python debug output' />
</div>

Notice that you don't see the output for debug or info logging in the output of running that code.  This is because anything with a level less that warning doesn't get displayed.  This is the default configuration that you can change with some settings setting the logging module to log all events.

The next part to know about is the configuration options that you have available.  One of the main ones you might want to know about are printing out to file.  
```
import logging

logging.basicConfig(filename='myApp.log')
logging.warning('This will get logged to a file')
```
In this snippet you can set a `filename` argument using basicConfig to specify a log filename.  The default write mode for this append so the logs will appended to each other.  You can change this by specifying the `filemode` to basic config as well so if you wanted the logs from the last run you can set it like this.
```
import logging

logging.basicConfig(filename='myApp.log', filemode='w')
logging.warning('This will get logged to a file')
```

Beyond just the actual log message you may want to format it a certain way you can also do that with the `basicConfig` method and pass in a `format` argument that will allow you to do things like put a timestamp in front of every message.

```
import logging

logging.basicConfig(filename='myApp.log', filemode='w', format='%(asctime)s - %(levelname)s - %(message)s')
logging.warning('This will get logged to a file')
```
The image below shows some other format variables you can assemble to get your logs the way you want them.
<div class='text-center'>
<img src='https://i.imgur.com/UgSgNFP.png' alt='logging format options and their format name' />
</div>

Next configuration option that's good to know about the `level` argument that you can use to set the base level at which it will record so if you wanted to see everything then you can set it to `logging.DEBUG` or if you only wanted the important messages then you can set it to `logging.ERROR`.
```
import logging

logging.basicConfig(level=logging.DEBUG)
logging.debug('This will get logged')
```

After getting the logs looking the way you want and output the way want your most likely going to need to log variables.  This you can do with any level but our example will only use error.

```
import logging

user = 'Nick'

logging.error('%s raised an error', user)
```

And that's a good starting point for what ever you need o log in your code, I encourage you to take a look at the [documentation](https://docs.python.org/2/library/logging.html) where you can find more detail on any of the processes.
