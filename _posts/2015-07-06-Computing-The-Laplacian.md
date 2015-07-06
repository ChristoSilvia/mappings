---
layout: post
title: Computing The Laplacian
date: 2015-07-06
categories: math
---

This post is going to be somewhat longer, because I'm going
	to cover something that I've been doing in my math program.
It may also have some background which I will link to and expand on.
The first section of this post presents work done by Bob Strichartz,
	while the second part contains my work.

### Theorem (Carré-DuChamps)

Suppose {% latex density=140 %} $\nu$ {%endlatex%} is an energy measure defined 
	by harmonic functions {%latex%} $u$ {% endlatex %} and 
	{%latex%}$v${%endlatex%}.
Then:

{% latex %}
$$ \int_{SG} f d\nu_{u,v} = \frac{1}{2} \left( \mathcal{E}(f u, v) 
	+ \mathcal{E}(f v, u) - \mathcal{E}(f, u v) \right) $$
{% endlatex %}

First of all, this looks superficially similar to the definition of the
	Christoffel Symbols in riemannian geometry.
Points to anybody who figures out if there's a real connection there.

### Our Energy Measures

For our purposes, we consider energy measures of the form:

{% latex %}
$$ \nu = \nu_{u,u} + \nu_{v,v} $$
{% endlatex %}

The integral of a function {% latex %} $f$ {% endlatex %} with respect to
	this energy measure will be:

{% latex %}
$$ \int_{SG} f d\nu = \mathcal{E}(f u,u) - \frac{1}{2} \mathcal{E}(f, u^2)
+ \mathcal{E}(f v, v) - \frac{1}{2} \mathcal{E}(f, v^2) $$
{% endlatex %}

The level m approximation {%latex%} $\Delta_m$ {%endlatex%} to the energy
	measure laplacian is:

{% latex %}
$$ - \Delta_m f(x) = \left( \frac{5}{3} \right)^m \frac{ \sum_{y ~_m x}
	(f(x) - f(y)) }{ \int \psi_x^{(m)} d\nu } $$
{% endlatex %}

It turns out that we can use what we know about the serpinski gasket to compute
	the above expression exactly.
The function {% latex %} $\psi_x^{(m)}$ {% endlatex %} is piecewise harmonic
	on level m, being 1 at {% latex %} $x$ {% endlatex %} 
	and zero everywhere else.
Therefore, we can express the energy in a finite way:

{% latex %} 
$$\int \psi_x^{(m)} d\nu = \mathcal{E}_m(\psi_x^{(m)} u, u) 
	- \frac{1}{2} \mathcal{E}(\psi_x^{(m)}, u^2)
	+ \mathcal{E}_m(\psi_x^{(m)} v, v) 
	- \frac{1}{2} \mathcal{E}(\psi_x^{(m)}, v^2) $$
{% endlatex %}

We notice that almost every term in the sum of the energy is zero;
	the only nonzero contributions come from the vertices
	which are adjacent to {% latex %} $x$ {% endlatex %}.
If we let {% latex %} $\{ y_j \}, j = 1 \dots 4$ {% endlatex %} are the
	neighboring vertices to {% latex %} $x$ {% endlatex %}
	at level m:

{% latex %}
$$ \mathcal{E}_m(\psi_x^{(m)} u, u) = \left( \frac{5}{3} \right)^m
	\sum_j (u(x) - 0) (u(x) - u(y_j)) $$

$$  \mathcal{E}_m(\psi_x^{(m)}, u^2) = \left( \frac{5}{3} \right)^m
	\sum_j (1 - 0) (u(x)^2 - u(y_j)^2) $$
{% endlatex %}

Which we can condense into a formula for the integral in question:

{% latex %}
\begin{align*}
\mathcal{E}_m(\psi_x^{(m)} u, u) 
	- \frac{1}{2} \mathcal{E}(\psi_x^{(m)}, u^2)
& = \left( \frac{5}{3} \right)^m \sum_j 
	\left( u(x)^2 - u(x) u(y_j) 
	- \frac{1}{2} u(x)^2 + \frac{1}{2} u(y_j)^2 \right)\\
& =  \left( \frac{5}{3} \right)^m \sum_j 
	\frac{1}{2} \left( u(x) - u(y_j) \right)^2\\
\end{align*}
{% endlatex %}
	
Therefore, the denominator of the energy measure now looks like this:

{% latex %}
$$\int \psi_x^{(m)} d\nu = \left( \frac{5}{3} \right)^m 
	\sum_j \frac{1}{2} \left[ \left( u(x) - u(y_j) \right)^2
	+ \left( v(x) - v(y_j) \right)^2 \right]$$
{% endlatex %}

Using our new notation for the vertices, we can now write the energy 
	laplacian with a nice pointwise formulation.
Note that the normalization factors nicely cancel each other out.

{% latex %}
$$ -\Delta_m f(x) = \frac{ \sum_j \left( f(x) - f(y_j) \right) }
	{ \frac{1}{2} \sum_j \left[ \left( u(x) - u(y_j) \right)^2
	+ \left( v(x) - v(y_j) \right)^2 \right]  }$$
{% endlatex %}

With a formula like this, we can reasonable start to go around looking for
	a way to compute the laplaian matrix, and find out some of its
	properties including its eigenvalues and eigenvectors.

### Computing the Energy Laplacian

Once we begin to start considering computing the energy laplacian matrix,
	we are presented with the problem of representing functions on
	the serpinski gasket.

The graph representations at level 0 of the serpinski gasket is just
	a triangle:

![level-0]( https://raw.githubusercontent.com/ChristoSilvia/mappings/master/_assets/vertices_level_0.png)

Add one more level:

![level-0]( https://raw.githubusercontent.com/ChristoSilvia/mappings/master/_assets/vertices_level_1.png)

And so on.
If we're working with an mth level approximation, we're going to represent
	a function by storing an array of its values on the mth level
	graph approximation to the serpinski gasket.
The formula we need to use includes differences between a vertex and its
	neighbors.
Therefore, for every element of a function's array representaiton,
	we need a way of efficiently figuring out which elements are adjacent
	to it.

We accomplish this by indexing the serpinski gasket with a space-filling path.
We will create a path which touches every point in the mth-level serpinski gasket
	exactly once (note that this path is a spanning tree).
We will refer to this path as the "spiked path".
The path will start at one endpoint and finish at the other.
We will then construct another path which starts and ends at the same points
	as the first path, does not touch the third boundary point at all,
	touches all of the vertices, and does not use any of the edges
	that the first path used.
We will refer to this path as the "flat path".
We can inductively construct these paths.

### Base Case

Consider the paths:

![base-spike](https://raw.githubusercontent.com/ChristoSilvia/mappings/master/_assets/flat_0.png)

and

![base-flat](https://raw.githubusercontent.com/ChristoSilvia/mappings/master/_assets/flat_0.png)

Note that the spiked path touches every point once.
That path touches two corners once and one corner twice.
Next, note that the flat path starts and ends at the same place as the
	spiked path, and doesn't touch the one other vertex.
It (trivially) touches every other point.
Therefore, for the 0th level serpinski gasket, we have constructed
	paths which satisfy our conditions.




