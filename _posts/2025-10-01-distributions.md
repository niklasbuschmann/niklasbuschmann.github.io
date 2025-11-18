---
title: "Common probability distributions"
date: 2025-10-01
description: Common probability distributions
mathjax: true
layout: post
---

### Probability of finding a sum of random variables
 
The probability density \\( p_n(x) \\) of finding the average \\( x = \frac{\sum_i x_i}{n} \\) of \\( n \\) random variables can be written using the [Delta function](https://en.wikipedia.org/wiki/Dirac_delta_function) as:

$$
  p_n(x) = \int \delta\left(x-\sum_i \frac{x_i}{n}\right)p_1(x_1) \dots p_n(x_n)\textrm{d}x_1 \dots \textrm{d}x_n  = \int \delta\left(x-\sum_i\frac{x_i}{n}\right)\prod_i p_i(x_i) \textrm{d}x_i
$$

Calculating the Fourier transform \\( \hat{p}\_n(k) \\) yields:

$$
  \hat{p}_n(k) = \int e^{-ikx}p_n(x)\textrm{d}x = \int e^{-ik \sum_i x_i/n}\prod_i p_i(x_i) \textrm{d}x_i = \prod_i \int  e^{-ikx_i/n} p_i(x_i) \textrm{d}x_i = \prod_i \hat{p}_i(k/n)
$$

The original \\( p_n(x) \\) can now be recovered using the inverse Fourier transform:

$$
  p_n(x) = \frac{1}{2\pi} \int e^{ikx}\hat{p}_n(k)\textrm{d}k
  = \frac{1}{2\pi} \int e^{ikx}\prod_i \hat{p}_i(k/n)\textrm{d}k
  = \frac{1}{2\pi} \int e^{ikx} \left({p}_i(k/n)\right)^n \textrm{d}k
$$

Where the last equality holds for identically distributed random variables.

### Normal distribution

Taylor expanding any probability density \\( \hat{p}(k/n) \\) around \\( k = 0 \\) yields:

$$
  \hat{p}(k/n) = \sum_m \frac{\partial_k^m \hat{p}(0)}{m!}\left(\frac{k}{n}\right)^m = \frac{\mathbb{E}[x_i^m]}{m!}\left(\frac{ik}{n}\right)^m = 1 + \frac{ik \mathbb{E}[x_i]}{n} + \frac{i^2k^2\mathbb{E}[x_i^2]}{2n^2} + \dots
$$

Comparing up to first order in \\( \frac{1}{n} \\) one gets:

$$
  (\hat{p}(k/n))^n = \left(1 + \frac{ik \mathbb{E}[x_i]}{n} + \frac{i^2k^2\mathbb{E}[x_i^2]}{2n^2} + \dots \right)^n = e^{ik \mathbb{E}[x_i]}e^{i^2k^2(\mathbb{E}[x_i^2]-\mathbb{E}[x_i]^2)/2n} + O\left(\frac{1}{n^2}\right)
$$

The original \\( p_n(x) \\) can now be recovered using the inverse Fourier transform:

$$
\begin{aligned}
  p_n(x) &\overset{n\rightarrow\infty}{\rightarrow} \frac{1}{2\pi} \int e^{ikx}e^{ik \mathbb{E}[x_i]}e^{i^2k^2(\mathbb{E}[x_i^2]-\mathbb{E}[x_i]^2)/2n}\textrm{d}k \\
  &= \frac{\exp\left(-\frac{(x-\mathbb{E}[x_i])^2}{2(\mathbb{E}[x_i^2]-\mathbb{E}[x_i]^2)/n)}\right)}{\sqrt{2\pi(\mathbb{E}[x_i^2]-\mathbb{E}[x_i]^2)/n}} \\
  &= \frac{1}{\sqrt{2\pi\sigma^2/n}}e^{-\frac{(x-\mu)^2}{2\sigma^2/n}}
\end{aligned}
$$

The average of \\( n \\) identically distributed random variables will for large \\( n \\) become normally distributed with the same mean \\( \mu \\) and lower variance \\( \sigma^2/n \\) compared to the original distribution.

### \\( \chi^2 \\) distribution

