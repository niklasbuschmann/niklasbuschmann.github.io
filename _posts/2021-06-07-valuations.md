---
title: "Valuing assets with different volatilities"
date: 2021-06-07
description: Discounted cash flows and the relationship between arithmetic and geometric mean of a lognormal distribution
mathjax: true
layout: post
---

Assuming someone approached you and offered to sell you his business: How much should you be willing to pay for it?

Intuitively one might estimate that he can get 10% returns p.a. on the stock market, so 10x the amount of income generated yearly would seem like a reasonable price. In the following we will look deeper into this question: first for a riskless return and later with more volatile returns.


### Table of contents

- [Riskless Investments](#part-1-a-riskless-investment)
- [Volatile Investments](#part-2-volatility)
- [How much to invest?](#part-3-how-much-to-invest)
- [Correlated assets](#part-4-portfolio-of-correlated-assets)

## Part 1: A riskless investment

Let us first assume that we can forecast the future yearly incomes with perfect accuracy, making it a riskless investment. Now how are riskless investments valued? Consider that $1 invested now at the risk free (central bank) rate of 4% is worth $1.04 in one year. Conversely $1 in one year should be worth roughly $0.96 now. The same holds true for longer time frames:

$$
  P = \sum_{n=1}^N \frac{C_n}{(1+i)^n}
$$

This is the [discounted cash flow](https://en.wikipedia.org/wiki/Discounted_cash_flow) formula, where the current price \\( P \\) depends on the future yearly incomes \\( C_n \\) and the interest rate \\( i \\). We simply discount all the cash received by a factor \\( (1+i)^{-n} \\), assuming that we could have instead gotten the same cash by compounding the interest at the current market rate of \\( i \\).

> Assuming a constant income \\( C_n = C \\), the [geometric series](https://en.wikipedia.org/wiki/Geometric_series) yields:
>
> $$ P = \sum_{n=1}^N \frac{C}{(1+i)^n} = \frac{C}{i}\left(1 - \frac{1}{(1+i)^N}\right) \overset{N\rightarrow \infty}{\rightarrow}\ \frac{C}{i} $$
>

So when interest rates rise, the valuation of the business declines and vice versa.

For a constant yearly income we should now be willing to pay the annual income divided by the current long-term interest rate of a riskless investment.

> Assuming that the income is paid continuously instead of once every year, one would need to replace the sum above with an integral:
>
> $$ P = \int_0^t \frac{C}{(1+i)^n} \textrm{d}n = \frac{C}{\ln(1+i)}\left(1 - \frac{1}{(1+i)^t}\right) \overset{t\rightarrow \infty}{\rightarrow}\ \frac{C}{\ln(1+i)} $$
>

## Part 2: Volatility

Until now we assumed there was a guaranteed, constant return on our investment. Let us now instead assume that the expected future income follows a distribution with mean \\( \mu \\) and variance \\( \sigma^2 \\), and our business is listed on the stock market. Heuristically we can not lose more than our complete investment - conflicting with a [normally distributed](https://en.wikipedia.org/wiki/Normal_distribution) return. Instead we will assume a [log-normally distributed](https://en.wikipedia.org/wiki/Log-normal_distribution) return, which empirically works somewhat well for actual stock returns.

The crucial part is now the relationship between the [geometric](https://en.wikipedia.org/wiki/Geometric_mean) and [arithmetic](https://en.wikipedia.org/wiki/Arithmetic_mean) mean:

$$
  \sqrt[N]{\prod_i x_i} = \sqrt[N]{e^{\ln\left(\prod_i x_i\right)}}  = e^{\frac{\sum_i\ln(x_i)}{N}} = e^{\mathbb{E}[\ln(x)]}
$$

Thus the geometric mean equals the exponential of the expectation value of the logarithm.

> For a continous distribution \\( f(x) \\) of returns the geometric mean can be calculated analogously from the [product integral](https://en.wikipedia.org/wiki/Product_integral#Type_II:_Geometric_integral):
> 
> $$ \prod x^{f(x)\textrm{dx}} = e^{\ln\left(\prod x^{f(x)\textrm{dx}}\right)} = e^{\int \textrm{dx} f(x)\ln(x)} = e^{\mathbb{E}[\ln(x)]} $$
>

Now, for a log-normal distribution \\( \ln(x) \\) is normally distributed with mean \\( \mu \\) and the [expectation value of the log-normal distribution](https://en.wikipedia.org/wiki/Log-normal_distribution#Arithmetic_moments) is \\( \mathbb{E}[x] = e^{\mu + \frac{\sigma^2}{2}} \\), yielding:

$$
  \mathbb{E}[\ln(x)] = \mu \qquad \ln(\mathbb{E}[x]) = \mu + \frac{\sigma^2}{2} =: \mu_a
$$

This gives the following important relationship between geometric and arithmetic growth rate:

$$
  \mu = \mu_a - \frac{\sigma^2}{2}
$$

We can see that the geometric growth rate \\( \mu \\) decreases with increasing volatility \\( \sigma \\).

> From [Jensen's inequality](https://en.wikipedia.org/wiki/Jensen%27s_inequality) follows that for any distribution:
>
> $$ \mathbb{E}[\ln(x)] < \ln(\mathbb{E}[x]) $$
> 

Let us look at some real data, for example the [S&P 500](https://datahub.io/core/s-and-p-500) (with dividends reinvested) from 1970 to 2018:
<table><thead><tr><th>S&amp;P&nbsp;500</th><th>mean</th><th>std&nbsp;deviation</th><th>1971</th><th>1972</th><th>1973</th><th>1974</th><th>1975</th><th>1976</th><th>1977</th><th>1978</th><th>1979</th><th>1980</th><th>1981</th><th>1982</th><th>1983</th><th>1984</th><th>1985</th><th>1986</th><th>1987</th><th>1988</th><th>1989</th><th>1990</th><th>1991</th><th>1992</th><th>1993</th><th>1994</th><th>1995</th><th>1996</th><th>1997</th><th>1998</th><th>1999</th><th>2000</th><th>2001</th><th>2002</th><th>2003</th><th>2004</th><th>2005</th><th>2006</th><th>2007</th><th>2008</th><th>2009</th><th>2010</th><th>2011</th><th>2012</th><th>2013</th><th>2014</th><th>2015</th><th>2016</th><th>2017</th></tr></thead><tbody><tr><td>returns</td><td>1.12011</td><td>0.16695</td><td>1.13638</td><td>1.21875</td><td>0.83048</td><td>0.73735</td><td>1.38063</td><td>1.22471</td><td>0.93529</td><td>1.07670</td><td>1.18021</td><td>1.30147</td><td>0.97334</td><td>1.19073</td><td>1.23127</td><td>1.04603</td><td>1.31337</td><td>1.24101</td><td>0.99828</td><td>1.18744</td><td>1.30187</td><td>0.97557</td><td>1.22065</td><td>1.15488</td><td>1.09935</td><td>1.00406</td><td>1.38454</td><td>1.23561</td><td>1.31796</td><td>1.25506</td><td>1.21569</td><td>0.94235</td><td>0.87161</td><td>0.79777</td><td>1.22264</td><td>1.12801</td><td>1.07059</td><td>1.14250</td><td>1.06288</td><td>0.60684</td><td>1.30113</td><td>1.14014</td><td>1.02062</td><td>1.16769</td><td>1.29718</td><td>1.15845</td><td>1.02003</td><td>1.11716</td><td>1.20911</td></tr><tr><td>log&nbsp;returns</td><td>0.10068</td><td>0.16597</td><td>0.12785</td><td>0.19783</td><td>-0.18575</td><td>-0.30469</td><td>0.32254</td><td>0.20270</td><td>-0.06690</td><td>0.07390</td><td>0.16569</td><td>0.26349</td><td>-0.02702</td><td>0.17456</td><td>0.20805</td><td>0.04500</td><td>0.27260</td><td>0.21593</td><td>-0.00172</td><td>0.17180</td><td>0.26380</td><td>-0.02473</td><td>0.19938</td><td>0.14399</td><td>0.09472</td><td>0.00405</td><td>0.32537</td><td>0.21157</td><td>0.27608</td><td>0.22718</td><td>0.19531</td><td>-0.05937</td><td>-0.13741</td><td>-0.22593</td><td>0.20102</td><td>0.12046</td><td>0.06821</td><td>0.13322</td><td>0.06098</td><td>-0.49949</td><td>0.26324</td><td>0.13115</td><td>0.02041</td><td>0.15503</td><td>0.26019</td><td>0.14708</td><td>0.01983</td><td>0.11079</td><td>0.18989</td></tr></tbody></table>
Calculating the geometric growth rate from \\( \mu = \mu_a - \sigma^2 / 2 = \ln(1.12011) - 0.16597^2/2 = 0.09965 \\) (assuming log-normal returns) deviates by only 1% from the correct value \\( \mu = 0.10068 \\).

## Part 3: How much to invest?

Now, is an investment with small return and low volatility worse than one with high return and high volatility? If we have access to leverage we can lever up both the arithmetic return \\( \mu_a \\) and the standard deviation \\( \sigma \\). Assuming one maintains a constant leverage and accouting for the borrowing rate \\( r \\) yields the geometric growth rate:

$$
  \mu = l \mu_a - (l\sigma)^2/2 + (1 - l)r
$$

Optimizing the growth rate \\( \mu \\) leads to:

$$
  \frac{\partial \mu}{\partial l} = \mu_a - r - l\sigma^2 \overset{!}{=} 0 \quad \implies \quad l = \frac{\mu_a - r}{\sigma^2}
$$

With a maximum rate of:

$$
  \mu = \frac{(\mu_a-r)^2}{2\sigma^2} + r
$$

> This is the continuous analog of the [Kelly formula](https://en.wikipedia.org/wiki/Kelly_criterion), where one has a series of favorable, but risky bets.
> Imagine for example you are offered many bets where 40% of the time your wager is tripled and 60% of the time lost. How would you play?

## Part 4: Portfolio of correlated assets

So when valuing an asset we should discount assets with higher volatility and chose to maximate the ratio of arithmetic returns to volatility. But should we put all our money into the asset with the highest ratio or diversify?

For a portfolio of multiple uncorrelated assets the standard deviation has the [same form](https://en.wikipedia.org/wiki/Propagation_of_uncertainty#Example_formulae) as a vector: Adding a small, orthogonal (uncorrelated) step leaves its overall length unchanged, but increases the expected return, making a diversified portfolio more attractive than an undiversified one.

The geometric growth of the portfolio from above can be generalized for a number of correlated assets with allocations \\( l_i \\), arithmetic returns \\( \mu^a_i \\) and the covariance matrix \\( \hat{\sigma}_{ij} \\):

$$
  \mu = \sum_i l_i \mu^a_i - \frac{1}{2}\sum_{ij} l_i\hat{\sigma}_{ij}l_j + \left(1-\sum_i l_i\right)r = \vec{l}\cdot\vec{\mu}_a - \vec{l}^\intercal \hat{\sigma} \vec{l}/2 + (1-l)r
$$

Optimizing \\( \mu \\) again yields:

$$
  \vec{l} = \frac{\vec{\mu}_a - r}{\hat{\sigma}}
$$
