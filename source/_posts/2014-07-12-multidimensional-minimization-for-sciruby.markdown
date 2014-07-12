---
layout: post
title: "Multidimensional minimization for SciRuby"
date: 2014-07-12 17:36
comments: true
author: Lahiru Lasandun
categories: [GSOC2014,GSOC,Integration,Students]
---
Multidimensional minimization methods
-------------------------------------

This is a new feature to SciRuby minimization gem. This gives some numerical methods to find local minimum of a multi variable function. These methods can be categorized into two groups: gradient methods and non-gradient methods. I have implemented four methods for SciRuby as below.

1) Gradient methods: Polak Ribbre method
		       Fletcher Reeves method
2) Non-Gradient methods: Nelder Mead algorithm (Downhill Simplex method)
			    Powell's method
Each of these methods has their own advantages and disadvantages. Gradient methods are faster than non-gradient methods in most cases. But the function should be differentiable.
On the other hand, non-gradient methods are more reliable than gradient methods.
Nelder Mead algorithm
---------------------
This requires a starting point to search minimum.
```ruby
min = Minimization::NelderMead.new(proc{|x| (x[0] + x[1])**2 + 5}, [1, 5])
while min.converging?
  min.minimize
end
min.x_minimum
min.f_minimum
```

Powell's method
--------------
This requires a starting point. In addition to that, the method should be given an upper bound and lower bound for each variable. (bounds for each axis direction)
```ruby
min = Minimization::PowellMinimizer.new(f, start_point, [-x1_lower, -x2_lower, -x3_lower], [x1_upper, x2_upper, x3_upper])
while(min.converging)
  min.minimize
end
min.x_minimum
min.f_minimum
```

Gradient methods
---------------
For above two gradient methods, the first derivative is enough. Technically it requires the second derivative. But it approximates the second derivatives itself.
```ruby
f  = proc{ |x| (x[0] - @p[0])**2 + (x[1] - @p[1])**2 + (x[2] - @p[2])**2 }
fd = proc{ |x| [ 2 * (x[0] - @p[0]) , 2 * (x[1] - @p[1]) , 2 * (x[2] - @p[2]) ] }
# method can be :fletcher_reeves or :polak_ribbre
min = Minimization::NonLinearConjugateGradientMinimizer.new(f, fd, start_point, :fletcher_reeves)
while(min.converging?)
  min.minimize
end
```