The probability of finde the sum of \\( n \\) squares of normally distributed variables with mean \\( \mu = 0\\) is proportional to the surface area of a [\\( n \\) dimensional sphere](https://en.wikipedia.org/wiki/N-sphere#Volume_and_area):

$$
  p(\chi^2) = \delta\left(\chi^2-\sum_i\frac{x_i^2}{2\sigma^2}\right)\prod_i \int\frac{e^{-\frac{x_i^2}{2\sigma^2}}}{\sqrt{2\pi \sigma^2}}\textrm{d}x_i = \frac{e^{-\chi^2}}{\sqrt{2\pi \sigma^2}^n} A(r=\chi^2;n-1) = \frac{\chi^{n-1}}{\Gamma(n/2)}\frac{e^{-\chi^2}}{\sigma^{n}}
$$

### Multinomial coefficient

Suppose you wants to arrange \\( k_i \\) copys of \\( i \\) different things into \\( n = \sum_i k_i \\) bins. How many distinct ways \\( \binom{n}{k_1,\dots,k_i} \\) are there to do this?

Starting with the case that each thing is unique (all \\( k_i\\) = 1), then the first bin can be filled with one of \\( n \\) things, the second with \\( (n-1) \\) remaining things, and so on, yielding \\( n! \\) distinct possibilities:

$$
  \binom{n}{k_1=1,\dots,k_i=1} = n(n-1)(n-2)(\dots) = n!
$$

Grouping together \\( k_i \\) of the previously unique things into a category \\(i\\) reduces the amount by a factor of \\( k_i! \\) since there are now \\( k_i \\) ways to select the first thing out of the category \\(i\\), and \\( k_i - 1 \\) ways to select the second, and so on, yielding an overall result of:

$$
  \binom{n}{k_1,\dots,k_i}  = \frac{n!}{k_1!,\dots,k_i!}= \frac{n!}{\prod_i k_i!}
$$

> What if multiple things can go into one bin? This problem can be reduce to the previous problem by adding \\( n-1 \\) separators to the number of bins and treating the separators as an additional category of thing that can be place in a single-occupied bin.

#### Multinomial distribution

Now what is the probability \\( P(k_1,\dots,k_i) \\) when measuring \\( n = \sum_i k_i \\) outcomes that there will be \\( k_i \\) outcomes of each type \\( i \\)? With the probabilities \\( p_i \\) of measuring outcome \\( i \\) in a single observation the total proability \\( P \\) is then the number of possibilities realizing this outcome multiplied by the product all of the \\( p_i \\):

$$
  P(k_1,\dots,k_i) = \binom{n}{k_1,\dots,k_i} \prod_i p_i^{k_i} = n!\prod_i \frac{p_i^{k_i}}{k_i!}
$$

> For two possible outcomes with \\( k_1 + k_2 = n \\) and \\( p_1 + p_2 = 1 \\) one recovers the **binomial distribution**:
>
> $$ P(n, k) = \frac{n!}{k!(n-k)!}p^k(1-p)^{n-k} $$
>

### Poisson distribution

Now taking the limit \\( n \rightarrow \infty \\) and \\( p_i \rightarrow 0 \\) while keeping the expectation values \\( \lambda_i = n \cdot p_i \\) constant yields the Poisson distribution. With the requirement \\( \sum_i p_i = 1 \\) we set \\( p_0 = 1-\sum_i p_i = 1-\sum_i \frac{\lambda_i}{n} \\) as the probability of nothing happening during one of the \\( n \\) obervations and \\( k_0 = n-\sum_i k_i \\) as the number of times this happens. The probability of observing each event \\( {k_i} \\) times is then given by:

$$
\begin{aligned}
  P(k_1,\dots,k_i) &= n!\frac{p_0^{k_0}}{k_0!}\prod_i \frac{p_i^{k_i}}{k_i!}  \\
  &= n!\frac{\left(1-\frac{\sum_i \lambda_i}{n}\right)^{n-\sum_i k_i}}{(n-\sum_i k_i)!}\prod_i \frac{1}{k_i!}\left(\frac{\lambda_i}{n}\right)^{k_i} \\
  &= \frac{n!}{(n-\sum_i k_i)!} \left(n\left(1- \frac{\sum_i\lambda_i}{n}\right)\right)^{-\sum_i k_i} \left(1-\frac{\sum_i \lambda_i}{n}\right)^n \prod_i \frac{\lambda_i^{k_i}}{k_i!} \\
  &\overset{n\rightarrow\infty}{\rightarrow} \prod_i \frac{\lambda_i^{k_i}}{k_i!}e^{-\lambda_i}
\end{aligned}
$$

### Geometric distribution

The probability of having the first success in a binomial setup after exactly \\( k \\) trials is simply given by:

$$
  P(k) = (1-p)^k p
$$

#### Exponential distribution

Writing \\( x = \frac{k}{n} \\) and \\( p = \frac{\lambda}{n} \\) and taking the limit \\( n \rightarrow \infty \\) yields the waiting time distribution in a Poisson process:

$$
  p(x) = \left(1-\frac{\lambda}{n}\right)^{xn} \frac{\lambda}{n} \overset{n\rightarrow\infty}{\rightarrow} e^{-\lambda x}\lambda\textrm{d}x
$$

Here the remaining factor \\( \frac{1}{n} \\) is removed by the normalisation of the density to 1.

### Erlang distribution

The cumulative waiting time for \\( n \\) events is the sum of \\( n \\) exponential distributions. Since the exponential distribution is a special case of a \\( \chi^2 \\) distribution with \\( k=2 \\) we can get the sum of \\( n \\) exponential distributions simply as \\( \chi^2 \\) distribution with \\( k=2n \\):

$$
  p(\chi^2=\lambda x, k=2n) = \frac{(\lambda x)^{n-1}}{\Gamma(n)}e^{-\lambda x}\frac{\textrm{d}\chi^2}{\textrm{d}x} = \frac{\lambda^n x^{n-1}}{(n-1)!}e^{-\lambda x}
$$
