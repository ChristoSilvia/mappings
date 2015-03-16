---
layout: post
title: "Writing Jekyll blog posts with Vim and Git"
date: 2015-03-15 18:31:00
categories: vim git meta
---

I'm going to lie to you in this blog post.  As you read this, I think that you're assuming I sat down 
to write this (after some inspiration of course!) and then wrote the whole thing in one go.  At least, I 
bet that you thought that I wrote the beginning before the end.  Wrong.  Here's the completed paragraph:



As I sit down to write about [making mathematica-style manipulate on the web](www.bro.ken), 
I come across a problem.  That problem is apostrophes.  Vim does some markdown syntax 
highlighting.  Right now vim is making the string "Writing Jekyll blog posts with Vim and Git"
appear red to me.  Now, I want to take a screenshot.  I am running ubuntu, with the (i3)[i3wm.org] 
window manager installed.  I google "screenshots ubuntu" (note that the text in quotes is red), and I get
an [askubuntu post](http://askubuntu.com/questions/6558/what-screenshot-tools-are-available) (which just 
taught me that `"+p` is the vim paste command), and the askubuntu post tells me that there is a tool called
[shutter](shutter-project.org) which lets me take screenshots on ubuntu.

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

I type `$ shutter` and looks like it has a gui.  Shit.  Close that.  Google "shutter
screenshots command line".  A nice helpul [tutorial](http://www.linuxstall.com/take-screenshot-in-linux-with-shutter/) 
(so glad i didn't have to type that whole url, because of the neat handy `"+p` command). 
It tells me that i need to execute `$ shutter --full` to take a screenshot.  I do so, but a
notification pops up (that I can't screenshot, sorry) telling me where it saved the screenshot.
The notification fades very fast.  Time to go hunting around my filesystem to see where the screenshot went.

```bash
$ ls ~
college dev Downloads Dropbox media res tmp work Workspace 2_001.png
```
In order to get the above image, I'm trying to toy around with xclip.  I do `$ ls ~ | xclip -i` (the '-i'
 is for in), and it works? Nope!  Not sure what's wrong!  I won't be, I've already wasted enough time 
that I just typed it manually.

### Let's back up a bit.

As I sit down to write about (making mathematica-style manipulate on the web)[www.bro.ken], 
I come across a problem.  That problem is apostrophes.  Vim does some markdown syntax 
highlighting.  Right now vim is making the string "Writing Jekyll blog posts with Vim and Git"
appear red to me.  Now, I want to take a screenshot.  I am running ubuntu, with the (i3)[i3wm.org] 
window manager installed.  I google "screenshots ubuntu" (note that the text in quotes is red), and I get
an [askubuntu post](http://askubuntu.com/questions/6558/what-screenshot-tools-are-available) (which just 
taught me that `"+p` is the vim paste command), and the askubuntu post tells me that there is a tool called
(shutter)[shutter-project.org] which lets me take screenshots on ubuntu.


I just pasted a paragraph from above, which means that I need to know how to copy and paste text in vim.
I know how to paste, not how to copy.  Google gets me 
[here](http://www.tech-recipes.com/rx/219/copy-and-paste-text-with-vi-or-vim/) which might help.  
Nope, let's do [this](http://vim.wikia.com/wiki/Copy,_cut_and_paste) instead.  The text is still interpreting 
anything posessive as a quote, making this crazy red.  But, I can embed pitures!

Oh, wait, I'm using Jekyll.  I have to look up how to embed pictures.  [Google](https://www.google.com/search?client=ubuntu&channel=fs&q=embed+pictures+in+Jekyll&ie=utf-8&oe=utf-8)
then [the official jekyll site](http://jekyllrb.com/docs/posts/) which says midway down that you can 
add in an image asset in a post.  I have to move the screenshot to assets (good get it out of my home 
directory!)

```bash
$ ls ~/dev/jekyll-blog
about.md  _config.yml   css       _includes   _layouts  README.md  _site
about.md~ _config.yml~  feed.xml  index.html  _posts    _sass
```

Is assets in `includes`? 

```bash
~/dev/jekyll-blog$ ls _includes
footer.html header.html head.html
```

Assets is probably not in includes.  Do I have to create it?  Yes, back on the [official jekyll site](http://jekyllrb.com/docs/posts/) it says that I need to make a directory called "something like `assets` or `downloads`, into which any images, downloads, or other resources are placed."  Okay.

`~/dev/jekyll-blog$ mkdir assets`

`~/dev/jekyll-blog$ mv ~/Workspace\ 2_001.png assets`

`~/dev/jekyll-blog$ ls assets`

`Workspace 2_001.png`

`~/dev/jekyll-blog$ mv assets/Workspace\ 2_001.png assets/red.png`

Yay, it's here.  It should appear below.
![If it's not broken this should be an image]({{ site.url }}/assets/red.png).

Of course, I go to look and it's not there.  Wtf.  

Maybe the directory needs to be named `_assets` and not `assets`?  That doesn't fix it.

### Let's start over

As I sit down to write about (making mathematica-style manipulate on the web)[www.bro.ken], 
I come across a problem.  That problem is apostrophes.  Vim does some markdown syntax 
highlighting.  Right now vim is making the string "Writing Jekyll blog posts with Vim and Git"
appear red to me.

![Ahaha sucker]({{ site.url }}/assets/red.png)

I give up.
