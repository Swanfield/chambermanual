---
title: Command Line Shortcuts
keywords: Command Line Shortcuts
tags: [shortcuts][command line]
sidebar: theme_sidebar
permalink: theme_command_line_shortcuts.html
folder: theme
summary: Shortcuts for use on the command line.
---


## Command Line Shortcuts for Git Bash

The following is for use with the Git Bash command prompt rather than the windows command prompt (cmd.exe).

### .bashrc

Command line shortcuts live in a file call .bashrc in the user's home directory.  i.e.

Windows:

```
C:\Users\UserName
```

Git Bash shell and Unix

```
~/
```

You can create a new document in Sublime Text and save it as .bashrc in your home directory.

### Aliases

Command line shortcuts in Bash are called aliases.  Here are a few that you can use in your .bashrc file:

```
alias aliasg='alias | grep'
alias la='ls -lA' #show hidden files
alias up='cd ..'
alias up2='cd ../..'
alias go='cd ~/Documents/git/manuals/'
alias gs='git status'
alias gc='git commit -m' # Commit with "message enclosed in quotes"
alias pull='git pull'
alias push='git push'
alias serve='bundle exec jekyll serve'
alias build='bundle exec jekyll build'
```

Aliases are not just for use as shortcuts.  They are also memory aids.  All you need to do is type 

```
alias
```

at the command line and all the aliases available to you will be shown.  If you have a lot of aliases you can search them with aliasg followed by some text that the alias will contain.  For example, to display all the aliases that contain git type:

```
aliasg git
```

