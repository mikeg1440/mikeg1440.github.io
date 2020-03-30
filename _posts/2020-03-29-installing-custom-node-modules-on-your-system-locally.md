---
layout: post
title: Installing Custom Node Modules On Your System Locally
date: 2020-03-29 22:15 -0400
---
<div class='text-center'>
  <img src='https://s3.amazonaws.com/usdphosting.accusoft/wp-content/uploads/2019/05/node-js-logo-sized.png' alt='Node.js logo' class='mx-auto' />
</div>
___

I recently found myself developing a Node.js CLI program that I wanted to test installing and using it locally.  This post is going to look at a few ways you can go about doing that.

The name of the program I was developing and the one being used in examples and screenshots is called corona-cli, it's CLI that displays the number of cases for countries around the world as well as the states.  It's a forked repo from [Ahmad Awais's Corona-CLI](https://github.com/ahmadawais/corona-cli) that just adds a interactive CLI menu, a filter for specific states and some other aesthetics.  Because I just did this as a side project to keep an eye on the ongoing pandemic and the fact that corona-cli is already published as a NPM package I only intended on using it for my personal systems.  But if you want to check out the code that I added to it you can see it [here](https://github.com/mikeg1440/corona-cli).

<br /><br /><br />


## Using NPM link

NPM comes with a very handy command to create a symlink to any local packages so that you test them out in the global context of your system and is the easiest way.  

All you need to do is run `npm link` from within the package folder which will create a symlink in the global folder `{prefix}/lib/node_modules/<package_name>` to the current folder's package.  The `{prefix}` could be slightly different for each configuration, mine was `$HOME/.nvm/versions/v12.16.1/` because I use NVM but to see the diffent possible locations check out the [NPM Docs](https://docs.npmjs.com/misc/config).  The link command will also link any files bins in the package to `{prefix}/bin/{name}`.  Also worth noting is the `<package_name>` will be taken from the name set in the `package.json` file so make sure you have that set before running the command.

After changing into the package directory `corona-cli` in this case for my CLI program and running `npm link` you should see a output similar to this, obviously your file paths with vary but this should give you general idea of what it looks like when the command is successful.

<div class='text-center'>
  <img src='https://i.imgur.com/v5dHz8m.png' alt='Screenshot of running npm link' class='mx-auto' />
</div>

As you can see it created two bin files, one called `corona` and the other `corona-cli` that both link back to the current packages `index.js` file.  This is because the `package.json` for the current folder has two entries that look like this.

```
"bin": {
                "corona": "index.js",
                "corona-cli": "index.js"
        },
```

This gives me the ability to run `corona` or `corona-cli` from anywhere in the terminal to start up the CLI program.

<br /><br />

### Linking to another project

If you wanted to then use your newly installed package within another project you can simply run the link command again from inside the projects folder, specifying the name of the package.

So for this example after changing into the new project directory I could run `npm link corona-cli` and it will create a new symbolic link from the globally-installed packge to the `node_modules` folder.

<div class='text-center'>
  <img src='https://i.imgur.com/AE6RuUO.png' alt='NPM link globally installed package to local node_modules' class='mx-auto' />
</div>

<br /><br />

### Removing the package

To uninstall the package you would just remove it like any other global package with the `uninstall` command or any of it's aliases like `remove`, `rm`, `r`, `un` or `unlink`.

<div class='text-center'>
  <img src='https://i.imgur.com/klVXBL5.png' alt='NPM remove globally installed package' class='mx-auto' />
</div>

<br /><br /><br />

## Other ways of doing it

Although I really liked the streamlined way `npm link` worked I came across a few other ways that you can do the same thing essentially and are worth noting a few differences.

<br /><br />

### Install with direct path

You can just run `npm -g install /absolute/path/to/package/corona-cli` and that will install it globally or from within a project folder `npm install /absolute/path/to/package/corona-cli` will modify the projects `package.json` to include the package as a dependency.  This may or may not be what you want depending on if you actually intend on publishing it or just keeping it local on your system.

The big difference between using this method and the link command is that `npm link` will not alter the `package.json` file in the current directory and it will not run any of the pre-install or post-install hooks that you can set with [npm-scripts](https://docs.npmjs.com/misc/scripts) if your not familiar with them you should briefly look them over to just to get an idea of what options you can set in your package.json file.

<br /><br />

### Install with a tarball

You can also pack everything up in a installable tarball by running the pack command inside the root folder of the package you want to pack up.  It will generate a `.tgz` file that contains all everything you need to install the package just by running `npm pack` which will produce a file called `<package_name>-<version>.tgz`.  
<div class='text-center'>
  <img src='https://i.imgur.com/fnn3c7R.png' alt='Screenshot of npm pack command output' class='mx-auto' />
</div>

You can then install the package by running `npm -g install <package_name>-<version>.tgz`.


## Wrapping up

You can see all the specifics of what `npm link` does and more by checking out the [NPM Docs](https://docs.npmjs.com/) but the main overview of the differences between the three ways we looked at for using your own node modules on your local system for testing and usage are

 - `npm install /absolute/path/folder` will run the preinstall/postinstall hooks, but npm link will not.
 - `npm link` uses the global NPM space, `npm install /absolute/path/folder` does not. npm link creates a symlink to the package in the global space, and then when you call npm link from another project directory, it creates a symlink not directly to the package, but rather to the global symlink. This is an important differences if you are using different global node.js
 - `npm install /absolute/path/folder` will alter package.json, npm link does not.
