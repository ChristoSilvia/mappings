---
layout: post
title: Grokking the Product Rule
date: 2015-07-06
categories: math reu grokking
---

This post is going to be about an integral identity called the
	[product rule](https://en.wikipedia.org/wiki/Product_rule).
This identity is going to be central to an efficient
	scheme for computing the energy-laplacian matrix
	for the serpinski gasket.
It's also cool.


In regular normal one-dimensional calculus, we define the derivative:

{% latex density=120 %}
$$ f'(x) := \lim_{h \to 0} \frac{ f(x + h) - f(x) }{h} $$
{% endlatex %}

Let's explore some of the properties.
Some functions can be split into a product of two other functions.
For example, {% latex density=120 %} $f(x) = x^2 - 1$ {% endlatex %}
	can be split into two functions in the following way:
	{% latex density=120 %} $f(x) = u(x) v(x) = (x - 1) (x + 1) $
	{% endlatex %}.
If {% latex density=120 %} $f(x)$ {% endlatex %} is of this form, 
	then knowing a rule for determining {% latex density=120 %}
	$f'(x)$ {% endlatex %} from {% latex density=120 %} $u(x)$
	{% endlatex %} and {% latex density=120 %} $v(x)$ {% endlatex %}
	might be valuable and save a good deal of effort.
If you want to, you might want to stop reading and try to figure out
	a way to do this yourself.
It will be rewarding even if you give up and keep reading.

Our first step in trying to figure out this rule is simply to use
	the definition of a derivative.

{% latex density=120 %}
$$ (u v)'(x) = \lim_{h \to 0} \frac{ u(x + h) v(x + h) - u( x ) v( x )}{h} $$
{% endlatex %}

What would be a nice form for this to take?
If the right side would perhaps contain either 

{% latex density=120 %}
$$ u'(x) := \lim_{h \to 0} \frac{ u(x + h) - u(x) }{h} $$
{% endlatex %} 

or 

{% latex density=120 %} 
$$ v'(x) := \lim_{h \to 0} \frac{ v(x + h) - v( x) }{h} $$
{% endlatex %}

then the formula would be very nice: it would simply express the 
	derivative of {% latex density=120 %} f(x) {% endlatex %}
	in terms of the derivatives of its parts.

With that motivation in mind, I'm going to try to refactor the expression
	for the derivative of {% latex density=120 %} u(x) v(x) {% endlatex %}.
We assume that the functions involved are contintuous.

{% latex density=120 usepackages=amsmath %}
\begin{align*}
(uv)'(x) & = \lim_{h \to 0} \frac{ u(x + h) v(x + h) - u( x ) v( x ) }{h} \\
& = \lim_{h \to 0} \frac{ u(x + h) v(x + h) - u(x + h) v( x ) + u(x + h) v( x)
	- u(x) v(x)}{h}\\
& = \lim_{h \to 0} u(x + h) \left( \frac{v(x + h) - v(x)}{h} \right)
	+ v(x) \left( \frac{u(x + h) - u(x) }{h} \right) \\
& = u(x) \left( \lim_{h \to 0} \frac{v(x + h) - v(x) }{h} \right)
+ v(x) \left( \lim_{h \to 0} \frac{ u(x + h) - u(x)}{h} \right)\\
(u v)'(x) & = u(x) v'(x) + v(x) u'(x) \\
\end{align*}
{% endlatex %}

I learned this under the name of the _product rule_.

We can use this fact about derivatives to learn a fact about integrals.
We use newton's Fundamental Theorem of Calculus:

{% latex density=120 %}
$$ \int_a^b \left( f(x) \right)' dx =  f(a) - f(b)$$
{% endlatex %}

On the function {% latex density=120 %} $u(x) v(x)$ {% endlatex %}:

{% latex density=120 %}
$$ \int_a^b u(x) v'(x) + v(x) u'(x) dx = \int_a^b ( u(x) v(x) )' dx 
	= u(a) v(a) - u( b ) v (b) $$
{% endlatex %}

for _any two functions_.
