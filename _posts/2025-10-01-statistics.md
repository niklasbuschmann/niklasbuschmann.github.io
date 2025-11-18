---
title: "Notes on Statistics"
date: 2025-10-01
description: Notes on Statistics
mathjax: true
layout: post
---

## Part 1: Bayesian

### Bayes' Theorem

From the definition of the [conditional probability](https://en.wikipedia.org/wiki/Conditional_probability) \\( P(X\|Y) \equiv \frac{P(X \cap Y)}{P(Y)} \\) one gets:

$$
  P(X|Y)P(Y) = P(X \cap Y) = P(Y|X)P(X)
$$

This theorem - called Bayes' Theorem - can be used to estimate the probability \\( p(\theta\|x) \\) of a parameter being \\( \theta \\) after measuring a value \\( x \\) drawn from a probability distribution \\( p_{\theta}(x) \equiv p(x\|\theta) \\):

$$
  p(\theta|x) = \frac{p(x|\theta)p(\theta)}{p(x)} = \frac{p(x|\theta)p(\theta)}{\int p(x|\theta')p(\theta') \textrm{d}\theta'}
$$

Where the fact that conditional probabilities sum up to one was used, implying that:

$$
  p(x) = p(x)\int p(\theta|x)\textrm{d}\theta = \int p(x|\theta)p(\theta)\textrm{d}\theta
$$

The four probability densities are then:
 - the **posterior** \\( p(\theta\|x) \\) which is the probability of the parameter being \\( \theta \\) considering that \\( x \\) was measured
 - the **likelihood** \\( p(x\|\theta) \\) which is the probability of measuring \\( x \\) for a given parameter \\( \theta \\)
 - the **prior** \\( p(\theta) \\) which is the assumed distribution of the parameter \\( \theta \\) for many repeated experiments
 - the **marginal probability** \\( p(x) \\) of measuring \\( x \\) averaged over all possible parameters \\( \theta \\)

After the posterior has been found, the new probability \\( p(x\|\mathbf{x}) = \int p(x\|\theta)p(\theta\|\mathbf{x})\textrm{d}\theta \\) is called **posterior predictive distribution**.

Since the likelihood \\( p(\mathbf{x}\|\theta) = \prod_i p(x_i\|\theta) \\) becomes narrower and narrower for more measurements, the posterior becomes more independent of the prior with more measurements.

### Asymptotic behaviour of the likelihood

That the likelihood becomes narrower with more measurements can be seen by rewriting the likelihood function:

$$
  p(\mathbf{x}|\theta) = \prod_i p(x_i|\theta) 
  = e^{\log(\prod_i p(x_i|\theta))}
  = e^{\sum_i \log(p(x_i|\theta))}
$$

Taylor expanding the exponent around the maximum of the likelihood (where \\( \theta_{\textrm{ML}} \\) maximizes the (log) likelihood, which - by definition - is the maximum likelihood estimate of \\( \theta\\)):

$$
  \sum_i \log(p(x_i|\theta)) \approx \sum_i \log(p(x_i|\theta_{\textrm{ML}})) + \left(\theta-\theta_{\textrm{ML}}\right)^2\frac{\partial^2}{\partial \theta^2}\sum_i \log(p(x_i|\theta_{\textrm{ML}}))/2
$$

The first term is just a multiplicative constant that is removed by the normalisation of the posterior. By the [law of large numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers) the second term will converge to the expectation value of the second derivative:

$$
  \frac{1}{N}\sum_i \frac{\partial^2}{\partial \theta^2} \log(p(x_i|\theta)) \quad\overset{N\rightarrow\infty}{\rightarrow}\quad \mathbb{E}\left[\frac{\partial^2}{\partial \theta^2} \log(p(x_i|\theta_{\textrm{ML}}))\right] < 0
$$

The likelihood function thus asymptotically becomes a normal distribution with mean \\( \mu = \theta_{\textrm{ML}} \\) and variance  \\( \sigma^2 = N^{-1} \left\|\mathbb{E}\left[\frac{\partial^2}{\partial \theta^2} \log(p(x_i\|\theta_{\textrm{ML}}))\right]\right\|^{-1} \\):

$$
  p(\mathbf{x}|\theta) \quad\overset{n\rightarrow\infty}{\rightarrow}\quad p(\mathbf{x}|\theta_{\textrm{ML}}) e^{-\left(\theta-\theta_{\textrm{ML}}\right)^2 N \left|\mathbb{E}\left[\frac{\partial^2}{\partial \theta^2} \log(p(x_i|\theta_{\textrm{ML}}))\right]\right|/2}
$$

A more rigerous statement of this is known as the [Bernstein–von Mises theorem](https://en.wikipedia.org/wiki/Bernstein–von_Mises_theorem).

> The expectation value of the second derivative of the log likelihood is also known as the [Fisher information](https://en.wikipedia.org/wiki/Fisher_information).

### Choice of prior

A flat prior \\( p(\theta)=1 \\) is a sensible choice when having no prior information, but is not invariant under reparametrizations: For example \\( p(\sigma)\textrm{d}\sigma=1 \\) implies \\( p(\sigma^2)\textrm{d}\sigma^2=\frac{1}{2\sigma}\textrm{d}\sigma^2 \\), even though they are the same parameter. A more sensible choice of prior is one that maximizes the difference between prior and posterior distribution. This difference can be measured by the [Kullback-Leibler divergence](https://en.wikipedia.org/wiki/Kullback–Leibler_divergence):

$$
  \iint p(x)p(\theta|x)\log \frac{p(\theta|x)}{p(\theta)}\textrm{d}\theta\textrm{d}x
$$

A prior that maximizes this difference is the Jeffreys prior, which is square root of the Fisher information:

$$
  p(\theta) = \sqrt{\mathbb{E}\left[\left(\frac{\partial \log(p(x|\theta))}{\partial \theta}\right)^2\right]}
$$

### Credible intervals

From the posterior \\( p(\theta\|\mathbf{x}) \\) one can determine a range \\( [\theta_{\textrm{min}}, \theta_{\textrm{max}}]\\) in which \\( \theta \\) lies with probability \\( 1-\alpha \\), called credible interval:

$$
  P(\theta\leq \theta_{\textrm{max}}|\mathbf{x})=\int_{-\infty}^{\theta_{\textrm{max}}} p(\theta|\mathbf{x})\textrm{d}\theta \overset{!}{=} 1-\alpha/2 \overset{!}{=} \int^{\infty}_{\theta_{\textrm{min}}} p(\theta|\mathbf{x})\textrm{d}\theta = P(\theta\geq \theta_{\textrm{min}}|\mathbf{x})
$$

### Loss functions

Instead of stating the full posterior \\( p(\theta\|\mathbf{x}) \\), one can characterise the distribution by a value \\( \hat{\theta}(\mathbf{x}) \\) that minimizes on average a loss function \\( \int L(\hat{\theta}(\mathbf{x}),\theta)p(\theta\|\mathbf{x})\textrm{d}\theta \\) over the posterior \\( p(\theta\|x) \\). Possible loss functions include:

#### Mean squared error

The loss function \\( L=\left\|\hat{\theta}(\mathbf{x})-\theta\right\|^2 \\)  is minimized by \\( \hat{\theta}(\mathbf{x}) = \textrm{mean}_\theta[p(\theta\|x)] \\) since one requires:

$$
  \frac{\partial}{\partial\,\hat{\theta}(\mathbf{x})} \int \left|\hat{\theta}(\mathbf{x})-\theta\right|^2 p(\theta|\mathbf{x})\textrm{d}\theta = 2\left(\hat{\theta}(\mathbf{x})-\int \theta\,p(\theta|\mathbf{x})\textrm{d}\theta\right) \overset{!}{=} 0
$$

Where was used that \\( \hat{\theta}(\mathbf{x}) \\) must be independent of \\( \theta \\). Plugging the mean back into the average squared error shows that the error is then given by the variance of the posterior.

> This can alternatively be seen directly from the average loss function:
> 
> $$ \langle (\hat{\theta}(\mathbf{x})-\theta)^2 \rangle = \langle (\hat{\theta}(\mathbf{x})-\langle \theta \rangle)^2 + (\theta - \langle \theta \rangle)^2 \rangle \geq \langle (\theta - \langle \theta \rangle)^2 \rangle $$
>

#### Mean absolute error

The loss function \\( L=\left\|\hat{\theta}(\mathbf{x})-\theta\right\|^1 \\)  is minimized by \\( \hat{\theta}(\mathbf{x}) = \textrm{median}_\theta[p(\theta\|x)] \\) since one requires:

$$
\begin{aligned}
  \frac{\partial}{\partial\,\hat{\theta}(\mathbf{x})} \int \left|\hat{\theta}(\mathbf{x})-\theta\right|^1 p(\theta|\mathbf{x})\textrm{d}\theta
  &=  \frac{\partial}{\partial\,\hat{\theta}(\mathbf{x})} \left(\int_{\hat{\theta}(\mathbf{x})}^{\infty} \left(\hat{\theta}(\mathbf{x})-\theta\right)p(\theta|\mathbf{x})\textrm{d}\theta -\int_{-\infty}^{\hat{\theta}(\mathbf{x})} \left(\hat{\theta}(\mathbf{x})-\theta\right)p(\theta|\mathbf{x})\textrm{d}\theta\right)\\
  &= \int_{-\infty}^{\hat{\theta}(\mathbf{x})} p(\theta|\mathbf{x})\textrm{d}\theta - \int_{\hat{\theta}(\mathbf{x})}^{\infty} p(\theta|\mathbf{x})\textrm{d}\theta \overset{!}{=} 0
\end{aligned}
$$

Where by definition the median is the value for that the cumulative probabilities above and below are equal.

#### Mean 0-1 error

The loss function \\( L=\left\|\hat{\theta}(\mathbf{x})-\theta\right\|^0 \\)  is minimized by \\( \hat{\theta}(\mathbf{x}) = \textrm{mode}_\theta[p(\theta\|x)] \\) since one requires:

$$
\begin{aligned}
  \frac{\partial}{\partial\,\hat{\theta}(\mathbf{x})} \int \left|\hat{\theta}(\mathbf{x})-\theta\right|^0 p(\theta|\mathbf{x})\textrm{d}\theta &= \frac{\partial}{\partial\,\hat{\theta}(\mathbf{x})} \left(1 - \int \delta\left(\hat{\theta}(\mathbf{x})-\theta\right)p(\theta|\mathbf{x})\textrm{d}\theta\right) \\
  &= -\frac{\partial}{\partial\,\hat{\theta}(\mathbf{x})} p(\hat{\theta}(\mathbf{x})|x) \overset{!}{=} 0
\end{aligned}
$$

Luckily for unimodal and symmetric distributions (e.g. the normal distribution) the mean, median and mode are all the same. In the following we will concentrate on the mean squared error.

## Example: Estimating the parameters of a normal distribution

The likelihood function \\( p(x\|\mu,\sigma) \\) can be written with \\( \overline{x} = \sum_i \frac{x_i}{n} \\) as:

$$
  p(\mathbf{x}|\mu,\sigma) = \prod_i p(x_i|\mu,\sigma) = \prod_i \frac{e^{-\frac{(x_i-\mu)^2}{2\sigma^2}}}{\sqrt{2\pi \sigma^2}}
  = \frac{e^{-\sum_i \frac{(x_i-\mu)^2}{2\sigma^2}}}{\sqrt{2\pi \sigma^2}^n}
  = \frac{e^{- \frac{\sum_i(x_i-\overline{x})^2 + n(\overline{x}-\mu)^2}{2\sigma^2}}}{\sqrt{2\pi \sigma^2}^n}
$$

Using the Jeffreys prior \\( p(\mu) = 1 \\), \\( p(\sigma) = \frac{1}{\sqrt{2\pi\sigma^2}} \\) and integrating out \\( \mu \\) and \\( \sigma \\) yields:

$$
\begin{aligned}
  p(\mathbf{x}) &= \iint p(\mathbf{x}|\mu,\sigma)p(\mu)p(\sigma)\textrm{d}\mu\textrm{d}\sigma
  = \int\frac{e^{- \frac{\sum_i(x_i-\overline{x})^2 + n(\overline{x}-\mu)^2}{2\sigma^2}}}{\sqrt{2\pi \sigma^2}^{n+1}}\textrm{d}\mu\textrm{d}\sigma \\
  &= \int\frac{e^{-\frac{\sum_i(x_i-\overline{x})^2}{2\sigma^2}}}{\sqrt{n}\sqrt{2\pi \sigma^2}^n}\textrm{d}\sigma
  = \sqrt{\sum_i(x_i-\overline{x})^2}^{-(n-1)}\frac{\Gamma\left(\frac{n-1}{2}\right)}{\sqrt{n}\sqrt{2\pi^n}}
\end{aligned}
$$

The posterior of the mean is a [Student's t distribution](https://en.wikipedia.org/wiki/Student%27s_t-distribution) with \\( n-1 \\) degrees of freedom:

$$
\begin{aligned}
  p(\mu|\mathbf{x}) &= \frac{1}{p(x)}\int p(\mathbf{x}|\mu,\sigma)p(\mu)p(\sigma)\textrm{d}\sigma
  = \frac{1}{p(\mathbf{x})}\int\frac{e^{- \frac{\sum_i(x_i-\overline{x})^2 + n(\overline{x}-\mu)^2}{2\sigma^2}}}{\sqrt{2\pi \sigma^2}^{n+1}}\textrm{d}\sigma \\
  &= \frac{1}{p(\mathbf{x})}\frac{\Gamma\left(\frac{n}{2}\right)}{\sqrt{2\pi^{n+1}}}\sqrt{\sum_i(x_i-\overline{x})^2 + n(\overline{x}-\mu)^2}^{-n} \\
  &= \frac{\Gamma\left(\frac{n}{2}\right)\sqrt{n}}{\sqrt{\pi}\Gamma\left(\frac{n-1}{2}\right)\sqrt{\sum_i(x_i-\overline{x})^2}}\sqrt{1 + \frac{n(\overline{x}-\mu)^2}{\sum_i(x_i-\overline{x})^2}}^{-n}
\end{aligned}
$$

The posterior of the variance is an [inverse \\( \chi^2 \\) distribution](https://en.wikipedia.org/wiki/Inverse-chi-squared_distribution) with \\( n-1 \\) degrees of freedom:

$$
\begin{aligned}
  p(\sigma|\mathbf{x}) &= \frac{1}{p(\mathbf{x})}\int p(\mathbf{x}|\mu,\sigma)p(\mu)p(\sigma)\textrm{d}\mu
  = \frac{1}{p(\mathbf{x})}\int \frac{e^{- \frac{\sum_i(x_i-\overline{x})^2 + n(\overline{x}-\mu)^2}{2\sigma^2}}}{\sqrt{2\pi \sigma^2}^{n+1}}\textrm{d}\mu \\
  &= \frac{1}{p(\mathbf{x})} \frac{e^{- \frac{\sum_i(x_i-\overline{x})^2}{2\sigma^2}}}{\sqrt{n}\sqrt{2\pi \sigma^2}^n}
  = \sqrt{\sum_i(x_i-\overline{x})^2}^n\frac{\sqrt{2}}{\Gamma\left(\frac{n-1}{2}\right)}\frac{e^{- \frac{\sum_i(x_i-\overline{x})^2}{2\sigma^2}}}{\sqrt{2\sigma^2}^n}
\end{aligned}
$$

> Estimating the parameter \\( p \\) of a binomial trial with uniform prior \\( p(\theta)=1 \\) yields a mean of:
>
> $$ \hat{p}(x) = \frac{\int p p^x(1-p)^{n-x} \textrm{d}p}{\int p^x(1-p)^{n-x} \textrm{d}p} = \frac{x+1}{n+2} $$
>
> And a mean squared error of:
>
> $$ (\delta \hat{p})^2 = \frac{\int p^2 p^x(1-p)^{n-x} \textrm{d}p - \left(\int p p^x(1-p)^{n-x} \textrm{d}p\right)^2}{\int p^x(1-p)^{n-x} \textrm{d}p} = \frac{(n-x+1)(x+1)}{(n+3)(n+2)^2} $$
>

### Bayes factor


## Part 2: Frequentist

An alternative to the Bayesian method is to not assume a prior/posterior distribution for \\( \theta \\), instead relying on the mode of the likelihood function and looking at the distribution of the possible measurements \\( x \\) instead of \\( \theta \\).

### Maximum Likelihood

A way of parameter estimation is the maximum likelihood method where the estimator \\( \hat{\theta}(\mathbf{x}) \\) is given by the condition that \\( \hat{\theta}(\mathbf{x}) \\) maximizes the likelihood, implying \\( \frac{\partial p(\mathbf{x}\|\hat{\theta}(\mathbf{x}))}{\partial \theta} \overset{!}{=} 0 \\). One can Taylor expand the likelihood around the true parameter \\( \theta_0 \\):

$$
  \frac{\partial \log(p(\mathbf{x}|\theta_0))}{\partial \theta} \approx \underbrace{\frac{\partial \log(p(\mathbf{x}|\hat{\theta}(\mathbf{x})))}{\partial \theta}}_{0} + \frac{\partial^2 \log(p(\mathbf{x}|\hat{\theta}(\mathbf{x})))}{\partial \theta^2} (\theta_0-\hat{\theta}(\mathbf{x}))
$$

This difference converges by the central limit theorem, the law of large numbers and Slutsky's theorem to the following normal distribution:

$$
   \theta_0-\hat{\theta}(\mathbf{x}) = \frac{\frac{\partial \log(p(\mathbf{x}|\theta_0))}{\partial \theta}}{\frac{\partial^2 \log(p(\mathbf{x}|\hat{\theta}(\mathbf{x})))}{\partial \theta^2}} = \frac{\sum_i\frac{1}{N} \frac{\partial \log(p(x_i|\theta_0))}{\partial \theta}}{\sum_i \frac{1}{N} \frac{\partial^2 \log(p(x_i|\hat{\theta}(\mathbf{x})))}{\partial \theta^2}} \overset{n\rightarrow\infty}{\rightarrow} \frac{\mathcal{N}\left(0, \mathbb{E}\left[\left(\frac{\partial \log(p(x|\theta_0))}{\partial \theta}\right)^2\right]/N\right)}{\mathbb{E}[\frac{\partial^2 \log(p(x|\hat{\theta}(\mathbf{x})))}{\partial \theta^2}]}
$$

Where was used that the derivative of the logarithm vanishes in expectation:

$$
  \mathbb{E}\left[\frac{\partial \log(p(x|\theta_0))}{\partial \theta}\right] = \int p(x|\theta_0)\frac{\partial \log(p(x|\theta_0))}{\partial \theta}\textrm{d}x = \int \frac{\partial}{\partial \theta}p(x|\theta_0)\textrm{d}x = \frac{\partial }{\partial \theta} \mathbb{E}[1] = 0
$$

Similarly:

$$
  \mathbb{E}\left[\frac{\partial^2 \log(p(x|\hat{\theta}(\mathbf{x})))}{\partial \theta^2}\right] = \mathbb{E}\left[\frac{\partial}{\partial \theta} \left(\frac{\partial p(x|\hat{\theta}(\mathbf{x}))}{\partial \theta}\frac{1}{p(x|\hat{\theta}(\mathbf{x}))}\right)\right] = \frac{\partial^2 \mathbb{E}[1]}{\partial \theta^2}-\mathbb{E}\left[\left(\frac{\partial \log(p(x|\hat{\theta}(\mathbf{x})))}{\partial \theta}\right)^2\right]
$$

If the estimator converges to the true value \\( \hat{\theta}(x) \rightarrow \theta_0 \\), then:

$$
   \theta_0-\hat{\theta}(\mathbf{x}) \quad\overset{n\rightarrow\infty}{\rightarrow}\quad \mathcal{N}\left(\mu = 0, \frac{1}{\sigma^2} = N\mathbb{E}\left[\left(\frac{\partial \log(p(x|\theta_0))}{\partial \theta}\right)^2\right]\right)
$$


### Confidence Intervals

The confidence interval \\( [\hat{\theta}\_\textrm{min},\hat{\theta}\_\textrm{max}] \\) is the analog of the credible interval, where after repeated measurements the true parameter \\( \theta \\) is included \\( 1-\alpha \\) of the time. For a given \\( x \\) the confidence interval can be found from:

$$
  P(X\leq x|\hat{\theta}_{\textrm{min}})=\int_{-\infty}^x p(x'|\hat{\theta}_{\textrm{min}})\textrm{d}x' \overset{!}{=} 1-\alpha/2 \overset{!}{=} \int_x^{\infty} p(x'|\hat{\theta}_{\textrm{max}})\textrm{d}x' = P(X\geq x|\hat{\theta}_{\textrm{max}})
$$

### Mean squared error

$$
  \int \left(\hat{\theta}(\mathbf{x})-\theta\right)^2 p(x|\theta)\textrm{d}x \equiv \langle(\hat{\theta}(\mathbf{x})-\theta)^2\rangle_x = (\underbrace{\langle\hat{\theta}(\mathbf{x})\rangle_x-\theta}_{\textrm{bias}})^2+\underbrace{\langle\hat{\theta}(\mathbf{x})^2\rangle_x-\langle\hat{\theta}(\mathbf{x})\rangle_x^2}_{\textrm{variance}}
$$

> Estimating for example the parameter \\( p \\) of a binomial trial with result \\( k = x \\):
>
> $$ \hat{p}(x) = \frac{x}{n} $$
>
> $$ (\delta \hat{p})^2 = \sum_x\binom{n}{x}p^x(1-p)^{n-x}\left(\frac{x}{n}-p\right)^2 = \frac{p-p^2}{n} $$

### Cramer Rao bound

Defining the scalar product \\( \langle f(x)g(x) \rangle \equiv \mathbb{E}[f(x)g(x)]\\) one gets from the [Cauchy–Schwarz inequality](https://en.wikipedia.org/wiki/Cauchy–Schwarz_inequality) that the variance of an estimator \\( \hat{\theta}(\mathbf{x}) \\) is bounded by the [Fisher information](https://en.wikipedia.org/wiki/Fisher_information):

$$
  \left(\partial_\theta\mathbb{E}[\hat{\theta}(\mathbf{x})]\right)^2 = \mathbb{E}\left[\left(\hat{\theta}(\mathbf{x})-\theta\right) \partial_\theta \log(p(x|\theta))\right]^2 \leq \mathbb{E}\left[\left(\hat{\theta}(\mathbf{x})-\theta\right)^2\right]\mathbb{E}\left[\left(\partial_\theta \log(p(x|\theta))\right)^2\right]
$$

Where the first equality follows from the interchange of integration and differentiation:

$$
\begin{aligned}
  \mathbb{E}\left[\left(\hat{\theta}(\mathbf{x})-\theta\right) \partial_\theta \log(p(x|\theta))\right] &= \int p(x|\theta)\left(\hat{\theta}(\mathbf{x})-\theta\right) \partial_\theta \log(p(x|\theta)) \textrm{d}x \\
  &= \int \left(\hat{\theta}(\mathbf{x})-\theta\right) \partial_\theta p(x|\theta) \textrm{d}x \\
  &= \partial_\theta\int \hat{\theta}(\mathbf{x}) p(x|\theta) \textrm{d}x - \theta\partial_\theta\int p(x|\theta) \textrm{d}x \\
  &= \partial_\theta \mathbb{E[\hat{\theta}(\mathbf{x})]}
\end{aligned}
$$

> The Cauchy-Schwarz inequality follows from:
>
> $$ \frac{1}{2}\ \mathbb{E}\left[\left(\frac{x}{\sqrt{\mathbb{E}[x^2]}} - \frac{y}{\sqrt{\mathbb{E}[y^2]}}\right)^2\right] = 1 - \frac{\mathbb{E}[xy]}{\sqrt{\mathbb{E}[x^2]\mathbb{E}[y^2]}} \geq 0 $$
>

### Linear regression

### Likelihood ratio test
