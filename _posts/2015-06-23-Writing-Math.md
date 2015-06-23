---
layout: post
title: "Math Notation with MathJax and Jekyll"
date: 2015-06-23 16:00:00
categories: jekyll math mathjax latex notation
---

I am configuring Latex preprocessors for math blogging. 
The first one I've tried is MathJax, which does client-side rendering.
It seems slow on my laptop, but it is extremely simple.
I got it working in under a half an hour.

I found out about [liquid latex](http://www.flx.cat/jekyll/2013/11/10/liquid-latex-jekyll-plugin.html)
	from googling, but that website is very difficult to read.
Fortunately, they link to the [github repo](https://github.com/fgalindo/jekyll-liquid-latex-plugin).
The repo is [MIT Licensed](https://en.wikipedia.org/wiki/MIT_License), and the installation
	instructions direct me to copy `liquid_latex.rb` to my `_plugins` directory.

{% highlight bash %}
$ wget https://raw.githubusercontent.com/fgalindo/jekyll-liquid-latex-plugin/master/liquid_latex.rb
{% endhighlight %}

One minor point of confusion is that nobody told me that I would have to create my own `_plugins`
	directory.
From there, the configuration instructions direct me to update my `_config.yml`.
I copied the config information verbatum into my `_config.yml`.

Next, I tried it out:
```
{% latex usepackages=sudoku %}
\begin{sudoku}
| |2| | |3| |9| |7|.
| |1| | | | | | | |.
|4| |7| | | |2| |8|.
| | |5|2| | | |9| |.
| | | |1|8| |7| | |.
| |4| | | |3| | | |.
| | | | |6| | |7|1|.
| |7| | | | | | | |.
|9| |3| |2| |6| |5|.
\end{sudoku}
{% endlatex %}
```

which produced:

{% latex usepackages=sudoku %}
\begin{sudoku}
| |2| | |3| |9| |7|.
| |1| | | | | | | |.
|4| |7| | | |2| |8|.
| | |5|2| | | |9| |.
| | | |1|8| |7| | |.
| |4| | | |3| | | |.
| | | | |6| | |7|1|.
| |7| | | | | | | |.
|9| |3| |2| |6| |5|.
\end{sudoku}
{% endlatex %}


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

<div>
$$ x^2 + y^2 = r^2 $$
</div>
