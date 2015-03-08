---
layout: post
title:  "Configuring Cuda on AWS for Mumax3"
date:   2014-12-07 10:10:25
categories: magnets
---
I've been working on simulating magnetic behavior with [Graham Rowlands][graham] using [mumax3][mumax].  Mumax is a software package for simulating the magnetic dynamics of a material, and it uses GPU acceleration with cuda to get a considerable boost.  

Magnetic dynamics is particularly well suited for parallelization.  To simulate the magnetic behavior of a material, mumax breaks the material up into many small cubes (finite-difference approximation) and uses simple differential equations to simulate what will happen.  This problem is quite well suited for parallelization.

I thought it would be quite easy to spin up an aws environment with a good gpu(g2.2xlarge), but I wasted a huge amount of time trying to install CUDA and MUMAX from source.  Eventually, I just ended up using the community machine image `ami-2cbf3e44` which used instruction from [here][cudainst].  The provided ami worked quite well.

On to installing [mumax][mumax].  Mumax uses go, and the installation is just `go get -u -v github.com/mumax/3/...`.  However, that failed, and it wasn't immedietely obvious why.  It took some time, but eventually I figured out that I needed some libraries which weren't linked.  I learned that something called an `$LD_LIBRARY_PATH` existed.  I needed to download the libraries, and added the directory containing them to my `$LD_LIBRARY_PATH` in a way analagous to adding directories to `$PATH`.

Once I had added the libraries, mumax worked! I could produce some simple [examples][ex]:

This is a simulation of a magnetic pole reversing.  The red represents regions magnetized right, the blue represents regions magnetized left.

![initial](/assets/m000000.png)

![middle](/assets/m000001.png)

![more](/assets/m000002.png)

![even more](/assets/m000003.png)

![moore](/assets/m000004.png)

![alan moore](/assets/m000005.png)

![alan cummings](/assets/m000006.png)

Happy Coding!

[graham]: www.grahamerowlands.com
[mumax]:  http://mumax.github.io
[cudainst]: http://tleyden.github.io/blog/2014/10/25/cuda-6-dot-5-on-aws-gpu-instance-running-ubuntu-14-dot-04/
[ex]: http://mumax.github.io/examples.html
