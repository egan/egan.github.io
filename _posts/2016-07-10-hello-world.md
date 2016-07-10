---
layout: post
comments: true
title: Hello World
tags: [misc, web]
---

Well, I have to start with something....

## Introduction
For a few years now, I have wanted to publish a website that treats some of my interests in mechanical engineering similarly to the hundreds of great software engineering technical blogs I frequent.
As a student I just never got around to it.
After just over a year of working as a mechatronics engineer in the industry, I figured I better get around to it before some of my less-used knowledge fades into oblivion.
At least now I theoretically have the free time.

As a hobbyist programmer who uses programming mostly out of laziness — some of my first useful programming projects were to automate some tedious high school homework assignments — I have really only built tools for my homebrew Arch Linux setup and for scientific and engineering analyses.
As such, the world of web development has been pretty foreign to me.
Of course, I knew all the buzzwords that would frequent Hacker News: ruby on rails, nodejs, json, etc.; I just had no idea what they were.

But in order to start this website I had to make my first foray into this world.
I was aware the GitHub supported a free web-hosting service but wanted something a little bit nicer than wrangling HTML eyesores in vim.
Conveniently, GitHub pages supports the [Jekyll](http://jekyllrb.com) content manager.
Jekyll is a static site generator, meaning that the web content is pregenerated and static, rather than dynamically generated when the user loads the page.
As someone who hates bloated web pages, this appealed to me, and fit nicely into my text-based workflow.

## Building the Site
Rather than starting from scratch, I googled around for some tutorials, hoping to find a nice template to work from.
I found these two to be quite useful:

* <http://byverdu.github.io/how-to-host-your-blog-at-github-using-jekyll-and-poole/>
* <http://joshualande.com/jekyll-github-pages-poole>

Both use [poole](http://getpoole.com/), a Jekyll template with a couple of nice themes, [hyde](http://hyde.getpoole.com/) and [lanyon](http://lanyon.getpoole.com/).

I started working from poole only to find that it no longer worked with the latest version of Jekyll.
I guess it would have been too easy if everything just worked!
Combining information from:

* <https://github.com/poole/hyde/pull/145>
* <https://kersulis.github.io/2015/10/31/jekyll-3/>
* <http://jekyllrb.com/docs/upgrading/2-to-3/>

I was able to get the site built.
After a little bit of customization drawn from all over the web, I have what I hope is good platform to write content.
If you are interested in the web development, you can always check out the GitHub [repository](https://github.com/egan/egan.github.io).
