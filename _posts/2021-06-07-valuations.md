---
title: "Valuing assets with different volatilities"
date: 2021-06-07
description: Discounted cash flows and the relationship between arithmetic and geometric mean of a lognormal distribution
---

Assuming someone approached you and offered to sell you his business. Now, how much should you be willing to pay?

Intuitively one might estimate that he can get 10% returns p.a. on the stock market, so 10x the amount of income generated yearly would seem like a reasonable price. In the following we will look deeper into this question: first for a riskless return and later with more volatile returns.

### Part 1: A riskless investment

Let us first assume that we can forecast the future yearly incomes with perfect accuracy, making it a riskless investment. Now how are riskless investments valued? Consider that 1$ invested now at the risk free (central bank) rate of 4% is worth 1.04$ in one year. Conversely 1$ in one year should be worth roughly 0.96$ now. The same holds true for longer time frames:

$$
  P = \sum_{n=1}^N \frac{C_n}{(1+i)^n}
$$

This is the [discounted cash flow](https://en.wikipedia.org/wiki/Discounted_cash_flow) formula, where the price \\( P \\) depends on the future yearly incomes \\( C_n \\) and the current interest rate \\( i \\). We simply discount all the cash received by a factor \\( (1+i)^{-n} \\), assuming that we could have instead gotten the same cash by compounding the interest at the current market rate of \\( i \\). Assuming a constant income \\( C_n = C \\), the [geometric series](https://en.wikipedia.org/wiki/Geometric_series) yields:

$$
  P = \frac{C}{i}\left(1 - \frac{1}{(1+i)^N}\right) \overset{N\rightarrow \infty}{\rightarrow}\ \frac{C}{i}
$$

So when interest rates rise, the value of the business declines and vice versa.

For a constant yearly income we should now be willing to pay the annual income divided by the current long-term interest rate of a riskless investment (Treasury bond).

> Assuming that the income is paid continuously instead of once every year, one would need to replace the sum above with an integral:
>
> $$ P = \int_0^t \frac{C}{(1+i)^n} \textrm{d}n = \frac{C}{\ln(1+i)}\left(1 - \frac{1}{(1+i)^t}\right) \overset{t\rightarrow \infty}{\rightarrow}\ \frac{C}{\ln(1+i)} $$
>

### Part 2: Volatility

Until now we assumed there was a guaranteed, constant return on our investment. Let us now instead assume that the expected future income follows a distribution with mean \\( \mu \\) and variance \\( \sigma^2 \\), and our business is listed on the stock market. Heuristically we can not lose more than our complete investment - which would conflict with a [normally distributed](https://en.wikipedia.org/wiki/Normal_distribution) return. Instead we will assume a [lognormally distributed](https://en.wikipedia.org/wiki/Log-normal_distribution) return, which empirically works somewhat well for actual stock returns.

The crucial part here is the relationship between the [arithmetic](https://en.wikipedia.org/wiki/Arithmetic_mean) and [geometric](https://en.wikipedia.org/wiki/Geometric_mean) mean. The geometric mean is always smaller than the arithmetic mean and the lower the volatility the smaller the difference between them becomes. The lognormal distribution is a normal distribution with the substitution  \\( x \rightarrow \ln(x) \\), so we know both the expectation value of the logarithm and the logarithm of the [expectation value](https://en.wikipedia.org/wiki/Log-normal_distribution#Arithmetic_moments):

$$
  E[\ln(x)] = \mu \qquad \ln(E[x]) = \mu + \frac{\sigma}{2} = \mu_a
$$

Here \\( \mu \\) is the logarithm of the geometric return, \\( \mu_a \\) the logarithm of the arithmetic return and \\( \sigma \\) the standard deviation of the logarithmic return.

> The fact that \\( \mu = E[\ln(x)] \\) equals the logarithm of the geometric mean is derived from the [product integral](https://en.wikipedia.org/wiki/Product_integral#Type_II:_geometric_integral):
> 
> $$ e^\mu = \prod f(x)^{\textrm{dx}} = \exp\left(\ln\left(\prod f(x)^{\textrm{dx}}\right)\right) = \exp\left(\int \textrm{dx} \ln(f(x)) \right) = e^{E[\ln(x)]} $$
>

Let us look at some real data, for example the S&P 500 (with dividends reinvested) from 1970 to 2018<a title="Data" href="#data"><sup>1</sup></a>:

| S&P 500     | μ       | σ       |
|-------------|---------|---------|
| returns     | 1.12011 | 0.16695 |
| log returns | 0.10068 | 0.16597 |

The S&P had a geometric mean of \\( e^\mu = 1.10592 \\) and a volatility of  \\( \sigma = 0.16597 \\). Calculating the geometric growth rate from the formula \\( \mu = \mu_a - \sigma^2 / 2 = \ln(1.12011) - 0.16597^2/2 \\)  yields a return rate of  \\( \mu = 0.09965 \\), which deviates by 1% from the correct value.

### Part 3: How much to invest?

Now, is an investment with small return and low volatility worse than one with high return and high volatility? If we have access to leverage we could simply lever up both the average arithmetic return and the standard deviation. Assuming one maintains a constant leverage and accouting for the borrowing rate \\( r \\) this gives us:

$$
  \mu = l \mu_a - (l\sigma)^2/2 + (1 - l)r
$$

Optimizing the geometric means leads to:

$$
  l = \frac{\mu_a - r}{\sigma^2}
$$

This leaves us with a maximum of:

$$
  \mu^* = \frac{(\mu_a-r)^2}{2\sigma^2} + r
$$

> This is the continous analog of the [Kelly formula](https://en.wikipedia.org/wiki/Kelly_criterion), where one has a series of favorable, but risky bets.
> Imagine for example you are offered many bets where 40% of the time your wager is tripled and 60% of the time lost. How would you play?

So when valuing an asset we should discount assets with higher volatility and chose to maximate the ratio of arithmetic returns to volatility. But should we put all our money into the asset with the highest ratio or diversify?

For a portfolio of multiple uncorrelated assets the standard deviation has the [same form](https://en.wikipedia.org/wiki/Propagation_of_uncertainty#Example_formulae) as a vector: Adding a small, orthogonal (uncorrelated) step leaves its overall length unchanged, but increases the expected return, making a diversified portfolio more attractive than an undiversified one.

For a number of (correlated) assets the Kelly formula has the following form:

$$
  \vec{l} = (1+r)\Sigma^{-1} (\vec{\mu} - r)
$$

Where \\( \vec{l} \\) is the amount of our wealth we should put into each asset, \\( r \\) the borrowing rate, \\( \vec{\mu} = E(\vec{x}) \\) the expected returns and \\( \Sigma = E(\vec{x}\vec{x}^\intercal)\\) the matrix of second moments.

___
<a id="data" href="https://datahub.io/core/s-and-p-500">Source</a>:
<table><thead><tr><th>S&amp;P 500</th><th>1971</th><th>1972</th><th>1973</th><th>1974</th><th>1975</th><th>1976</th><th>1977</th><th>1978</th><th>1979</th><th>1980</th><th>1981</th><th>1982</th><th>1983</th><th>1984</th><th>1985</th><th>1986</th><th>1987</th><th>1988</th><th>1989</th><th>1990</th><th>1991</th><th>1992</th><th>1993</th><th>1994</th><th>1995</th><th>1996</th><th>1997</th><th>1998</th><th>1999</th><th>2000</th><th>2001</th><th>2002</th><th>2003</th><th>2004</th><th>2005</th><th>2006</th><th>2007</th><th>2008</th><th>2009</th><th>2010</th><th>2011</th><th>2012</th><th>2013</th><th>2014</th><th>2015</th><th>2016</th><th>2017</th><th>μ</th><th>σ</th></tr></thead><tbody><tr><td>returns</td><td>1.13638</td><td>1.21875</td><td>0.83048</td><td>0.73735</td><td>1.38063</td><td>1.22471</td><td>0.93529</td><td>1.07670</td><td>1.18021</td><td>1.30147</td><td>0.97334</td><td>1.19073</td><td>1.23127</td><td>1.04603</td><td>1.31337</td><td>1.24101</td><td>0.99828</td><td>1.18744</td><td>1.30187</td><td>0.97557</td><td>1.22065</td><td>1.15488</td><td>1.09935</td><td>1.00406</td><td>1.38454</td><td>1.23561</td><td>1.31796</td><td>1.25506</td><td>1.21569</td><td>0.94235</td><td>0.87161</td><td>0.79777</td><td>1.22264</td><td>1.12801</td><td>1.07059</td><td>1.14250</td><td>1.06288</td><td>0.60684</td><td>1.30113</td><td>1.14014</td><td>1.02062</td><td>1.16769</td><td>1.29718</td><td>1.15845</td><td>1.02003</td><td>1.11716</td><td>1.20911</td><td>1.12011</td><td>0.16695</td></tr><tr><td>log_returns</td><td>0.12785</td><td>0.19783</td><td>-0.18575</td><td>-0.30469</td><td>0.32254</td><td>0.20270</td><td>-0.06690</td><td>0.07390</td><td>0.16569</td><td>0.26349</td><td>-0.02702</td><td>0.17456</td><td>0.20805</td><td>0.04500</td><td>0.27260</td><td>0.21593</td><td>-0.00172</td><td>0.17180</td><td>0.26380</td><td>-0.02473</td><td>0.19938</td><td>0.14399</td><td>0.09472</td><td>0.00405</td><td>0.32537</td><td>0.21157</td><td>0.27608</td><td>0.22718</td><td>0.19531</td><td>-0.05937</td><td>-0.13741</td><td>-0.22593</td><td>0.20102</td><td>0.12046</td><td>0.06821</td><td>0.13322</td><td>0.06098</td><td>-0.49949</td><td>0.26324</td><td>0.13115</td><td>0.02041</td><td>0.15503</td><td>0.26019</td><td>0.14708</td><td>0.01983</td><td>0.11079</td><td>0.18989</td><td>0.10068</td><td>0.16597</td></tr></tbody></table>
