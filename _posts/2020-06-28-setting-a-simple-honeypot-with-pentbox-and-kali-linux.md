---
layout: post
title: Setting a simple honeypot with Pentbox and Kali Linux
date: 2020-06-28 21:27 -0400
---
In the past few years Cyber Security has become a hot topic and seems to be a issue with allot of companies failing to secure their networks.  I wanted to experiment with my own defensive measure on my home network and decided to set up a honeypot.  Honeypots are at their core just mimics of likely targets of cyber attacks that seem alluring for potential hackers to checkout on a network.  These are the steps I took to run a simple honeypot that is built into the tool Pentbox on Kali Linux.

I wanted to explore the different types of honeypots and the first one I came across was the one built into [Pentbox](https://sourceforge.net/projects/pentbox18realised/files/latest/download).  I can't speak to how safe it is to expose this honeypot to the actual internet but it hasn't been updated since 2014 so I wouldn't recommend it, this is just for research purposes on a protected network.

To follow along with this guide you need all you really need is [Kali Linux](https://www.kali.org/downloads/).  I decided to use Kali because it's really easy to download the virtual image or run the live ISO and it comes with allot tools you might use to test a honeypot.

First we need to download the [Pentbox](https://sourceforge.net/projects/pentbox18realised/files/latest/download) code
```
wget -O pentbox.tar.gz https://sourceforge.net/projects/pentbox18realised/files/latest/download
```
<br/>
<div class='text-center img-fluid'>
  <img src="{{ site.baseurl }}/assets/img/pentbox/pentbox-download.png" alt='Pentbox downloaded via wget' />
</div>

I used the `-O` arguemnt to set the filename because it saves as `download` if you don't.  After moving the file go to the folder you want to store the code in and execute the following to unpack it.
```
tar xvf pentbox.tar.gz
```

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/pentbox/pentbox-unpack.png" alt='Pentbox unpacked in terminal' />
</div>


Change into the unpacked `pentbox` directory and run
```
./pentbox.rb
```

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/pentbox/pentbox-menu.png" alt='Pentbox menu screenshot' />
</div>

This is the main menu and from here we want the `Network Tools` option so we enter `2`.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/pentbox/pentbox-web-menu.png" alt='Pentbox secondary menu screenshot' />
</div>

Then next we want the `Honeypot` option so enter `3` and then select the `Fast Auto Configuration` by entering `1`.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/pentbox/pentbox-honeypot-menu.png" alt='Pentbox honeypot options' />
</div>

That's it there should now be a honeypot running on port 80 of the machine and showing the information of any computer that try's to visit the open port is output to the terminal.

If anyone visits the open port they will see this message.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/pentbox/pentbox-page.png" alt='Screenshot of when a visitor goes to the open port' />
</div>

The terminal will show what some pieces of info like IP address and port that is trying to visit as well as the request that was sent.

<div class='text-center'>
  <img src="{{ site.baseurl }}/assets/img/pentbox/pentbox-log.png" alt='Pentbox log output to terminal' />
</div>


I plan on testing the honeypot further by running some things like nmap or nikto on it to see what it looks like when you are actively being scanned or attacked.  Also I'm going to check out the advanced settings option that is in the menu and see what options you can configure.  

I though this was a good way to integrate into the honeypot world, I wouldn't rate this one very high on the list of impressive honeypots but it works and gives you a code base to look over or play around with it.  I would definitely personalize the home page with something further like a login page or something that allows interaction and can be logged so whoever stumbles into the honeypot will leave some information as to what their intention is.  Honeypots can be a great way to keep an eye on your network for unwanted access as indicator you need to take a closer look at your systems and I plan exploring the other options out there in the future. 
