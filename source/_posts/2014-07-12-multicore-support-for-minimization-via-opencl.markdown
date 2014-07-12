---
layout: post
title: "Multicore support for Minimization via OpenCL"
date: 2014-07-12 18:20
comments: true
author: Lahiru Lasandun
categories: [GSOC2014,GSOC,Integration,Students]
---
Multicore support for Minimization via OpenCL
---------------------------------------------
Minimization gem includes some numerical calculations which uses the processor time considerably. But those algorithms are written for single processing unit. Therefore with the guidance of mentors, I have been researching how Ruby can use multi-core support to solve these numerical problems. Finally we came to a conclusion that OpenCL will be the most suitable way to het multi-core power for Minimization and Integration gems.

Minimization algorithms are sequential. It's not possible to write those algorithms in parallel. But these algorithms finds only a local minimum. Therefore it can be given multiple start points and find local minimums for each start point. The best result can be taken as the final result.
OpenCL has two ends.

1) The OpenCL C program which executes the algorithm on the Processor or GPU.

2) The host program which runs on the processor which triggers the OpenCL C program. (C or C++)
I have currently replicated the Goden Section and Newton Rampson methods into OpenCL C and created a generic host program for all the unidimensional minimization methods.
Drawbacks
---------
The minimization function can't be given as a ruby proc or a lambda as the algorithm should be in OpenCL C language. In my implementation minimizing function should be given as a string taking the variable as 'x'.
```C
“pow(x,  2) – 2 * x + sin(x) ”
```
If the function includes complex mathamatical functions such as log, sin, cos, etc they should be given in OpenCL C syntaxes. Not in ruby format.
ToDo
----
Brent algorithm and bisection algorithm should be ported into OpenCL C.
A ruby end should be created for these C host programs.
