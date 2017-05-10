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

* A [GitHub] account
* Ruby for Windows
* [Git for Windows](https://git-scm.com/), [TortoiseGit](https://tortoisegit.org/), and/or [GitHub Desktop](https://help.github.com/desktop/guides/getting-started/installing-github-desktop/)
* Jekyll 
* Bundler
* A UTF-8 compatible text editor like [Sublime Text](https://www.sublimetext.com/)

## Install The Software

The easiest way to install and manage the software required for this project is to first install [Chocolatey](https://chocolatey.org/).

### Install Chocolatey

[Install Chocolatey](https://chocolatey.org/install)

At the time of writing, copy and paste this to the command line:

```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

### Install Ruby for Windows and Ruby Development Kit

At the command line enter:

```
choco install ruby -y
choco install ruby2.devkit
```

### Install Git for Windows


You can pick anyone of the following or all three:

```
choco install git.install
choco install tortoisegit 
choco install github 
```
I have not used [GitHub Desktop] so will not document it here until I have.

### Install Bundler and Jekyll

At the command line enter:

```
gem install bundler
gem install jekyll
```

### Install a text editor

Any text editor that can handle UTF-8 encoded text files is recommended (unfortunately this does not include Notepad).  There are many free editors and IDEs that can be used.  [Sublime Text](https://www.sublimetext.com/) is a commonly recommended editor which can be downloaded with an unlimited free trial but a license must be bought for continued use.  At the moment, I am going to suggest trying it out.

Download Sublime Text 3 from here: https://www.sublimetext.com/3

## Set up Git

You will need to synchronise your newly installed version of git with GitHub.  I have not used [GitHub Desktop] so will not document it here until I have.  [TortoiseGit] uses the same settings as [Git for Windows].  You can think of it simply as a Windows Explorer add-on for Git.  I will focus on [Git for Windows] as I am most familiar with this but if you really like using your mouse and Windows Explorer then pretty much anything that can be done on the command line with [Git for Windows] can be done with [TortoiseGit].

### Open the Git Shell application

1. I would suggest creating a folder called git in your Documents folder in Windows Explorer;
1. Right click on the folder;
1. Select "Git Bash here", and a shell window will open.  This shell window uses Unix/Bash style commands rather than Windows style commands.
1. Set a Git username:

    ```
git config --global user.name "My Name"
    ```
1. Confirm that you have set the Git username correctly:

    ```
git config --global user.name
> My Name
    ```
1. Set an email address in Git. To keep your email address private, you can use the address username@users.noreply.github.com, replacing username with your GitHub username.  You will need to [tell GitHub to keep your email address private](https://help.github.com/articles/keeping-your-email-address-private/).

    ```
git config --global user.email "email@example.com"
    ```
1. Confirm that you have set the email address correctly in Git:
    ```
git config --global user.email
> email@example.com
    ```
1. [Link the email address to your GitHub account](https://help.github.com/articles/adding-an-email-address-to-your-github-account/), so that your commits can be attributed to you and displayed in your contributions graph.

### Set up authentication with GitHub.

When you download or "clone" a repository from GitHub you need to connect securely and authenticate yourself.  This is done automatically with a protocol called SSH but it still needs to be set up.

 If you have GitHub Desktop installed, you can use it to clone repositories and not deal with SSH keys. 

#### Generate a new SSH key

Generate an SSH public-private keypair on your computer.  You will add the public key to your GitHub account later.

1. Open Git Bash.
1. Paste the text below, substituting in your GitHub email address.
   ```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
This creates a new ssh key, using the provided email as a label.

   ```
Generating public/private rsa key pair.
   ```
1. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

   ```
Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]
   ```

1. If other people share your computer with you then, at the prompt, type a secure passphrase. You will need to type in this passphrase everytime you connect to GitHub.  If you are the only person who uses your computer then you can probably leave this blank.

   ```
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
   ```

#### Add your SSH key to the ssh-agent

ssh-agent is a program that manages SSH keys for you.

1. Ensure the ssh-agent is running:

   ```
eval $(ssh-agent -s)
> Agent pid 59566
   ```
   
1. Add your SSH private key to the ssh-agent.

   ```
ssh-add ~/.ssh/id_rsa
   ```

#### [Add the SSH key to your GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
  1. Copy the SSH key to your clipboard.
   ```
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

When more than two people are working on the same repository then, *before they start editing*, they download any changes that have been made since the last time the worked on the repository (**pull**).  They can then **edit**, **commit**, and **push** their changes.  Doing a pull before doing anything else is very good practice and keeps life simple.

If you have made changes, and someone else has made changes at the same time, then Git allows you to merge them together easily.  Merging, however, is beyond the scope of this document as it should rarely be an issue for a simple repository like this one.


### Download this Repository

You will need to download this repository from GitHub.

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

You will see that there isn't much there initially.  That is because you are looking at the master branch and we aren't using it for much at the moment.  The documentation files are all in a special branch used specifically by GitHub Pages.

#### What are Branches?



## Build the Theme

Follow these instructions to build the theme.

### 1. Download the theme

First download or clone the theme from the [GitHub repo](https://github.com/tomjohnson1492/documentation-theme-jekyll). Most likely you won't be pulling in updates once you start customizing the theme, so downloading the theme (instead of cloning it) probably makes the most sense. In GitHub, click the **Clone or download** button, and then click **Download ZIP**.

### 2. Install Jekyll

If you've never installed or run a Jekyll site locally on your computer, follow these instructions to install Jekyll:

* [Install Jekyll on Mac][theme_install_jekyll_on_mac]
* [Install Jekyll on Windows][theme_install_jekyll_on_windows]

### 3. Install Bundler

In case you haven't installed Bundler, install it:

```
gem install bundler
```

You'll want [Bundler](http://bundler.io/) to make sure all the Ruby gems needed work well with your project. Bundler sorts out dependencies and installs missing gems or matches up gems with the right versions based on gem dependencies.

### 4. Option 1: Build the Theme (*without* the github-pages gem) {#option1}

Use this option if you're not planning to publish your Jekyll site using [GitHub Pages](https://pages.github.com/).

Bundler's Gemfile is how it specifies and manages project dependencies are managed. Although this project includes a Gemfile, this theme doesn't have any dependencies beyond core Jekyll. The Gemfile is used to specify gems needed for publishing on GitHub Pages. **If you're not planning to have GitHub Pages build your Jekyll project, delete these two files from the theme's root directory:**

* Gemfile
* Gemfile.lock

If you've never run Jekyll on your computer (you can check with `jekyll --version`), you may need to install the jekyll gem:

```
gem install jekyll
```

Now run jekyll serve (first change directories (`cd`) to where you downloaded the project):

```
jekyll serve
```

### 4. Option 2: Build the Theme (*with* the github-pages gem) {#option2}

If you *are* in fact publishing on GitHub Pages, leave the Gemfile and Gemfile.lock files in the theme.The Gemfile tells Jekyll to use the github-pages gem. **However, note that you cannot use the normal `jekyll serve` command with this gem due to dependency conflicts between the latest version of Jekyll and GitHub Pages** (which are noted [briefly here](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)).

You need Bundler to resolve these dependency conflicts. Use Bundler to install all the needed Ruby gems:

```
bundle update
```

Then *always* use this command to build Jekyll:

```
bundle exec jekyll serve
```

If you want to shorten this long command, you can put this code in a file such as jekyll.sh (on a Mac) and then simply type `. jekyll.sh` to build Jekyll.

## Configure the sidebar

There are several products in this theme. Each product uses a different sidebar. This is the essence of what makes this theme unique -- different sidebars for different product documentation. The idea is that when users are reading documentation for a specific product, the sidebar navigation should be specific to that product. (You can read more of my thoughts on why multiple sidebars are important in this [blog post](http://idratherbewriting.com/2016/03/23/release-of-documentation-theme-for-jekyll-50/).)

The top navigation remains the same, because it allows users to navigate across products. But the sidebar navigation adapts to the product.

Because each product uses a different sidebar, you'll need to set up your sidebars. There's a file inside \_includes/custom called "sidebarconfigs.html." This file controls which sidebar gets associated with which product. Open up this file to see its contents.

The sidebarconfigs.html file uses simple `if elsif` logic to set a variable that the sidebar.html file uses to read the sidebar data file. The code in sidebarconfigs.html looks like this:

{% raw %}
```liquid
{% if page.sidebar == "home_sidebar" %}
{% assign sidebar = site.data.sidebars.home_sidebar.entries %}

{% elsif page.sidebar == "product1_sidebar" %}
{% assign sidebar = site.data.sidebars.product1_sidebar.entries %}

{% elsif page.sidebar == "product2_sidebar" %}
{% assign sidebar = site.data.sidebars.product2_sidebar.entries %}

{% elsif page.sidebar == "theme_sidebar" %}
{% assign sidebar = site.data.sidebars.theme_sidebar.entries %}

{% else %}
{% assign sidebar = site.data.sidebars.home_sidebar.entries %}
{% endif %}
```
{% endraw %}

In each page's frontmatter, you must specify the sidebar you want that page to use. Here's an example of the page frontmatter showing the sidebar property:

<pre>
---
title: Alerts
tags: [formatting]
keywords: notes, tips, cautions, warnings, admonitions
last_updated: July 3, 2016
summary: "You can insert notes, tips, warnings, and important alerts in your content. These notes are stored as shortcodes made available through the linksrefs.hmtl include."
<span class="red">sidebar: theme_sidebar</span>
permalink: theme_alerts
---
</pre>

The `sidebar: theme_sidebar` refers to the \_data/sidebars/theme_sidebar.yml file (meaning, the theme_sidebar.yml file inside the sidebars subfolder inside the \data folder).

If no sidebar assignment is found in the page frontmatter, the default sidebar (specified by the `else` statement) will be shown: `site.data.sidebars.home_sidebar.entries`.

Note that your sidebar can only have 2 levels. Given that each product has its own sidebar, this depth should be sufficient (it's really like 3 levels). Deeper nesting goes against usability recommendations.

{% include note.html content="Note that each level must have at least one topic before the next level starts. You can't have a second level that contains multiple third levels without having at least one standalone topic in the second level." %}

You can optionally turn off the sidebar on any page (e.g. landing pages). To turn off the sidebar for a page, you should set the page frontmatter tag as `hide_sidebar: true`.

For more detail on the sidebar, see [Sidebar navigation][theme_sidebar_navigation].

## Sidebar syntax

The sidebar data file uses a specific YAML syntax that you must follow. Follow the sample pattern shown in the theme. For example:

```yaml
entries:
- title: sidebar
  product: Jekyll Doc Theme
  version: 6.0
  folders:

  - title: Overview
    output: web, pdf
    folderitems:

    - title: Get started
      url: /index.html
      output: web, pdf

    - title: Introduction
      url: /theme_introduction.html
      output: web, pdf

    - title: Supported features
      url: /theme_supported_features.html
      output: web, pdf

    - title: About the theme author
      url: /theme_about.html
      output: web, pdf

    - title: Support
      url: /theme_support.html
      output: web, pdf

  - title: Release Notes
    output: web, pdf
    folderitems:

    - title: 6.0 Release notes
      url: /theme_release_notes_60.html
      output: web, pdf

    - title: 5.0 Release notes
      url: /theme_release_notes_50.html
      output: web, pdf

```

Each `folder` or `subfolder` must contain a `title` and `output` property. Each `folderitem` or `subfolderitem` must contain a `title`, `url`, and `output` property.

The two outputs available are `web` and `pdf`. (Even if you aren't publishing PDF, you still need to specify `output: web`).

The YAML syntax depends on exact spacing, so make sure you follow the pattern shown in the sample sidebars. See my [YAML tutorial](theme_yaml_tutorial) for more details about how YAML works.

To accommodate the title page and table of contents in PDF outputs, each product sidebar must list these pages before any other:

```yaml
- title:
  output: pdf
  type: frontmatter
  folderitems:
  - title:
    url: /titlepage
    output: pdf
    type: frontmatter
  - title:
    url: /tocpage
    output: pdf
    type: frontmatter
```

Leave the output as `output: pdf` for these frontmatter pages so that they don't appear in the web output.

For more detail on the sidebar, see [Sidebar navigation][theme_sidebar_navigation] and [YAML tutorial][theme_yaml_tutorial].

## Relative links and offline viewing

This theme uses relative links throughout so that you can view the site offline and not worry about which server or directory you're hosting it. It's common with tech docs to push content to an internal server for review prior to pushing the content to an external server for publication. Because of the need for seamless transferrence from one host to another, the site has to use relative links.

To view pages locally on your machine (without the Jekyll preview server), they need to have the `.html` extension. The `permalink` property in the page's frontmatter (without surrounding slashes) is what pushes the files into the root directory when the site builds.

## Page frontmatter

When you write pages, include these same frontmatter properties with each page:

```yaml
---
title: "Some title"
tags: [sample1, sample2]
keywords: keyword1, keyword2, keyword3
last_updated: Month day, year
summary: "optional summary here"
sidebar: sidebarname
permalink: filename.html
---
```

(You will customize the values for each of these properties, of course.)

For titles, surrounding the title in quotes is optional, but if you have a colon in the title, you must surround the title with quotation marks. If you have a quotation mark inside the title, escape it first with a backlash `\`.

Values for `keywords` get populated into the metadata of the page for SEO.

Values for `tags` must be defined in your \_data/tags.yml list. You also need a corresponding tag file inside the tags folder that follows the same pattern as the other tag files shown in the tags folder. (Jekyll won't auto-create these tag files.)

If you don't want the mini-TOC to show on a page (such as for the homepage or landing pages), add `toc: false` in the frontmatter.

The `permalink` value should be the same as your filename and include the ".html" file extension.

For more detail, see [Pages][theme_pages].

## Where to store your documentation topics

You can store your files for each product inside subfolders following the pattern shown in the theme. For example, product1, product2, etc, can be stored in their own subfolders inside the \_pages directory. Inside \_pages, you can store your topics inside sub-subfolders or sub-sub-folders to your heart's content. When Jekyll builds your site, it will pull the topics into the root directory and use the permalink for the URL.

Note that product1, product2, and theme are all just sample content to demonstrate how to add multiple products into the theme. You can freely delete that content.

For more information, see [Pages][theme_pages] and [Posts][theme_posts].

## Configure the top navigation

The top navigation bar's menu items are set through the \_data/topnav.yml file. Use the top navigation bar to provide links for navigating from one product to another, or to navigate to external resources.

For external URLs, use `external_url` in the item property, as shown in the example topnav.yml file. For internal links, use `url` the same was you do in the sidebar data files.

Note that the topnav has two sections: `topnav` and `topnav_dropdowns`. The topnav section contains single links, while the `topnav_dropdowns` section contains dropdown menus. The two sections are independent of each other.

## Generating PDF

If you want to generate PDF, you'll need a license for [Prince XML](http://www.princexml.com/). You will also need to [install Prince](http://www.princexml.com/doc/installing/).  You can generate PDFs by product (but not for every product on the site combined together into one massive PDF). Prince will work even without a license, but it will imprint a small Prince image on the first page, and you're supposed to buy the license to use it.

If you're on Windows, install [Git Bash client](https://git-for-windows.github.io/) rather than using the default Windows command prompt.

Open up the css/printstyles.css file and customize the email address (`youremail@domain.com`) that is listed there. This email address appears in the bottom left footer of the PDF output. You'll also need to create a PDF configuration file following the examples shown in the pdfconfigs folder, and also customize some build scripts following the same pattern shown in the root: pdf-product1.sh

See the section on [Generating PDFs][theme_generating_pdfs] for more details about setting the theme up for this output.

## Blogs / News

For blog posts, create your markdown files in the \_posts folder following the sample formats. Post file names always begin with the date (YYYY-MM-DD-title).

The news/news.html file displays the posts, and the news_archive.html file shows a yearly history of posts. In documentation, you might use the news to highlight product features outside of your documentation, or to provide release notes and other updates.

See [Posts][theme_posts] for more information.

## Markdown

This theme uses [kramdown markdown](http://kramdown.gettalong.org/). kramdown is similar to GitHub-flavored Markdown, except that when you have text that intercepts list items, the spacing of the intercepting text must align with the spacing of the first character after the space of a numbered list item. Basically, with your list item numbering, use two spaces after the dot in the number, like this:

```
1.  First item
2.  Second item
3.  Third item
```

When you want to insert paragraphs, notes, code snippets, or other matter in between the list items, use four spaces to indent. The four spaces will line up with the first letter of the list item (the <b>F</b>irst or <b>S</b>econd or <b>T</b>hird).

```
1.  First item

    ```
    alert("hello");
    ```

2.  Second item

    Some pig!

3.  Third item
```

See the topics under "Formatting" in the sidebar for more information.

## Automated links

If you want to use an automated system for managing links, see [Automated Links][theme_hyperlinks.html#automatedlinks]. This approach automatically creates a list of Markdown references to simplify linking.

## Other instructions

The content here is just a getting started guide only. For other details in working with the theme, see the various sections in the sidebar.

{% include links.html %}
