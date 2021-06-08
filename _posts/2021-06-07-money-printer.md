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
  P = \frac{C}{i}\left(1 - \frac{1}{(1+i)^N}\right) + \frac{F}{(1+i)^N} \overset{N\rightarrow \infty}{\rightarrow} \frac{C}{i}
$$

For newly issued bonds with \\( C = F \cdot i \\) the bond value simply equals the face value \\( P = F \\). Older bonds lose value with rising interest rates and gain value with sinking interest rates. Now back to the money printer: We would need the yield of a infinite duration bond for comparison, but the Treasury only offers bonds with a maximum duration of 30 years.

Since there is no face value paid back in 30 years, we need to further discount the term \\( \frac{F}{(1+i)^{30}} \\) (which currently makes up 55% of the whole bond price) to account for the risk of changing interest rates, inflation and default after the first 30 years. For the current rate of 2% for 30 year bonds, the discounted value of an infinite bond is then somewhere between 2% (no further discount) to 4.4% (full discount).

Another difference between government bonds and the money printer is that the coupon is paid once every year, whereas the printer outputs money continously. The sum above needs to be replaced by an integral:

$$
  P = \int_0^N \frac{C}{(1+i)^n} \textrm{d}n + \frac{F}{(1+i)^N}
$$

Calculating the integral we can see that \\( \frac{1}{i} \\) needs to be replaced with \\( \frac{1}{\ln(1+i)} \approx \frac{1}{i} \\):

$$
  P = \frac{C}{\ln(1+i)}\left(1 - \frac{1}{(1+i)^N}\right) + \frac{F}{(1+i)^N} \overset{N\rightarrow \infty}{\rightarrow} \frac{C}{\ln(1+i)}
$$

After all we should now be willing to pay the annual output of the printer divided by the (logarithmic) interest rate of an infinite-duration government bond, which lies somewhere between 2% and 4.3%.

### Part 2: Volatility

Until now we assumed there was a guaranteed, constant return on our investment. Let us now instead assume that the return follows a distribution with mean \\( \mu \\) and variance \\( \sigma^2 \\). Heuristically we can not lose more than our complete investment - which would conflict with a [normally distributed](https://en.wikipedia.org/wiki/Normal_distribution) return. Instead we will assume a [lognormally distributed](https://en.wikipedia.org/wiki/Log-normal_distribution) return, which empirically works somewhat well for actual stock returns.

The crucial part here is the relationship between the [arithmetic](https://en.wikipedia.org/wiki/Arithmetic_mean) and [geometric](https://en.wikipedia.org/wiki/Geometric_mean) mean. The geometric mean is always smaller than the arithmetic mean and the lower the volatility the smaller the difference between them becomes. Luckily there exists a simple formula of this relationship for the lognormal distribution:

$$
  \mu_g = \mu_a - \sigma^2 / 2
$$

Here \\( \mu_g \\) is the logarithmic geometric return, \\( \mu_a \\) the logarithmic arithmetic return and \\( \sigma \\) the standard deviation of the logarithmic return.

> Using the [product integral](https://en.wikipedia.org/wiki/Product_integral#Type_II:_geometric_integral) we can see that \\( \mu_g \\) equals the expected logarithmic return \\( \mu_g = E(\log(x)) \\), whereas \\( \mu_a \\) equals the logarithm of the [expected return](https://en.wikipedia.org/wiki/Log-normal_distribution#Arithmetic_moments) \\( \mu_a = \log(E(x)) = \mu_g + \sigma^2 / 2 \\).

Let us look at some real data:

| 1980 - 2018 | μ       | σ       |
|-------------|---------|---------|
| returns     | 1.11908 | 0.16624 |
| log returns | 0.10023 | 0.16137 |

| 1871 - 2018 | μ       | σ       |
|-------------|---------|---------|
| returns     | 1.10586 | 0.18097 |
| log returns | 0.08638 | 0.17219 |

From 1980 to 2018 the S&P 500 (with dividends reinvested) had a geometric mean of 1.10543 and a volatility of 0.16137.

We can see that the geometric mean is in fact the exponential of the average log return since 1.10543 = exp(0.10023). Calculating the geometric mean from the average returns and volatility yields a return of 1.10460 = exp(ln(1.11908) - 0.16137^2/2) which deviates only 0.1% from the true value.

> The relationship between geometric mean and mean log is simply based on the fact that the sum of logarithms is the logarithm of the product: <br><br>
> $$ \sqrt[N]{\prod f(x)} = \exp\left(\ln\left(\prod f(x)^{\frac{1}{N}}\right)\right) = \exp\left(\sum \frac{\ln(f(x))}{N} \right) $$ <br>

Now, is an investment with small return and low volatility worse than one with high return and high volatility? If we have access to leverage we could simply lever up both the average arithmetic return and the standard deviation. Accouting for the cost of leverage at a borrowing rate \\( r \\) this gives us:

$$
  \mu_g = l \mu_a - \frac{(l\sigma)^2}{2} - (l - 1)r
$$

Optimizing the geometric means leads to:

$$
  l = \frac{\mu_a - r}{\sigma^2}
$$

This leaves us with a maximum of:

$$
  \mu_g^* = \frac{(\mu_a-r)^2}{2\sigma^2} + r
$$

> This is the continous analog of the [Kelly formula](https://en.wikipedia.org/wiki/Kelly_criterion), where one has a series of favorable, but risky bets. Imagine for example you are offered 1000 bets where 40% of the time your wager is tripled and 60% of the time lost. How would you play?

So the geometric growth depends on the ration between arithmetic growth and volatility. Note that for uncorrelated assets the standard deviation of a portfolio has the [same form](https://en.wikipedia.org/wiki/Propagation_of_uncertainty#Example_formulae) as a vector: Adding a small, orthogonal (uncorrelated) step leaves its overall length unchanged, but increases the expected return.

For a number of correlated assets the Kelly formula has the following form:

$$
  \vec{l} = (1+r)\Sigma^{-1} (\vec{mu} - r)
$$

Where \\( \vec{l} \\) is the amount of our wealth we should put into each asset, \\( r \\) the borrowing rate, \\( \vec{\mu} \\) the expected returns and \\( \Sigma = E(\vec{\mu}\vec{\mu}^\intercal)\\) the second moment matrix.
