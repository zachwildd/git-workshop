# Git Workshop

Git is a distributed version-control system for tracking changes in source code during software development. It is designed for coordinating work among programmers, but it can be used to track changes in any set of files. (https://en.wikipedia.org/wiki/Git)

### Installation
If you don't have git installed you can install by following the tutorial for your operating system.

| Operating System | Tutorial |
| ------ | ------ |
| Windows | https://www.atlassian.com/git/tutorials/install-git#windows |
| OSX / Mac | https://www.atlassian.com/git/tutorials/install-git#mac-os-x |
| Debian / Ubuntu | https://www.atlassian.com/git/tutorials/install-git#linux |

### Github
Github is a service for hosting git repositories. Sign up for an account if you haven't already because you'll need it for this workshop. (https://github.com/)

### Forking the Original Project
We are going to fork the original project on Github. The project that we'll be working with is a simple website. Forking allows you to create a copy of a repository and make changes to it without affecting the original project. Now that we are logged into Github, go to the original project at https://github.com/zach-wild/git-workshop.

### Downloading this repository
First make a new directory to initialize your git project in.
```sh
$ mkdir git-workshop
$ cd git-workshop
$ git init
```
Now we have intialized a git project in our directory. The next step is to add a remote. We will call this remote origin and the url will be that of the fork we just created. A remote is just a reference to a git repository.
```sh
$ git remote add origin https://github.com/<your-username>/git-workshop.git
```
Now that we have referenced our newly created repository we can download it do our local machine and start making changes to it. The fetch command is used to download all of the git objects from the repository; more detail (https://www.atlassian.com/git/tutorials/syncing/git-fetch).
```sh
$ git fetch origin
```
Now that we have all of the changes we want to incorporate them into our local git repository. The merge command joins the git history of two branches, we'll talk more about branches later. In our case this will be the origin/master branch we fetched and the master (default) branch that was created when we ran git init.
```sh
$ git merge origin/master
```
Now we have a local copy of this repository that we can make changes to. There is also an easier way to do all of this which is to instead clone a repository.
```sh
$ git clone https://github.com/<your-username>/git-workshop.git
```
This will perform all of the steps we just did with one command. But now lets makes some changes to the project. 

### Making some changes to the website
The whole point of git is to keep track of the changes made to a project. We'll run a quick command to look at the history of changes made to this project.
```sh
$ git log
```
We'll see that we have two commits. Each commit has a hash (if you want more information on this read this (https://blog.thoughtram.io/git/2014/11/18/the-anatomy-of-a-git-commit.html) , author, date, and commit message. We can get more information about each commit by adding additonal parameters to the log command (https://git-scm.com/docs/git-log). We'll run one version which will give us some high-leve information about what was done in each commit.
```sh
$ git log --stat
```
Now that we can see what files we're changed during each commit, lets do something similar. Open the project in your favorite text editor and we'll change some of the files. Feel free to make whatever changes you'd like. I'm going to change the joke on the jokes page from
```html
<div id="navbar">
      <a href="./index.html">Home</a>
      <a href="">Jokes</a>
  </div>

  <h1>This page is under construction.</h1>
  <br>
  <h1>... Watch your head.</h1>
</body>
```
to
```html
<div id="navbar">
      <a href="./index.html">Home</a>
      <a href="">Jokes</a>
  </div>

  <h1>That was a great joke.</h1>
</body>
```
Now that we've made whatever changes we want to save these changes. If we run the status command we can get an idea of what has changed about the project since the last commit.
```sh
$ git status
```
We'll see files that have been changed and it also mentions that no changed have been added to commit. We can add changed to commit with the add command. 
```sh
$ git add jokes.html
```
Now if we run git status again we'll see that it says that we have modified the jokes.html file and that this change is ready to be committed. So now we are going to create a commit. 
```sh
$ git commit -m 'Included better jokes'
```
Now if we check our git history we will see that our newest commit is included.
```sh
$ git log --stat
```
Now that we have made some changes to our project we would like to sync up our local and remote repositories. We are going to push the changes we made, the commit, to our remote repository. Remember that we created a reference to our remote repository with the origin remote. Similar to how we donwloaded from this remote we can upload to it.
```sh
$ git push origin master
```
We should recieve a permission denied error.

### Configuration
We are going to follow this tutorial to configure git to work with your github account. (https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
After this we need to change the remote url.
```sh
$ git remote set-url origin git@github.com:<your-username>/git-workshop.git
$ git push origin master
```
Now we have pushed our changes that we made locally to our Github repository.

### Reverting changes
So now that we have added changes we are going to work on removing changes that we've made. Say we want to remove the changes from the commit we just created. 
```sh
$ git revert HEAD~1
$ git push origin master
```
So one of the things that we've been doing that we shouldn't be doing is making all of our commits on the master branch. We'll talk a bit about branches and then we'll all work together on the website.

### Branches
If you want a more technical description go here, (https://git-scm.com/book/en/v1/Git-Branching-What-a-Branch-Is). Branches can be visualized pretty easily. A way to think about branches are as a list of commits. In this picture each dot represents a commit and an arrow represents a merge.
![](http://www.rittmanmead.com/blog/content/images/2017/01/gitflow.png)
We are going to recreate a portion of this diagram in our own repository. We are going to focus on recreated the master, develop, and feature branches. First lets create the develop branch. develop will be the name of the new branch we are creating and it will be based on the master branches history.
```sh
$ git checkout -b develop master
$ git push origin develop
```
Now we are going to create a couple of commits in our develop branch similar to the diagram.
```sh
# change some file(s)
$ git add .
$ git commit -m 'Added 1st commit to develop'
# change some file(s)
$ git add .
$ git commit -m 'Added 2nd commit to develop'
```
Now we are going to create a feature branch off of our develop branch. We are going to make some changes in this branch and then merge them back into the develop branch once we are done with our feature.
```sh
$ git checkout -b feature-a develop
# change some files
$ git add .
$ git commit -m 'Added some jokes'
# change some files
$ git add .
$ git commit -m 'Changed the style of jokes'
# push changes from our feature branch to our repository
$ git push origin feature-a
```
Now that we have finished our feature, we are going to merge our feature into the develop branch. We created two commits on the feature branch but we are going to merge them into a single commit as far as the develop branch is concerned. This will make our feature appear as a single commit in our develop branch.
```sh
$ git checkout develop
$ git merge feature-a --squash
$ git add .
$ git commit -m 'Added feature a'
$ git push origin develop
```
Finally, we can go to Github and create what is called a pull request. A pull request is a formal request to include your changes. We are going to create a pull request in our own fork to request to pull the changes from our develop branch into our master branch.

### Group Project
We all know how to fork a project, create branches, commit changes, push them to repositories, and make pull requests. What we're going to do now is all contribute as a group to a single website. I have a Github pages site hosted at (https://zach-wild.github.io/git-workshop/index.html) with the files from this project. The goal is for everyone to add their own changes to the site in their own copies of the repository, push them to their fork on github, and make pull requests to the original Github repository from their forks. 
The first step for us is to get our local repository in the same state as the origin repository.
```sh
# add a remote reference to the original repository
$ git remote add source git@github.com:zach-wild/git-workshop.git
# make sure we are in the master branch
$ git checkout master
# find the most recent commit from the remote repository
$ git log
# reset the master branch to point to this commit
$ git reset --hard d6bceea09
# push the new master branch with this commit at the head to our fork
$ git push origin master --force
# if any changes are made to the remote repository we can pull them
$ git pull source master
# remove our develop branch
$ git branch -D develop
# create a new develop branch
$ git checkout -b develop master
$ git push origin develop
# create your feature branch
$ git checkout -b feature-<your-name> develop
# make changes in your branch
# ...
# push changes to your fork
$ git add .
$ git commit -m 'Finish feature <your-name>'
$ git push origin feature-<your-name>
# make pull request on github
```
So I ask that you make your changes to the website in the form of creating a new web page. This is in order to avoid merge conflicts. I'll go through an example of what I mean.
```html
  <div id="navbar">
    <a href="">Home</a>
    <a href="./jokes.html">Jokes</a>
    // you should add a link to your page in the index.html like this
    <a href="./zach.html">zach</a>
  </div>
```
You should also create a new html file for yourself based on the jokes page. Change whatever you want and make this page your own. You can add your own css files or whatever you'd like to do.
```html
<html>

<head>
  <title>Zach's Page</title>
  <link rel="stylesheet" type="text/css" href="./style.css">
</head>

<body>
  <div id="title">
    Github Workshop Jokes
  </div>

  <div id="navbar">
      <a href="./index.html">Home</a>
      <a href="">Jokes</a>
  </div>

  <h1>This is zach's page</h1>
</body>

</html>
```
Once you've done this make a pull request and I'll accept them as they come in. You'll be able to see the changes reflected at (https://zach-wild.github.io/git-workshop/index.html).
