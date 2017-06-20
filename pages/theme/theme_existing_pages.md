---
title: Editing an Existing Manual
keywords: editing
tags: [editing]
sidebar: theme_sidebar
permalink: theme_existing_pages.html
folder: theme
summary: How to start editing an existing manual.
---

# Working with a repository

## By yourself

If you are the only person working on a repository then you can pretty much do what you want how you want.  However, if you are working on multiple computers or with other people...

## With Other People

When working with other people on the same repository then it is best to have a standardised work flow:

1. Pull the changes other people have made (or you have made on other machines),
1. Make your changes,
1. Commit your changes regularly and frequently with helpful commit messages.
1. Push your commits regularly (the frequency depends on how your team works).

With this particular project you don't want to push changes too many times per day because Github will only update your Github Pages website a few times per day and each push will cause a website update.

More complicated projects are managed slightly differently.  Ideally everyone would commit and push their work regularly and frequently.  However, they would work on their own branches until their work was ready for integration (merging) with the master branch.  This way, the most important master branch would remain stable at all times and wouldn't be subjected directly to frequent or potentially dodgy commits.

## Update Repository

The very first thing you do, before you do ANYTHING else is update the repository with changes that other people may have made to it.  If you do not update it first then you may have conflicts between your work and their work that you will have to resolve later.  This is best avoided!

### Open Git Bash Prompt

There are two ways to open up the Git Bash command prompt.  You can:

1. Open "Git Bash" from the start menu or
1. Right click on a folder within windows explorer and select "Git Bash here"

If you choose the second option then your command prompt will be in the right place otherwise you will need to:

### Navigate to the repository

   ```shell
cd Documents/git/manuals
   ```

or

   ```shell
cd ~/Documents/git/manuals
   ```


### Update Repository

The very first thing you do,

   ```shell
git pull
   ```

## Edit Pages

Open any pages that you want to edit in Sublime Text.  Make your changes. 

## Create New Pages

You can:

1. copy and rename an existing page;
1. create a new document in windows explorer with an appropriate name;
1. at the command line, go to the directory where you want the new document to be, type "touch " followed by the name of the new document.

### Preview Changes

You can preview your changes before you commit them by running:

```
bundle exec Jekyll serve
```

in the repository's root directory (manuals).  This command creates a local copy of the website AND runs a wee web server.  You can view the site here:

[http://127.0.0.1:4000/](http://127.0.0.1:4000/)

### Commit Changes

If you are happy with your work, you need to add it to your up coming commit.  The following command will add all edits AND new files.

```
git add .
``

You then commit your changes to the repository.

```
git commit -m "Commit message within these quotes."
``

Edits only:

```
git commit -a -m "Commit message within these quotes."
``

The -a above adds all files to the commit that are already present in the repository.  It will not add any NEW files to the commit.