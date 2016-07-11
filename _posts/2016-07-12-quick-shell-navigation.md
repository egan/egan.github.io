---
layout: post
title: Quick Directory Navigation in the Shell
comments: true
tags: [arch, shell]
---

I spend a lot of time in the shell.
In fact, the only two graphical programs I use at home are my [web browser](https://github.com/The-Compiler/qutebrowser) and [pdf viewer](https://pwmt.org/projects/zathura/), and those have vim-like interfaces with extremely minimal [chrome](http://www.catb.org/~esr/jargon/html/C/chrome.html).

Without a graphical file manager, good organization is a must.
Moreover, in a command line setting, I prefer short filenames and abbreviated directory names to limit my typing.
Even with heavy use of tab completion, directory navigation can still be pretty tedious.

## Bookmarks
One useful tool is the ability to store and recall directory bookmarks.
My use case for this is a deep directory where I'm working on a project and would like to jump there quickly.
A good solution is the [bashmarks](https://github.com/huyng/bashmarks) shell script.

This script should be sourced by bash in e.g. your `.bashrc` file.
It provides a few one-letter (abbreviated indeed!) functions.

```
s <bookmark_name> # Saves the current directory as "bookmark_name"
g <bookmark_name> # Goes (cd) to the directory with "bookmark_name"
p <bookmark_name> # Prints the directory with "bookmark_name"
d <bookmark_name> # Deletes the bookmark
l                 # Lists all available bookmarks
```

Conveniently, these functions are tied into bash completion so pressing tab will complete with known bookmarks.

Bookmarks are only good for directories you need to access a lot, and need to be created first.
One could try to map their entire file system into a flat dictionary of bookmarks but this would basically defeat their purpose.

## Up
One navigation operation I found particularly tedious was moving up the directory tree with e.g.

	egan@teuthida:~/docs/old/15/q3/eme/185/code/weinbot/src/Drive/Sabertooth (master *)$ cd ../../../../../../..

I would invariably make a typo or miscount the number of steps I needed.
The correspondence between the command and the mental operation was not good either.
You don't want to "go seven levels up in the tree", but rather "go to q3".
So I implemented a shell script [upstr.sh](https://github.com/egan/scripts/blob/master/upstr.sh) that returns a `..` path from either a number or a substring of the current working directory path.

To use it, add a bash function to your bash environment:

{% highlight bash %}
function up() { d=$(upstr.sh "$1") && cd "$d"; }
{% endhighlight %}

Then in the prior example,

	up 7

Or better,

	up q3

Are equivalent to the pinky-snapping `..` path.
While I was coming up with the substring concept, I realized that the same principal could be applied in the opposite direction, down the directory tree.

## Down
In the prior example, it is not just tedious to back out of the that deep path, but also tedious to get there is the first place.
It would be nice to jump straight there.
However, a bookmark isn't appropriate because this is an archived directory that I am not actively working in.

Applying the substring jumping concept from upstr.sh, I developed [downstr.sh](https://github.com/egan/scripts/blob/master/downstr.sh) that searches the child directory tree for a substring and returns the first matching path.
To use it, add a similar bash function as up to your bash environment:

{% highlight bash %}
function down() { d=$(downstr.sh "$@") && cd "$d"; }
{% endhighlight %}

Then, from `~`, it is as simple as:

	down Sabertooth

To `cd` all the way into that deep directory.
It just so happens that this is the only directory named "Sabertooth" on my entire filesystem.
What if I wanted to jump directly the "src" directory in that path?

Doing:

	down src

Takes me immediately to `~/src/` because down prefers the shortest path.
This is usually desirable because deeper paths are typically less often traveled.

One could do:

	down weinbot/src

To get to the correct place, but that requires excessive typing and sometimes you don't remember the exact directory structure.
Fortunately, I designed downstr.sh to accept multiple substrings:

	down 185 src

This is an extremely powerful interaction mechanism because you often remember parts of the path like this — in this case, I would want to go to the source code directory for my 185 project.
I don't have to remember what year or whatever to get to the right path.
This means that even obscure paths can be jumped to if you organize your files in a systematic way.

## Sidenote: Long Pathnames in the Prompt
I have lots of super long pathnames, as long or longer than the prior example.
Eventually the pathname may overwhelm your command line, which is an eyesore.
Many times, long pathnames are the result of moving deep directory trees into a deep path.
In these cases, you don't need as much context when working in the local tree.

My solution to long pathnames is to chomp them in the bash prompt once they reach a certain column length:

{% highlight bash %}
# Abbreviate working directory if long.
_dir_chomp () {
	local p=${1/#$HOME/\~} b s
	s=${#p}
	while [[ $p != "${p//\/}" ]]&&(($s>$2))
	do
		p=${p#/}
		[[ $p =~ \.?. ]]
		b=$b/${BASH_REMATCH[0]}
		p=${p#*/}
		((s=${#b}+${#p}))
	done
	echo ${b/\/~/\~}${b+/}$p
}

# Show git information in prompt.
. /usr/share/git/git-prompt.sh
export GIT_PS1_SHOWDIRTYSTATE=1

# Command prompts.
PS1='\[\033[G\]\u@\h:$(_dir_chomp "$(pwd)" 35)$(__git_ps1 " (%s)")\$ '
{% endhighlight %}

The result for the prior example is:

	egan@teuthida:~/d/o/1/q3/eme/185/code/weinbot/src (master *)$

