---
title: "Valuing assets with different volatilities"
date: 2021-06-07
description: Discounted cash flows and the relationship between arithmetic and geometric mean of a lognormal distribution
---

Assuming Jerome Powell approached you and offered to sell you one of the Feds money printing presses. He tells you they had a great experience with them, but now they are no longer needed, so the Fed is willing to sell them. Now, how much should you be willing to pay for them?

Intuitively one might estimate that he can get 10% returns p.a. on the stock market, so 10x the amount of dollars printed yearly would seem like a reasonable price. In the following we will look deeper into this question: first for a riskless return and later with more volatile returns.

### Part 1: A riskless investment

Independent of the price you might be willing to pay, the relevant question is: What would the market value of such a printer be? If you can get it cheaper, then you can simply sell it to the market, otherwise you can decline the offer and buy at the market instead. Whether you would hold such an investment yourself doesn't really matter as long as there is a market you trade it with.

Let us first assume that money printing is legal and Jerome Powell trustworthy, making it a low-risk invesment - but you are the first private person to own a money printer and have to estimate its value. The best thing to compare would be Treasury Bonds, since the debt can be repaid with printed dollars, so the risk of default is low. Now how are government bonds valued?

$$
  P = \sum_{n=1}^N \frac{C}{(1+i)^n} + \frac{F}{(1+i)^N}
$$

