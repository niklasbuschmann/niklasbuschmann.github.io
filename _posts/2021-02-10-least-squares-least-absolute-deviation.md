---
title: "Least squares and least absolute deviations"
date: 2021-02-10
description: A comparison between two methods of linear regression
---

When doing any kind of linear regression, the result can always be computed by requiring the estimated parameters \\( \theta_j \\) to minimize the [sum of squared differences](https://en.wikipedia.org/wiki/Ordinary_least_squares) between the measured values \\( y_i \\) and predicted values \\( f_{\theta}(x_i) \\):

$$
  S_1 = \sum_i |\Delta_i|^2 = \sum_i |y_i - f_{\theta}(x_i)|^2
$$

But this requirement always seemed kind of arbitrary to me - why not minimize the [sum of absolute differences](https://en.wikipedia.org/wiki/Least_absolute_deviations) for example?


$$
  S_2 = \sum_i |\Delta_i| = \sum_i |y_i - f_{\theta}(x_i)|
$$

In both cases the fact that the sum is minimized implies that the derivative with respect to any parameter \\( \theta_j \\) must vanish:

$$
  \frac{\partial S_1}{\partial \theta_j} = \sum_i\frac{\partial |\Delta_i|^2}{\partial \theta_j}  = -2 \sum_i \frac{\Delta_i}{1} \frac{\partial f_{\theta}(x_i)}{\partial \theta_j} = 0 \\[2ex]
  \frac{\partial S_2}{\partial \theta_j} = \sum_i\frac{\partial |\Delta_i|}{\partial \theta_j} = -\sum_i \frac{\Delta_i}{|\Delta_i|} \frac{\partial f_{\theta}(x_i)}{\partial \theta_j} = 0
$$

Let us now assume that the model function \\( f \\) includes a constant \\( c \\) so that \\( f_{\theta}(x_i) = g_{\theta}(x_i) + c \\). Since \\( \frac{\partial f_{\theta}}{\partial c} = 1 \\) this implies that:

$$
  \frac{\partial S_1}{\partial c} = -2\sum_i \Delta_i = 0 \quad \Rightarrow \quad \sum_i \frac{\Delta_i}{n} = 0
$$

We can see that - with least squares - the constant \\( c \\) ensures that the **mean** difference \\( \Delta_i \\) between predicted and measured values is zero.

Comparing this with the least absolute deviations approach yields: 

$$
  \frac{\partial S_2}{\partial c} = -\sum_i \frac{\Delta_i}{|\Delta_i|} = 0 \quad \Rightarrow \quad n(\Delta_i > 0) = n(\Delta_i < 0)
$$

Now - with least absolute deviations - the constant \\( c \\) ensures that the **median** difference between predicted and measured values is zero. Since \\( \frac{\Delta_i}{\|\Delta_i\|}=\pm 1 \\), depending on whether the difference is positive or negative, the amount of times the difference is lower and greater than zero must be the same.

The same applies to all other parameters: least squares optimizes the mean difference and least absolute deviations optimizes the median difference - only that now the differences are weighted by a factor \\( \frac{\partial f_{\theta}}{\partial \theta_j} \\) (which since \\(f_{\theta}\\) is linear in \\( \theta_j \\) only depends on \\( x_i\\)).

This explains why usually the least squares approach is chosen, because intuitively one would expect the model to be on average equal to the measurement. But when outliers are present, the least absolute deviations approach can be more useful, since optimizing the median is more robust to outliers.

Another comparison between the two approaches can be made using the [maximum likelihood method](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation), where least squares corresponds to [normally distributed](https://en.wikipedia.org/wiki/Normal_distribution) errors and least absolute deviations corresponds to [double exponentially distributed](https://en.wikipedia.org/wiki/Laplace_distribution) errors.

See also [Modes, Medians and Means: A Unifying Perspective](http://www.johnmyleswhite.com/notebook/2013/03/22/modes-medians-and-means-an-unifying-perspective/) as foundation of this post.
