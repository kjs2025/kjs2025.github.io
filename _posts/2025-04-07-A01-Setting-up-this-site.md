---
title: A01 On Setting Up This Site
date: 2025-04-07 03:58:00 America/Los_Angeles

# Templates, Articles, Computer-Networking, or Programming
categories: [Article]

# any concept or entity relevant in the document can be mentioned here
tags: [docker,vs-code,git,jekyll,rouge,code-snippets,syntax-highlighting]
author: kevin
description: in this article i summarize the steps it took to set this website up, and discuss some of the problems i ran into along the way.
---

## What I needed and what I decided to try

I wanted to write about my takeaways doing networking labs. I needed a place that would make it easy to start, and easy to keep going. Jekyll is a static site generator meant to support technical writing. It's a perfect match for me.

## What ended up working for me in terms of set-up

### setting up GitHub and using Jekyll for site template

When I started I already knew I would want to host this site on GitHub Pages, so when I went to <a href="https://github.com/cotes2020/chirpy-starter" target="_blank">chirpy starter here</a>, I clicked the green "use this template" button on the right and chose from the drag-down to "create a new repository"

![img-descrip](/assets/image/a01-5.PNG)

the key thing in setting up the repo for GitHub Pages hosting is to name it `username`.github.io where `username` is ones GitHub username:

![img-descrip](/assets/image/a01-6.PNG)

and that's all for this part for now.

### setting up docker

I already installed Docker some time ago, so all I had to do was update the software version. For a fresh start, definitely grab Docker <a href="https://www.docker.com/" target="_blank">here</a>. 

I also updated my WSL(Windows Subsystem for Linux) to get Docker to work.

```shell
wsl --update
```

for a fresh installation the command would be

```shell
wsl --install
```

then to verify wsl status:

![img-description](/assets/image/a01-1.PNG)

> `Docker` is like a magic box that enables running of different programs in isolated environments for security and consistency. WSL allows the use of these magic boxes on Windows platform. 
{: .prompt-info }

### setting up VS-code containerized git repo

The way managing a static site works is there would be a local code base, where changes related to the site are made and posts are initially written. Then there could be a remote location where the site is published, like GitHub Pages, that periodically receive updates.

For the local code base, I went with VS-code (a code editor program) + Dev Containers extension. VS-code can be grabbed <a href="https://code.visualstudio.com/" target="_blank">here</a>. 

After installing VS-code and opening it up, the Dev Containers extension can be found on the side panel on the left:

![img-descrip](/assets/image/a01-2.PNG)
_click to enter extensions page_

![img-descrip](/assets/image/a01-3.PNG)
_install Dev Containers extension_

Then to clone a git repository into a container, press F1 in VS-code to bring up the command palette and ask to clone repo into a container

![img-descrip](/assets/image/a01-4.PNG)

The command palette will ask for GitHub credentials, then ask one repo from the drop-down to be chosen for cloning:

![img-descrip](/assets/image/a01-7.PNG)

once the repo is chosen, it will prompt again for the branch, but we only have 1 branch (main) so choose that. Then wait a while for the container setup to complete and we have a local code base to make changes from!

the container may be visited at any time in the future from the side panel on the left:

![img-descrip](/assets/image/a01-8.PNG)

![img-descrip](/assets/image/a01-9.PNG)

### local hosting and testing

in VS-code, the keyboard shortcut to bring up the terminal is ``ctrl+` ``

then to host the site from a local port, run the command:

```shell
bundle exec jekyll serve
```

the output from running the above command: 

![img-descrip](/assets/image/a01-10.PNG)
_click on the `open in browser` to navigate to the locally hosted site_

>for small changes made to the site like updating contents of a post, refreshing the page is enough to see updated results. However, if bigger changes like modifications to the _config.yml file are made, then the server needs to be restarted for changes to take effect. 
{: .prompt-tip }

to restart server:

```text
ctrl + ` to bring up terminal
ctrl + C to break out of current process
then up arrow to find 'bundle exec jekyll server' in cmd history and re-run that
```

### publishing on github pages

publishing on GitHub Pages takes 2 requirements:

1. the repo is named `username`.github.io where `username` is username of the GitHub account
2. Pages is enabled in settings by choosing a branch to host

![img-descrip](/assets/image/a01-11.PNG)
_all the way on the right, Settings tab_

on the Pages section of the settings, the `Source` can be either from repo branch or GitHub Actions. Since Chirpy (the Jekyll template I picked) has built in deployment script, I don't need the one provided by GitHub, so I pick to source from GitHub Action.

![img-descrip](/assets/image/a01-12.PNG)

Either way, 

### personalizing the site
### learning to write blog posts

## Problems Encountered along the way

## Summary
