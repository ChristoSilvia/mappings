---
layout: post
title: "Math Notation with MathJax and Jekyll"
date: 2015-06-23 16:00:00
categories: jekyll math mathjax latex notation
---

If I want to write blog posts about math, at some point I'm going to need
	to figure out how to write mathematical notation.
In all of high school, it never occurred to me that people needed to have a way
	to write math notation on the computer.
So as soon as I got to college, I heard about LaTeX.
Being a good hacker, I decided to write one of my linear algebra problem sets in LaTeX to try it out.
Terrible idea.
LaTeX took quite some time to learn and was fairly complicated.
It made that assignment miserable to complete, but I was fairly competent afterwards.

Later on, I heard of [IPython notebooks](http://ipython.org/notebook.htm)
	and used them to write math code as well.
Now that I'm blogging, I'm going to need to write math notation that will compile to HTML.
I heard about MathJax from poking around in the IPython configuration options,
	and I'm going to try to set it up on this Jekyll site.

First step: check what is in the home directory of my Jekyll site.

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

<div>
$$ x^2 + y^2 = r^2 $$
</div>
