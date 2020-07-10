---
layout: post
title: Hosting a React App on GitHub with GitHub Pages
date: 2020-07-10 12:28 -0400
---
React is great library that allows you to build reusable components and provide a responsive front end interface for users.  The best part is getting your react apps hosted is easier than you think, if you already have a GitHub account then your already set!  That's right, you can use [GitHub Pages](https://pages.github.com/) to host your apps.  This post is looking at exactly what you can do to get your app running.

You need a few things to following along with this guide.
  - [X] [GitHub Account](https://github.com/join)
  - [X] [Git](https://git-scm.com/) installed
  - [X] [Node.js](https://nodejs.org/) installed
  - [X] [NPM](https://www.npmjs.com/) installed

If you already have a app your using or are familiar with creating apps with `react-app` package then you can go ahead and skip to the
[Hosting with GH-Pages](#hosting-with-gh-pages)
## Repo Setup

For the sake of this post I will use the generated code to show the process from start to finish with a app.  The first thing you want to do is create the app so pick a name for it(in this example `timer`) and run
```
  npm use react-app timer
```

<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/create_app.png" alt='Screenshot of the last part of the output from running the command `npm use react-app timer`' />
</div>

The next part is create a [new GitHub Repo](https://github.com/new), you need to set it as public unless your using a paid GitHub account. Whatever you set the name of the repository to be will be the extension to get to your react app when hosted, so in this example the I set the name to `Timer` so the end URL will be `https://YOUR-GITHUB-USERNAME.github.io/Timer`.

<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/create_repo.png" alt='Creating a new repository on github' />
</div>

Make sure your remember the web address of repo or copy it so we can configure git and upload our files.  Change into the app folder we just created and run `git init` to initialize git.

To connect the GitGub repo we just created we need to set the remote origin setting for git locally.  Run these commands substituting your URL in place.

```
git remote add origin https://GITHUB-USERNAME.com/REPO-NAME
```

Next we need to commit the files so we can push them up so run
```
git add ./*
git commit -m 'Initial commit'
git push --setup-stream origin master
```
<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/git_add.png" alt='Staging files for commit with git' />
</div>

Then commit the files with some sort of message
```
git commit -m 'Initial commit'
```
<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/git_commit.png" alt='Commiting files with git' />
</div>

And now just before we push up to GitHub we need to set the master branch to push to the origin.
```
git remote add --set-upstream origin master
```
<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/git_set-upstream.png" alt='Setting the remote origin for the master branch with git' />
</div>

## Hosting with GH-Pages

Getting the GitHub Pages set up starts with adding the `gh-pages` package as a dev dependency using npm by running `npm install --save-dev gh-pages`

Next open up the `package.json` file so we can add some settings.  The first thing we need to add as a main setting is the `hompeage` key and set it to the URL that we specified earlier with the name of the repo, so add this to the file
```
"homepage": "http://GITHUB-USERNAME.github.io/Timer",
```

Then we need to add some commands to run on deploy and just before deploy runs, so in the `scripts` section add this
```
"predeploy": "npm run build",
"deploy": "gh-pages -d build"
```
<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/package-json.png" alt='' />
</div>

This will build the app before deploying and then build the site with the gh-pages package.  We need to commit and push the changes up to GitHub using the same process outlined in the first section of the post.  Then go ahead and run
```
npm run deploy
```
<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/npm_deploy.png" alt='Output from running `npm run deploy`' />
</div>

And that's it!  If you open a browser and navigate to `https://GITHUB-USERNAME.github.io/REPO-NAME` and you should see the app working just like it should!

<div class='center-text'>
  <img src="{{ site.baseurl }}/assets/img/react/hosted_app.png" alt='React app hosted using GitHub Pages' />
</div>
