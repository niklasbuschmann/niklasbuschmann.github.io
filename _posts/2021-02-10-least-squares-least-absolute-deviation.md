---
title: "Least squares and least absolute deviations"
date: 2021-02-10
description: A comparison between two methods of linear regression
mathjax: true
layout: post
---

When doing a linear regression, the result can be computed by requiring the estimated parameters \\( \theta_j \\) to minimize the [sum of squared differences](https://en.wikipedia.org/wiki/Ordinary_least_squares) between the measured values \\( y_i \\) and predicted values \\( f_{\theta}(x_i) \\):

$$
  S_2 \equiv \sum_i |f_{\theta}(x_i) - y_i|^2 \equiv \sum_i |\Delta_i|^2
$$

But this requirement seems kind of arbitrary - why not minimize the [sum of absolute differences](https://en.wikipedia.org/wiki/Least_absolute_deviations) instead?


$$
  S_1 \equiv \sum_i |f_{\theta}(x_i) - y_i|^1 \equiv \sum_i |\Delta_i|^1
$$

Or one could even go a step lower and minimize the following sum:

$$
  S_0 \equiv \sum_i |f_{\theta}(x_i) - y_i|^0 \equiv \sum_i |\Delta_i|^0
$$

For the sum \\( S_m \\) to be minimal requires the derivatives to vanish: \\( \partial_{\theta_i} S_n = 0 \\). If the model function \\( f \\) includes a constant \\( \alpha \\) so that \\( f_{\theta}(x) = g_{\theta}(x) + \alpha \\), then \\( \frac{\partial}{\partial_{\alpha}} S_n = \frac{\partial}{\partial f(x)} S_n \\) must be zero.

For the leat squares approach optimizing \\( \alpha \\) ensures that the **mean** difference between predicted and measured values is zero since:

$$
  \frac{\partial}{\partial \alpha } S_2 = \frac{\partial}{\partial f} \sum_i |\Delta_i|^2 = \sum_i \Delta_i \overset{!}{=} 0
$$

With the least absolute deviations approach instead, the optimization ensures that the **median** difference between predicted and measured values is zero: 

$$
  \frac{\partial}{\partial \alpha } S_1 = \frac{\partial}{\partial f} \sum_i |\Delta_i|^1 = \sum_i \frac{\Delta_i}{|\Delta_i|} = n(\Delta_i > 0) - n(\Delta_i < 0) \overset{!}{=} 0
$$

Optimizing \\( S_0 \\) will ensure that the difference has a  **mode** at zero:

$$
  \frac{\partial}{\partial \alpha } S_0 = \frac{\partial}{\partial f} \sum_i \left| \Delta_i \right|^0 = - \partial_{\alpha}n\left(\Delta_i = 0\right)\overset{!}{=} 0
$$

This explains why usually the least squares approach is chosen, because intuitively one would expect the model to be on average equal to the measurement. But when outliers are present, the least absolute deviations approach can be more useful, since optimizing the median is more robust to outliers.

Another comparison between the two approaches can be made using the [maximum likelihood method](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation), where least squares corresponds to [normally distributed](https://en.wikipedia.org/wiki/Normal_distribution) errors and least absolute deviations corresponds to [double exponentially distributed](https://en.wikipedia.org/wiki/Laplace_distribution) errors.

See also [Modes, Medians and Means: A Unifying Perspective](http://www.johnmyleswhite.com/notebook/2013/03/22/modes-medians-and-means-an-unifying-perspective/) as foundation of this post.
