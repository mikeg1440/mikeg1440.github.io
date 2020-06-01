---
layout: post
title: Run tests for React.js App with Github Actions
date: 2020-05-31 23:05 -0400
---
Github is a great platform for coders were you can push your code to repositories, clone other peoples code and collaborate on different coding projects.  One of the features that they other is called Github Actions, which is just a way to automate testing, building and deploying your code as your developing it.  Having recently started playing around with Actions I wanted to do a short article on how to set up a action that will run tests for a React.js project everytime you push to the repository.  

# Create/Pick a Repo

First things first is to create a repo for the code if doesn't exist already.  For this example I'm just going to use the generated code that you get by running `create-react-app` for a app called my news.  This is just a simple example to showcase how to set up the action and see it work because create-react-app with generate tests for the App component that it creates.

If your using a repo that already exists then you just have to have some test files created for your components as `<ComponentName>.test.js`.  First thing is to make sure that your tests are passing so run `npm test` to run the tests, you want to make sure your seeing some green and not red.

<div class='text-center'>
  <img src='https://i.imgur.com/JkAeoR7.png' alt='Screenshot of running npm test' />
</div>

After passing all the tests push to the code up to your repository to make sure everything is update to date.  Then go to the repository's page on Github and click on the actions tab.

<div class='text-center'>
  <img src='https://i.imgur.com/QCr90nx.png' alt='Github page showing the actions tab' />s
</div>

Now as long as you have not set up any actions yet your going to see a page like the one below where you get a couple suggestions for actions based on what kind of code you uploaded to the repo as well as a list of the other actions that you could implement for the repository.  We are just going to click on the Node.js option that says "Build and test a Node.js project with npm".  This is going to open a template for bulding and testing the project.  

<div class='text-center'>
  <img src='https://imgur.com/a/wfUCt6G' alt='Screenshot of Github actions page' />
</div>

You can actually just go ahead commit the template code, I wanted to implement code linting as well so this is how I set up my workflow file.

```

name: CI

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm install
    - run: npm run build --if-present
    - run: npm test

    - name: Run linters
      uses: samuelmeuli/lint-action@v1
      with:
        github_token: ${{ secrets.github_token }}
        # Enable linters
        eslint: true
        #prettier: true

      env:
        CI: true
```

This is just going to run eslint to lint the code in the project as well as run the test files on any push or pull request to the master branch.  You can customize this so that it only runs for specific branches that you want and if you want it to just run on a push request or whatever type of request you want.  

After commiting the code to your repo it is going to add a `main.yml` file in the `.github/workflow` path and create folders in your repository, you can actually just do this yourself locally if you want to without having to use the Github site and it will work the same.  If you do use the Github site then you will need to pull the new changes down to your local repo so don't forget to do that.  

Now whenever you push some code to the repo it is going to first run the workflow file and try to run the tests to make sure they pass, as well as the linter if you added the same code that I have.  You can view the status of this process by looking in the actions tab on Github after pushing some new code to your repo.  

<div class='text-center'>
  <img src='https://i.imgur.com/4Rnl1Yq.png' alt='Screenshot of Actions status page' />
</div>

You can click on a instance of a job and see the breakdown of the specifcs of the job and how long each took to run.  Each section has a dropdown where it gives you the console output if there was any as well.

<div class='text-center'>
  <img src='https://i.imgur.com/ikYkL28.png' alt='Action job breakdown' />
</div>

This is just scraping the surface of what Github Actions are capable of and can help you streamline your development process and automate some of the repetitive tasks that is part of your pipeline.   