One way is the above formula of the [discounted cash flow](https://en.wikipedia.org/wiki/Discounted_cash_flow), where the price \\( P \\) depends on the coupon \\( C \\), the current interest rate \\( i \\) and face value \\( F \\). We simply discount all the cash received by a factor \\( (1+i)^{-n} \\), assuming that we could have instead gotten the same cash by compounding the interest at the current market rate of \\( i \\). Rewriting the [geometric series](https://en.wikipedia.org/wiki/Geometric_series) gives us:

$$
  P = \frac{C}{i}\left(1 - \frac{1}{(1+i)^N}\right) + \frac{F}{(1+i)^N}\ \overset{N\rightarrow \infty}{\rightarrow}\ \frac{C}{i}
$$

For newly issued bonds with \\( C = F \cdot i \\) the bond value simply equals the face value \\( P = F \\). Older bonds lose value with rising interest rates and gain value with sinking interest rates. Now back to the money printer: We would need the yield of a infinite duration bond for comparison, but the Treasury only offers bonds with a maximum duration of 30 years.

Since there is no face value paid back in 30 years, we need to further discount the term \\( \frac{F}{(1+i)^{30}} \\) (which currently makes up 55% of the whole bond price) to account for the risk of changing interest rates, inflation and default after the first 30 years. With the current rate of 2% for 30 year bonds, an infinite bond would then pay a rate somewhere between 2% (no discount) to 4.4% (full discount).

After all we should now be willing to pay the annual output of the printer divided by the interest rate of an infinite-duration government bond.

> Assuming that the coupon is paid continously instead of once every year, we would need to replace the sum above with an integral:
>
> $$ P = \int_0^N \frac{C}{(1+i)^n} \textrm{d}n + \frac{F}{(1+i)^N} = \frac{C}{\ln(1+i)}\left(1 - \frac{1}{(1+i)^N}\right) + \frac{F}{(1+i)^N}\ \overset{N\rightarrow \infty}{\rightarrow}\ \frac{C}{\ln(1+i)} $$
>

### Part 2: Volatility

Until now we assumed there was a guaranteed, constant return on our investment. Let us now instead assume that the return follows a distribution with mean \\( \mu \\) and variance \\( \sigma^2 \\). Heuristically we can not lose more than our complete investment - which would conflict with a [normally distributed](https://en.wikipedia.org/wiki/Normal_distribution) return. Instead we will assume a [lognormally distributed](https://en.wikipedia.org/wiki/Log-normal_distribution) return, which empirically works somewhat well for actual stock returns.

The crucial part here is the relationship between the [arithmetic](https://en.wikipedia.org/wiki/Arithmetic_mean) and [geometric](https://en.wikipedia.org/wiki/Geometric_mean) mean. The geometric mean is always smaller than the arithmetic mean and the lower the volatility the smaller the difference between them becomes. Luckily there exists a simple formula describing the relationship for the lognormal distribution:

$$
  \mu_g = \mu_a - \sigma^2 / 2
$$

Here \\( \mu_g \\) is the logarithmic geometric return, \\( \mu_a \\) the logarithmic arithmetic return and \\( \sigma \\) the standard deviation of the logarithmic return.

> Using the [product integral](https://en.wikipedia.org/wiki/Product_integral#Type_II:_geometric_integral) we can see that \\( \mu_g \\) equals the expected logarithmic return \\( \mu_g = E(\log(x)) \\), whereas \\( \mu_a \\) equals the logarithm of the [expected return](https://en.wikipedia.org/wiki/Log-normal_distribution#Arithmetic_moments) \\( \mu_a = \log(E(x)) = \mu_g + \sigma^2 / 2 \\).

Let us look at some real data, for example the S&P 500 (with dividends reinvested) from 1970 to 2018:

| S&P 500     | μ       | σ       |
|-------------|---------|---------|
| returns     | 1.12011 | 0.16695 |
| log returns | 0.10068 | 0.16597 |

<details><summary>Data</summary>
| S&P 500     | 1971    | 1972    | 1973     | 1974     | 1975    | 1976    | 1977     | 1978    | 1979    | 1980    | 1981     | 1982    | 1983    | 1984    | 1985    | 1986    | 1987     | 1988    | 1989    | 1990     | 1991    | 1992    | 1993    | 1994    | 1995    | 1996    | 1997    | 1998    | 1999    | 2000     | 2001     | 2002     | 2003    | 2004    | 2005    | 2006    | 2007    | 2008     | 2009    | 2010    | 2011    | 2012    | 2013    | 2014    | 2015    | 2016    | 2017    |
|-------------|---------|---------|----------|----------|---------|---------|----------|---------|---------|---------|----------|---------|---------|---------|---------|---------|----------|---------|---------|----------|---------|---------|---------|---------|---------|---------|---------|---------|---------|----------|----------|----------|---------|---------|---------|---------|---------|----------|---------|---------|---------|---------|---------|---------|---------|---------|---------|
| returns     | 1.13638 | 1.21875 | 0.83048  | 0.73735  | 1.38063 | 1.22471 | 0.93529  | 1.07670 | 1.18021 | 1.30147 | 0.97334  | 1.19073 | 1.23127 | 1.04603 | 1.31337 | 1.24101 | 0.99828  | 1.18744 | 1.30187 | 0.97557  | 1.22065 | 1.15488 | 1.09935 | 1.00406 | 1.38454 | 1.23561 | 1.31796 | 1.25506 | 1.21569 | 0.94235  | 0.87161  | 0.79777  | 1.22264 | 1.12801 | 1.07059 | 1.14250 | 1.06288 | 0.60684  | 1.30113 | 1.14014 | 1.02062 | 1.16769 | 1.29718 | 1.15845 | 1.02003 | 1.11716 | 1.20911 |
| log returns | 0.12785 | 0.19783 | -0.18575 | -0.30469 | 0.32254 | 0.20270 | -0.06690 | 0.07390 | 0.16569 | 0.26349 | -0.02702 | 0.17456 | 0.20805 | 0.04500 | 0.27260 | 0.21593 | -0.00172 | 0.17180 | 0.26380 | -0.02473 | 0.19938 | 0.14399 | 0.09472 | 0.00405 | 0.32537 | 0.21157 | 0.27608 | 0.22718 | 0.19531 | -0.05937 | -0.13741 | -0.22593 | 0.20102 | 0.12046 | 0.06821 | 0.13322 | 0.06098 | -0.49949 | 0.26324 | 0.13115 | 0.02041 | 0.15503 | 0.26019 | 0.14708 | 0.01983 | 0.11079 | 0.18989 |
</details>

The S&P had a geometric mean of 1.10592 (which is in fact the exponential of the average log return) and a volatility of 0.16597. Calculating the geometric mean from the average returns and volatility yields a geometric return of 1.10479 = exp(ln(1.12011) - 0.16597^2/2) - which deviates only 0.1% from the true value.

> The relationship between geometric mean and mean log is simply based on the fact that the sum of logarithms is the logarithm of the product:
> 
> $$ \sqrt[N]{\prod x_i} = \exp\left(\ln\left(\prod x_i^{\frac{1}{N}}\right)\right) = \exp\left(\sum \frac{\ln(x_i)}{N} \right) = \exp(E(\ln(x))) $$
>

Now, is an investment with small return and low volatility worse than one with high return and high volatility? If we have access to leverage we could simply lever up both the average arithmetic return and the standard deviation. Accouting for the cost of leverage at a borrowing rate \\( r \\) this gives us:

$$
  \mu_g = l \mu_a - (l\sigma)^2/2 - (l - 1)r
$$

Optimizing the geometric means leads to:

$$
  l = \frac{\mu_a - r}{\sigma^2}
$$

This leaves us with a maximum of:

$$
  \mu_g^* = \frac{(\mu_a-r)^2}{2\sigma^2} + r
$$

> This is the continous analog of the [Kelly formula](https://en.wikipedia.org/wiki/Kelly_criterion), where one has a series of favorable, but risky bets.
> Imagine for example you are offered 1000 bets where 40% of the time your wager is tripled and 60% of the time lost. How would you play?

So when valuing an asset we should discount assets with higher volatility and chose to maximate the ratio of arithmetic returns to volatility.

Now, for a portfolio of multiple uncorrelated assets the standard deviation has the [same form](https://en.wikipedia.org/wiki/Propagation_of_uncertainty#Example_formulae) as a vector: Adding a small, orthogonal (uncorrelated) step leaves its overall length unchanged, but increases the expected return, making a diversified portfolio more attractive than an undiversified one.

For a number of (correlated) assets the Kelly formula has the following form:

$$
  \vec{l} = (1+r)\Sigma^{-1} (\vec{\mu} - r)
$$

Where \\( \vec{l} \\) is the amount of our wealth we should put into each asset, \\( r \\) the borrowing rate, \\( \vec{\mu} \\) the expected returns and \\( \Sigma = E(\vec{x}\vec{x}^\intercal)\\) the matrix of second moments.
