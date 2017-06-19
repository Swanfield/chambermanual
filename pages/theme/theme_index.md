---
title: Getting started
keywords: sample homepage
tags: [getting_started]
sidebar: theme_sidebar
permalink: theme_index.html
folder: theme
summary: How to install, set-up and quickly get started with this theme.
---

## Introduction

You will need:

* A [GitHub](https://github.com/) account
* Ruby for Windows
* [Git for Windows](https://git-scm.com/), [TortoiseGit](https://tortoisegit.org/), and/or [GitHub Desktop](https://help.github.com/desktop/guides/getting-started/installing-github-desktop/)
* Jekyll 
* Bundler
* A UTF-8 compatible text editor like [Sublime Text](https://www.sublimetext.com/)



## Install The Software

The easiest way to install and manage the software required for this project is to first install [Chocolatey](https://chocolatey.org/).


### Install Chocolatey

[Install Chocolatey](https://chocolatey.org/install)

At the time of writing, copy and paste this to the command line (with administrator privliges):

```shell
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```


### Install a text editor

Any text editor that can handle UTF-8 encoded text files is recommended (unfortunately this does not include Notepad).  There are many free editors and IDEs that can be used.  [Sublime Text](https://www.sublimetext.com/) is a commonly recommended editor which can be downloaded with an unlimited free trial but a license must be bought for continued use.  At the moment, I am going to suggest trying it out.

Download Sublime Text 3 from here: https://www.sublimetext.com/3 or:

```shell
choco install sublimetext3.packagecontrol -y
```



### Install Ruby for Windows and Ruby Development Kit

At the command line enter:

```shell
choco install ruby -y --version 2.3.0
choco install ruby2.devkit -y
```

(I haven't been able to get Ruby 2.4 to work with this theme.)

You now need to initialise and install the Ruby Development Kit.

```shell
cd C:\tools\DevKit2
ruby dk.rb init
```

Now edit config.yml in the DevKit2 directory (using Sublime Text that you installed earlier) and add the following at the bottom if it isn't already there.:

```shell
 - C:/tools/ruby23
```
Then finish the install

```shell
ruby dk.rb install
```


### Install Bundler and Jekyll

At the command line enter (you will need to open a new window):

**NB** Your firewall might throw up a warning about the the software connecting to the internet.  You have to click OK fairly promptly or it will time out and you will have problems getting it to work again.

```shell
gem install bundler jekyll
```


### Install Git for Windows

You can pick anyone of the following or all three:

```shell
choco install git.install tortoisegit -y
choco install github 
```
I have not used [GitHub Desktop](https://desktop.github.com/), and have not been able to install it with chocolatey, so will not document it here until I have.



## Set up Git

You will need to synchronise your newly installed version of git with GitHub.  I have not used [GitHub Desktop] so will not document it here until I have.  [TortoiseGit] uses the same settings as [Git for Windows].  You can think of it simply as a Windows Explorer add-on for Git.  I will focus on [Git for Windows] as I am most familiar with this but if you really like using your mouse and Windows Explorer then pretty much anything that can be done on the command line with [Git for Windows] can be done with [TortoiseGit].


### Open the Git Shell application

1. I would suggest creating a folder called git in your Documents folder in Windows Explorer (or in your public documents folder);
1. Right click on the folder;
1. Select "Git Bash here", and a shell window will open.  This shell window uses Unix/Bash style commands rather than Windows style commands.
1. Set a Git username:

   ```shell
git config --global user.name "My Name"
   ```

1. Confirm that you have set the Git username correctly:

   ```shell
git config --global user.name
> My Name
   ```

1. Set an email address in Git. 

   ```shell
git config --global user.email "email@example.com"
   ```

    To keep your email address private, you can use the address username@users.noreply.github.com, replacing username with your GitHub username.  You will need to [tell GitHub to keep your email address private](https://help.github.com/articles/keeping-your-email-address-private/).

1. Confirm that you have set the email address correctly in Git:

   ```shell
git config --global user.email
> email@example.com
   ```

1. [Link the email address to your GitHub account](https://help.github.com/articles/adding-an-email-address-to-your-github-account/), so that your commits can be attributed to you and displayed in your contributions graph.

1. Ensure that you commit Unix style line endings.  Windows uses different characters for line endings to Unix but most web servers run on Unix so it is good to ensure that your commits are Unix friendly.

   ```shell
git config --global core.autocrlf input
   ```



### Set up authentication with GitHub.

When you download or "clone" a repository from GitHub you need to connect securely and authenticate yourself.  This is done automatically with a protocol called SSH but it still needs to be set up.

If you have GitHub Desktop installed, you can use it to clone repositories and not deal with SSH keys. 


#### Generate a new SSH key

Generate an SSH public-private keypair on your computer.  You will add the public key to your GitHub account later.

1. Open Git Bash.
1. Paste the text below, substituting in your GitHub email address.

   ```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

    This creates a new ssh key, using the provided email as a label.

   ```shell
Generating public/private rsa key pair.
   ```

1. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

   ```shell
Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]
   ```

1. If other people share your computer with you then, at the prompt, type a secure passphrase. You will need to type in this passphrase everytime you connect to GitHub.  If you are the only person who uses your computer then you can probably leave this blank.

   ```shell
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
   ```

#### Add your SSH key to the ssh-agent

ssh-agent is a program that manages SSH keys for you.

1. Ensure the ssh-agent is running:

   ```shell
eval $(ssh-agent -s)
> Agent pid 59566
   ```
   
1. Add your SSH private key to the ssh-agent.

   ```shell
ssh-add ~/.ssh/id_rsa
   ```

#### [Add the SSH key to your GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
  1. Copy the SSH key to your clipboard.
  
       ```shell
clip < ~/.ssh/id_rsa.pub
        ```
 
  1. Go to [GitHub]
  1. In the upper-right corner of any page, click your profile photo, then click Settings.
  1. In the user settings sidebar, click SSH and GPG keys.
  1. Click New SSH key or Add SSH key.
   1. In the "Title" field, add a descriptive label for the new key. e.g. "Work PC."
   1. Paste your key into the "Key" field.
   1. Click Add SSH key.
   1. If prompted, confirm your GitHub password.

#### Used a Passphrase? Auto Launch ssh-agent with Git Bash

ssh-agent runs each time you connect to GitHub.  However, if you have used a passphrase then you can have it run in the background when you start Git Bash.  This way, it will only ask for your passphrase once - at the start of your session - rather than each time you connect to GitHub.

##### Edit .profile or .bashrc

Find one of the following files in your user root directory (e.g. C:\Users\YourUsername\ - this directory is shortened to ~/ in Unix/Bash).

~/.profile

or

~/.bashrc

You can right click the file and open it with Sublime Text or you can open Sublime Text and drag and drop the file into it.

Copy and paste the following into the file that you selected and then save (ctrl-s).

```
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

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


## Set up TortoiseGit

In windows explorer, right click on the folder for your repository.  Go to the TortoiseGit option, then select settings at the bottom of the new pop-up menu.  Select Network on the left hand side of the settings window that appears.  Now you need to set the ssh client in the box on the bottom right to:

'''
C:\Program Files\Git\usr\bin\ssh.exe
'''

This ensures that TortoiseGit pushes and pulls with the same settings as the command line.



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



## View Your Documents Locally Before You Push

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

When you run Jekyll through Bundler two things are done.

- First, Jekyll runs through all the files in the repository and generates static web pages from them.  It puts these pages in the ```_site/``` folder.
- Secondly, a wee web server is run to serve those pages to your web browser.

You don't have to use the web server to access the pages.  You can open them in your browser directly and they should work as intended.  

