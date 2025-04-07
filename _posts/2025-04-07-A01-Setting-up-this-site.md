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


### local hosting and testing
### publishing on github pages
### personalizing the site
### learning to write blog posts

#### I, you statements, Front Matter, structure of lab reports, various expressive tools 

## Problems Encountered along the way

## Summary
