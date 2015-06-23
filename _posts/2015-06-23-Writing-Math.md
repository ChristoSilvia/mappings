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
Unfortunately, it didn't work when I copied in the defaults, but it worked with not config.  Strange.

Next, I tried it out:

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

Hooray!

