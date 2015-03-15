---
layout: post
title: "Writing Jekyll blog posts with Vim and Git"
date: 2015-03-15 18:31:00
categories: vim git meta
---

As I sit down to write about (making mathematica-style manipulate on the web)[www.bro.ken], 
I come across a problem.  That problem is apostrophes.  Vim does some markdown syntax 
highlighting.  Right now vim is making the string "Writing Jekyll blog posts with Vim and Git"
appear red to me.  Now, I want to take a screenshot.  I am running ubuntu, with the (i3)[i3wm.org] 
window manager installed.  I google "screenshots ubuntu" (note that the text in quotes is red), and I get
an (askubuntu post)[http://askubuntu.com/questions/6558/what-screenshot-tools-are-available] (which just 
taught me that `"+p` is the vim paste command), and the askubuntu post tells me that there is a tool called
(shutter)[shutter-project.org] which lets me take screenshots on ubuntu.

Now I know what to do:
```bash
$ sudo apt-get install shutter
...
After this operation, 32.3 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
```

Just to be clear, the original goal of this was to make the vim syntax highlighting be sane about apostrophes
in markdown, and not treat them like they're quotes.  To show you this problem, I'm installing a screenshot
tool.  I originally encountered this problem when trying to write another blog post.

I type `$ shutter` and looks like it has a gui.  Shit.  Close that.  Google "shutter screenshots command line".  A nice helpul (tutorial)[http://www.linuxstall.com/take-screenshot-in-linux-with-shutter/] (so glad i didn't have to type that whole url, because of the neat handy `"+p` command). It tells me that i need to execute `$ shutter --full` to take a screenshot.  I do so, but a notification pops up (that I can't screenshot, sorry) telling me where it saved the screenshot.  The notification fades very fast. 
