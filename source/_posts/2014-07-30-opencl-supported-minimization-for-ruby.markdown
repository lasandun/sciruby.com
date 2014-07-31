---
layout: post
title: "OpenCL supported minimization for Ruby"
date: 2014-07-30 23:19
comments: true
categories: [GSOC2014,GSOC,Minimization,Students]
---
OpenCL supported minimization for Ruby
--------------------------------------

This is a new feature to Minimization gem which is still being developed.
The original minimization methods finds a local minimum in a given interval. The basic idea of the OpenCL supported minimization is to
accept multiple intervals and minimize the given function in those intervals in parallel.
Currently, the OpenCL supported minimization has a different interface from the existing minimization interface. In future, OpenCL support may come with the existing minimization project which could be enabled manually.

Golden Section Method
```ruby
n              = 3
start_point    = [1, 3, 5]
expected_point = [1.5, 3.5, 5.5]
end_point      = [3, 5, 7]
f              = "pow((x-2)*(x-4)*(x-6), 2)+1"
min = OpenCLMinimization::GoldenSectionMinimizer.new(n, start_point, expected_point, end_point, f)
min.minimize
x_minimum
f_minimum
```

Bisection Method
```ruby
n              = 3
start_point    = [1, 3, 5]
expected_point = [1.5, 3.5, 5.5]
end_point      = [3, 5, 7]
f              = "pow((x-2)*(x-4)*(x-6), 2)+1"
min = OpenCLMinimization::BisectionMinimizer.new(n, start_point, expected_point, end_point, f)
min.minimize
x_minimum
f_minimum
```

Newton Rampson Method
```ruby
n              = 3
expected_point = [1, 100, 1000]
f              = "(x-3)*(x-3)+5"
fd             = "2*(x-3)"
fdd            = "2"
min = OpenCLMinimization::NewtonRampsonMinimizer.new(n, expected_point, f, fd, fdd)
min.minimize
x_minimum
f_minimum
```

Brent Method
```ruby
n              = 3
f              = "(x-55)*(x-55)+5"
start_point    = [1, 3, 5]
expected_point = [33, 55, 77]
end_point      = [100, 300, 500]
min = OpenCLMinimization::BrentMinimizer.new(n, start_point, expected_point, end_point, f)
min.minimize
x_minimum
f_minimum
```

Differences from the existing pure Ruby Minmiazation
----------------------------------------------------
The unidimensional minimizing function should be a string in new implementation. The variable of the function should be 'x'. Proc or lambda won't work. The syntax should be OpenCL C.

```ruby
f = "(x-55)*(x-55)+5"
There can be more than one set of intervals. Therefore the starting points and ending points are taken as arrays.
start_points    = [1, 3, 5]
expected_points = [33, 55, 77]
now there are 3 intervals which are (1, 33), (3, 55) and (5, 77)
```
How to Setup OpenCL
-------------------
To run OpenCL, the system needs,
* OpenCL supported hardware
* OpenCL headers 
* OpenCL shared library (comes with the device drivers)

Installation instruction for Linux platform
----------------------------------
To install OpenCL headers:

It's a good idea to install OpenCL headers using apt-get rather than doing it manually, as it will install correct versions depending on the hardware.
```
sudo apt-get install opencl-headers
```

To install Intel SDK:
Follow [this] (https://gist.github.com/rmcgibbo/6314452) link for a good guidance for installing Intel CPU OpenCL SDK.
It includes the libOpenCL.so shared library which is required to compile the OpenCL host program.

After setting up OpenCL successfully, run 'make' command in the lib directory. The makefile creates a C shared library.
If you got an error saying libOpenCL.so not found please set the give to the libOpenCL.so at the makefile.
In Linux, if you installed the opencl-headres using aptitude(apt-get) and followed the provided tutorial correctly you, no need to edit the makefile. Just let the parameters empty.


```make
CL_HEADERS_PATH=
LIBOPENCL_PATH=
```

After doing make, the OpenCL minimization is ready to run. Run the [spec file] (https://github.com/lasandun/minimization/blob/opencl/spec/minimization_unidimensional_opencl.rb) and make sure the OpenCL minimization has been setup successfully.

