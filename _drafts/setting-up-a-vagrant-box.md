---
layout: post
title: Setting up a Vagrant Box
---

As a longtime user of Virtualbox I have come to love virtual environments and was thrilled to find out a about a new tool called Vagrant that allows you to setup environments with a simple Vagrantfile.  This post will go through the setup of a Ubuntu 18.04 box front a Ubuntu host machine.

## Prerequisites
First off we need a provider, which is just the location the virtual environment is going to run in.  Vagrants default is to use Virtualbox so if you don't already have it installed you need to do so first with `sudo apt install virtualbox`.  Next we obviously need to install Vagrant itself so simple run `sudo apt install vagrant`.  Verify vagrant is installed by running `vagrant --version` and you get the current installed version number.


## Create a box

We are going to first create a directory that will store our Vagrantfile and whatever we want to include in our virtual machine.  

```
mkdir -p vagrant/tutorial
```

Now we need a Vagrantfile and the easiest way is to use the command
```
vagrant init ubuntu/bionic64
```

This creates our new Vagrantfile in the current directory and sets the type of machine we are using as well as the configuration and provisions for it as well.

## Running the box

So far we have just created a simple Vagrantfile that tells it to create a virtual machine using the Ubuntu Bionic 64 bit image and in this case with use Virtualbox to provision the machine.  To start the machine up we need to run
```
vagrant up
```

This will look in the current directory for the Vagrantfile and use that to configure the machine.
<img of command screenshot>
You can then see the machine's status by running
```
vagrant status
```

## Connecting to the box

As you might have noticed we don't have any way to interact with the newly created virtual machine, Vagrant conviently sets up the machine with a ssh key that we can use to ssh into it and the Vagrant CLI tool gives us a easy way to securely connect to the environment using the following command
```
vagrant ssh
```

Now we are connected through ssh to the our virtual environment and we can't start doing whatever we want to with our new machine.

When we are ready to shut it down all need to do is run this simple command
```
vagrant down
```

And that's all it takes!  You have just set up your first vagrant box, keep in mind we did nothing other create a simple box and connect to it.  Vagrant is a very powerful tool with many uses such as software testing, development as well as other things like if you have a untrusted program you want to run and see what it does then you can just spin up your box and run the program within the virtual environment.
