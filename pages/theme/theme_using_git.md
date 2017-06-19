---
title: Using Git
keywords: Using Git
tags: [getting_started]
sidebar: theme_sidebar
permalink: theme_using_git.html
folder: theme
summary: How to use Git with this theme.
---


## Git and The Repository

### What is Git?

So, we have spent all this time setting up Git so that it will work with GitHub but what exactly is Git?

Git is a version control system which allows a team of people to work together, all using the same files. It helps the team cope with the confusion that tends to happen when multiple people are editing the same files.

Git is very sophisticated but we are using it in a very simple way: *to download and upload files contained in our repository.*

#### Initial Work Flow

We first:
- download the repository from GitHub (**clone** the GitHub repository).
- We then make changes to it (**edit**),
- save the changes locally to the repository (**commit**),
- and upload the changes to GitHub (**push**).

GitHub will update the website automatically.  It is that simple.

#### Subsequent Work Flow

When more than two people are working on the same repository then, *before they start editing*, they download any changes that have been made since the last time the worked on the repository (**pull**).  They can then **edit**, **commit**, and **push** their changes.

- Doing a pull before doing anything else is very good practice and keeps life simple.

#### Simultaneous Editing

If you have made changes, and someone else has made changes at the same time, then Git allows you to merge them together easily.  Merging, however, is beyond the scope of this document as it should rarely be an issue for a simple repository, with few contributors, like this one.


### Download this Repository

You will need to download [this repository](https://github.com/Swanfield/manuals) from GitHub.

1. If you haven't already, navigate to the parent folder of the git folder you created earlier (~/documents/ i.e. C:\Users\UserName\Documents\).
1. Right-click the git folder and select "Git Bash here"
1. If Git has been setup correctly then all you need to do is copy and paste this to the Git Bash window's command line:

    ```
git clone git@github.com:Swanfield/manuals.git
    ```
    This will create a manuals folder (~/git/manuals/).

1. Enter the folder:

    ```
cd manuals
    ```

1. You can view the contents of the folder at the command line:

    ```
ls
    ```

    or, for a more detailed view:

    ```
ls -la
    ```

    or you can view it's contents in the normal way with windows explorer.



### Branches

You will see that there isn't much there initially.  That is because you are looking at the master branch and we aren't using it for much at the moment.  The documentation files are all in a special branch, called gh-pages, used specifically by GitHub Pages.

#### What are Branches?

The actual inner workings of Git is not for the faint of heart.  Essentially though git keeps a compressed record of every file - and the changes made to every file - in your repository (this record is kept in the .git folder in every repository). Branches are basically separate repositories that live in the same folder but not at the same time. When you move to one branch from another (checkout a branch) all the files in the repository's folder are removed and the new branch is created dynamically from git's compressed record.

You can see this in action now.

1. Open the repository's folder in Windows Explorer.  You will see only a few files and folders.
1. Bring up the Git Bash window and move it so that you can see the contents of the repository's folder behind it.
1. Now, in the terminal type:
```
git checkout gh-pages
```
1. You will briefly see all the files from the repository's master branch disappear and be replaced by all the files from the gh-pages branch (GitHub Pages branch).
1. If you want to see this again you can type:
```
git checkout master
```

As we don't plan to work on the gh-pages branch please ensure that it is the last branch that you have checked out.


## The Final Setup Steps

### View Your Documents Locally Before You Push

You can view your documents, and any changes that you have made to them on your own computer before you commit them and/or push them to GitHub.  This is actually a very good idea as even a small typo will cause you to make another edit, another commit, and another push.  GitHub will only build the website a limited number of times a day for you so it is best practice to check your changes before you commit (and especially before you push).


### Bundler

Earlier, we installed [Bundler](http://bundler.io/).  This is the magic sauce that lets us install and run all the software required to view our GitHub Pages website locally on our own computer.

Use Bundler to install all the needed Ruby gems:

```
bundle install
```

Then always use this command to build Jekyll:

```
bundle exec jekyll serve
```

**NB** If this doesn't work then you probably haven't installed the Ruby Development Kit correctly which prevents "bundle install" from installing all the required gems.

As this is pretty long, we will create a short alias, called "jserve" for this command later.

You will now be able to view your documentation at:

[http://127.0.0.1:4000/](http://127.0.0.1:4000/)

If you are using an editor that allows you to preview Markdown files then you will see between this theme and Jekyll (the application GitHub and Bundler use to generate the webpages), your changes might not appear exactly as you had expected.

If you need to make changes then you will need to stop the jekyll server (ctrl+c) and restart it.  Restarting is easy though (although it will take about a minute or so).  All you need to do is press the cursor up key and your last command will appear in the Git Bash window, then hit enter to execute it.


### How it works

When you run Jekyll through Bundler three things are done.

 1. Jekyll runs through all the files in the repository and generates static web pages from them.  It puts these pages in the ```_site/``` folder.
 2. A wee web server is run to serve those pages to your web browser.
 3. Jekyll watches for file changes.  Every time a page is saved, Jekyll will rebuild ALL the pages in the ```_site/``` folder.  This will take a while - at least the same length of time the initial build took.  Please also note, Jekyll only watches files that it knows about.  If you create a new page then Jekyll will not updated itself automatically and will need to be restarted.

You don't have to use the web server to access the pages.  You can open them in your browser directly and they should work as intended.  

### Watch Errors

On Windows, when a file changes, the site folder isn't rebuilt due to a problem with the GitHub gem (a GitHub Metadata error is produced).  At the moment this has been solved with the following environment variable set in _config.yml:

```
github: [metadata]
```

I do not know yet if this has any unitended consequences elsewhere.