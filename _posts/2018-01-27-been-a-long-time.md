---
layout: post
comments: true
title: It's Been a Long Time
tags: [misc]
---

I won't go into how and why this website has gone silent since its creation almost two years ago.
Anyone would understand, I am sure.
But why the hopefully-not-so-brief reanimation?

## A Surprise Email
I am the maintainer of a [few packages](https://aur.archlinux.org/packages/?K=egan&SeB=m) on the [Arch Linux User Repository](http://aur.archlinux.org/) (AUR).

> Between 2015-06-08 and 2015-08-08 the AUR transitioned from version 3.5.1 to 4.0.0, introducing the use of Git repositories for publishing the PKGBUILDs. Existing packages were dropped unless manually migrated to the new infrastructure by their maintainers.

Some of the packages I had installed at the time lost their maintainership during the transition, so I decided I was as good as anyone to take over.
I uploaded the PKGBUILDs and promptly forgot all about them.

...Until today, two-and-a-half years later, I receive an email from a helpful user explaining some problems with my package for tint, the Tetris emulator.
After fixing these problems I noticed my AUR account was missing a homepage, and this website is it, such as it is.

Since my last update here, I have become the firmware manager at my company and have been very active on GitHub, though privately.
I interact a lot with students, prospective hires, and coworkers who are less familiar with the Unixy world.
Between that and the realization that a small group of Arch Linux users may be interested in my work, I figured I should add a direct link to my profile in the sidebar here so people might find e.g. my dotfiles repository more easily.

## Everything Breaks
I add a link to my GitHub profile to the sidebar and the styling breaks.
So I resolve to debug the problem locally.

First, I could not even remember how to build a locally serve the site.
After a quick review, I discover my entire ruby installation is messed up and I cannot run jekyll or bundle.

Gems are installed to your home directory, and this location must be added to the `$PATH` in order to operate correctly.
My `$PATH` pointed to a directory for ruby 2.3.0 rather than 2.5.0, which is what is now installed on my machine (gotta love rolling releases).

While cleaning up the ruby stuff, I went up one level two high and accidentally issued a `rm -r *` on my home directory!
Luckily the ownership of my [`~/bin/cflush`](https://github.com/egan/scripts/blob/master/cflush.c) was root and `rm` halted there before wiping everything.
Interestingly, while `rm` seemed to only touch files in my `~/bin` directory, the files it deleted therein were randomized before it halted.
Anyway, after recovering from my backups, I did a quick `pacman` update to make sure all the ruby stuff was squared away.

That update broke a dependency of uber-mutt, my email client that last built in 2014 --- RIP.
I'll look into neomutt soon, and if successful, will make a post here.

After installing bundle, trying to figure out what was wrong with the json gem build, and eventually getting the website building, I quickly found the culprit:

[https://github.com/github/pages-gem/issues/350](https://github.com/github/pages-gem/issues/350)

Things look like they are working now.

## Conclusion
I had been coasting on the relative stability of my Arch Linux installation, and my lack of real computing work to do at home.
Even though a large part of my job is now writing software, today's experience shows me I still thoroughly enjoy hacking away at software in my free time.
I have also gained a wealth of engineering experience the last couple years at work, so if I can keep my motivation, I ought be able to write some interesting posts here.
We will see.
