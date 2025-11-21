---
title: "Common probability distributions"
date: 2025-10-01
description: Common probability distributions
mathjax: true
layout: post
---

### Table of contents

- [Sum of random variables](#probability-of-finding-a-sum-of-random-variables)
- [Normal distribution](#normal-distribution)
- [\\( \chi^2 \\) distribution](#-chi--distribution)
- [Student t distribution](#student-t-distribution)
- [Multinomial distribution](#multinomial-coefficient)
- [Poisson distribution](#poisson-distribution)
- [Erlang distribution](#erlang-distribution)

### Probability of finding a sum of random variables
 
The probability density \\( \textrm{p}(\overline{x}) \\) of finding the average \\( \overline{x} = \frac{\sum_i x_i}{n} \\) of \\( n \\) random variables can be written using the [Delta function](https://en.wikipedia.org/wiki/Dirac_delta_function) as:

$$
  \textrm{p}(\overline{x}) = \int \delta\left(\overline{x}-\sum_i \frac{x_i}{n}\right)\textrm{p}_1(x_1) \dots \textrm{p}_n(x_n)\textrm{d}x_1 \dots \textrm{d}x_n  = \int \delta\left(\overline{x}-\sum_i\frac{x_i}{n}\right)\prod_i \textrm{p}_i(x_i) \textrm{d}x_i
$$

Calculating the Fourier transform \\( \hat{\textrm{p}}(k) \\) yields:

$$
  \hat{\textrm{p}}(k) = \int e^{-ik\overline{x}}\textrm{p}(x)\textrm{d}\overline{x} = \int e^{-ik \sum_i x_i/n}\prod_i \textrm{p}_i(x_i) \textrm{d}x_i = \prod_i \int  e^{-ikx_i/n} \textrm{p}_i(x_i) \textrm{d}x_i = \prod_i \hat{\textrm{p}}_i(k/n)
$$

The original \\( \textrm{p}(\overline{x}) \\) can now be recovered using the inverse Fourier transform:

$$
  \textrm{p}(\overline{x}) = \frac{1}{2\pi} \int e^{ik\overline{x}}\hat{\textrm{p}}(k)\textrm{d}k
  = \frac{1}{2\pi} \int e^{ik\overline{x}}\prod_i \hat{\textrm{p}}_i(k/n)\textrm{d}k
  = \frac{1}{2\pi} \int e^{ik\overline{x}} \left({\textrm{p}}_i(k/n)\right)^n \textrm{d}k
$$

Where the last equality holds for identically distributed random variables.

### Normal distribution

Taylor expanding any probability density \\( \hat{\textrm{p}}\_i(k/n) \\) around \\( k = 0 \\) yields:

$$
  \hat{\textrm{p}}_i(k/n) = \sum_m \frac{\partial_k^m \hat{\textrm{p}}_i(0)}{m!}\left(\frac{k}{n}\right)^m = \frac{\mathbb{E}[x_i^m]}{m!}\left(\frac{ik}{n}\right)^m = 1 + \frac{ik \mathbb{E}[x_i]}{n} + \frac{i^2k^2\mathbb{E}[x_i^2]}{2n^2} + \dots
$$

Comparing up to first order in \\( \frac{1}{n} \\) one gets:

$$
  (\hat{\textrm{p}}(k/n))^n = \left(1 + \frac{ik \mathbb{E}[x_i]}{n} + \frac{i^2k^2\mathbb{E}[x_i^2]}{2n^2} + \dots \right)^n = e^{ik \mathbb{E}[x_i]}e^{i^2k^2(\mathbb{E}[x_i^2]-\mathbb{E}[x_i]^2)/2n} + O\left(\frac{1}{n^2}\right)
$$

Writing \\( \mu \equiv \mathbb{E}[x_i] \\) and \\( \sigma^2 \equiv \mathbb{E}[x_i^2]-\mathbb{E}[x_i]^2 \\), the original \\( \textrm{p}(\overline{x}) \\) can now be recovered using the inverse Fourier transform:

$$
  \textrm{p}(\overline{x}) \overset{n\rightarrow\infty}{\rightarrow} \frac{1}{2\pi} \int e^{ik\overline{x}}e^{ik \mu}e^{i^2k^2\sigma^2/2n}\textrm{d}k
  = \frac{1}{\sqrt{2\pi\sigma^2/n}}e^{-\frac{(\overline{x}-\mu)^2}{2\sigma^2/n}}
$$

The average of \\( n \\) identically distributed random variables will for large \\( n \\) become normally distributed with the same mean \\( \mu \\) and lower variance \\( \sigma^2/n \\) compared to the original distribution.

### \\( \chi \\) distribution

The probability of finding the root of the sum of squares of \\( n \\) standard-normal distributed random variables is proportional to the surface area of a [\\( n \\) dimensional sphere](https://en.wikipedia.org/wiki/N-sphere#Volume_and_area):

$$
  \textrm{p}(\chi) = \int\delta\left(\chi-\sqrt{\sum_ix_i^2}\right)\prod_i \frac{e^{-x_i^2/2}}{\sqrt{2\pi}}\textrm{d}x_i
  = \int\textrm{d}A\int\delta(\chi-r)\frac{e^{-r^2/2}}{\sqrt{2\pi}^n}r^{n-1}\textrm{d}r
  = \frac{e^{-\chi^2/2}\chi^{n-1}}{\sqrt{2^{n-2}}\Gamma\left(\frac{n}{2}\right)}
$$

> The surface area of the n-dimensional sphere can be calculated from:
>
> $$ 1=\int\prod_i \frac{e^{-x_i^2/2}}{\sqrt{2\pi}}\textrm{d}x_i = \int \textrm{d}A \int \frac{e^{-r^2/2}r^{n-1}}{\sqrt{2\pi}^n}\textrm{d}r = \int \textrm{d}A \int \frac{e^{-t}t^{\frac{n}{2}-1}}{2\sqrt{\pi^n}}\textrm{d}t \equiv \frac{\Gamma\left(\frac{n}{2}\right)}{2\sqrt{\pi^n}} \int \textrm{d}A $$
>

#### \\( \chi^2 \\) distribution

A change of variables yields the distribution for the sum of squares:

$$
  \textrm{p}(\chi^2) = \textrm{p}(\chi)\frac{\textrm{d} \chi}{\textrm{d} \chi^2} = \frac{e^{-\chi^2/2}\chi^{n-2}}{\sqrt{2^n}\Gamma\left(\frac{n}{2}\right)}
$$

### Student t distribution

The estimated mean \\( \overline{x} = \frac{\sum_i x_i}{n} \\) deviates from the true mean \\( \mu \\) with a standard deviation of \\( \frac{\sigma}{\sqrt{n}} \\). Since \\( \sigma \\) ist most likely unknown one has to use the estimated variance \\( \frac{s}{\sqrt{n}} \\) instead. Since \\( \frac{\overline{x}-\mu}{\sigma/\sqrt{n}} \\) ist standardnormal distributed and \\( \frac{s^2}{\sigma^2} \\) is \\( \chi^2 \\) distributed \\( \frac{\overline{x}-\mu}{s/\sqrt{n}} \\) is independent of \\( \sigma \\) and follows a Student t distribution:

$$
\begin{aligned}
  p(t) &= \int\delta\left(t-\frac{x}{\sqrt{\chi^2/n}}\right)\frac{e^{-x^2/2}}{\sqrt{2\pi}}\frac{e^{-\chi^2/2}\chi^{n-2}}{\sqrt{2^n}\Gamma\left(\frac{n}{2}\right)}\textrm{d}\chi^2\textrm{d}x \\
  &= \int\frac{e^{-(t\chi)^2/2n}}{\sqrt{2\pi n}}\frac{e^{-\chi^2/2}\chi^{n-2}}{\sqrt{2^n}\Gamma\left(\frac{n}{2}\right)}\chi\textrm{d}\chi^2 \\
  &= \int\frac{e^{-\chi^2(1+t^2/n)/2}}{\sqrt{2^{n+1}\pi n }}\frac{(\chi^2)^{\frac{n-1}{2}}}{\Gamma\left(\frac{n}{2}\right)}\textrm{d}\chi^2 \\
  &= \frac{\int e^{-u}u^{\frac{n-1}{2}}\textrm{d}u }{\sqrt{\pi n }\Gamma\left(\frac{n}{2}\right)\left(1+\frac{t^2}{n}\right)^{\frac{n+1}{2}}} \\
  &= \frac{\Gamma\left(\frac{n+1}{2}\right)}{\sqrt{\pi n }\Gamma\left(\frac{n}{2}\right)}\left(1+\frac{t^2}{n}\right)^{-\frac{n+1}{2}}
\end{aligned}
$$

The variance of \\( \frac{\overline{x}-\mu}{s/\sqrt{n}} \\) is no longer 1 but \\( \frac{n}{n-2} \\) instead.

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

Now what is the probability \\( \textrm{P}(k_1,\dots,k_i) \\) when measuring \\( n = \sum_i k_i \\) outcomes that there will be \\( k_i \\) outcomes of each type \\( i \\)? With the probabilities \\( p_i \\) of measuring outcome \\( i \\) in a single observation the total proability \\( P \\) is then the number of possibilities realizing this outcome multiplied by the product all of the \\( p_i \\):

$$
  \textrm{P}(k_1,\dots,k_i) = \binom{n}{k_1,\dots,k_i} \prod_i p_i^{k_i} = n!\prod_i \frac{p_i^{k_i}}{k_i!}
$$

> For two possible outcomes with \\( k_1 + k_2 = n \\) and \\( p_1 + p_2 = 1 \\) one recovers the **binomial distribution**:
>
> $$ \textrm{P}(n, k) = \frac{n!}{k!(n-k)!}p^k(1-p)^{n-k} $$
>

### Poisson distribution

Now taking the limit \\( n \rightarrow \infty \\) and \\( p_i \rightarrow 0 \\) while keeping the expectation values \\( \lambda_i \equiv n \cdot p_i \\) constant yields the Poisson distribution. With the requirement \\( \sum_i p_i = 1 \\) we set \\( p_0 \equiv 1-\sum_i p_i = 1-\sum_i \frac{\lambda_i}{n} \\) as the probability of nothing happening during one of the \\( n \\) obervations and \\( k_0 \equiv n-\sum_i k_i \equiv n-k \\) as the number of times this happens. The probability of observing each event \\( {k_i} \\) times is then given by:

$$
\begin{aligned}
  \textrm{P}(k_1,\dots,k_i) &= n!\frac{p_0^{k_0}}{k_0!}\prod_i \frac{p_i^{k_i}}{k_i!}  \\
  &= \frac{n!}{(n-k)!}p_0^{n-k}\frac{1}{n^k}\prod_i \frac{\lambda_i^{k_i}}{k_i!} \\
  &= \underbrace{\frac{n!}{(n-k)!}\frac{1}{n^k}}_{\rightarrow\ 1}\underbrace{\left(1-\frac{\sum_i \lambda_i}{n}\right)^n}_{\rightarrow\ \exp\left(\sum_i \lambda_i\right)} \bigg({\underbrace{1-\frac{\sum_i \lambda_i}{n}}_{\rightarrow\ 1}} \bigg)^{-k}\prod_i \frac{\lambda_i^{k_i}}{k_i!} \\
  &\overset{n\rightarrow\infty}{\rightarrow} \prod_i \frac{\lambda_i^{k_i}}{k_i!}e^{\lambda_i}
\end{aligned}
$$

### Geometric distribution

The probability of having the first success in a binomial setup after exactly \\( k \\) trials is simply given by:

$$
  \textrm{P}(k) = (1-p)^k p
$$

#### Exponential distribution

Writing \\( x = \frac{k}{n} \\) and \\( p = \frac{\lambda}{n} \\) and taking the limit \\( n \rightarrow \infty \\) yields the waiting time distribution in a Poisson process:

$$
  \textrm{p}(x) = \left(1-\frac{\lambda}{n}\right)^{xn} \frac{\lambda}{n} \overset{n\rightarrow\infty}{\rightarrow} e^{-\lambda x}\lambda\textrm{d}x
$$

Here the remaining factor \\( \frac{1}{n} \\) is removed by the normalisation of the density to 1.

### Erlang distribution

The cumulative waiting time for \\( n \\) events is the sum of \\( n \\) exponential distributions. Since the exponential distribution is a special case of a \\( \chi^2 \\) distribution with \\( k=2 \\) we can get the sum of \\( n \\) exponential distributions simply as \\( \chi^2 \\) distribution with \\( k=2n \\):

$$
  \textrm{p}(\chi^2=2\lambda x, k=2n) = \frac{(2\lambda x)^{n-1}}{2^n\Gamma(n)}e^{-\lambda x}\frac{\textrm{d}\chi^2}{\textrm{d}x} = \frac{\lambda^n x^{n-1}}{(n-1)!}e^{-\lambda x}
$$
