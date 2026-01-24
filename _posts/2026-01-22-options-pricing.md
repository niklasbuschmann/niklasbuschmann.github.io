---
title: "The Black-Scholes equation"
date: 2026-01-22
description: Options Pricing
mathjax: true
layout: post
---

When writing an option, the market maker will hedge his exposure by buying stock of the amount equal to the first derivative of the option with respect to the underlying \\( \frac{\partial V}{\partial S} \\). When \\( S \\) changes by some amount, the change in price of the option will be compensated up to first order by the change of \\( S \\).

However, the option price is convex in regard to the stock price, thus the amount of stock held \\( \frac{\partial V}{\partial S} \\) decreases when \\( S \\) decreases and increase when \\( S \\) increases. In other words: the market maker always buys high and sells low, making volatility costly for him. To compensate for this, an option will lose value over time, compensating for the volatility experienced during that time, and leaving the market maker with the risk-free return when averaged over many different underlyings.

The famous [Black-Scholes formula](https://en.wikipedia.org/wiki/Black–Scholes_equation) quantizes this relationship between the time evolution of the option price and its second derivative with respect to \\( S \\), allowing one to calculate the time premium for a given expected volatility.

### Random Walk

Assume one takes steps of equal size \\( s \\) after a fixed interval \\( \tau \\), with the direction of each step decided randomly between noth and south. After the time \\( t \\) there will have been made \\( \frac{t}{\tau} \\) steps, and the probability of then beeing at point \\( x(t) \\) will after many steps, by the [central limit theorem](https://en.wikipedia.org/wiki/Central_limit_theorem), follow a normal distribution with expectation value \\( \mathbb{E}[x] = \frac{t}{\tau} (p_\uparrow-p_\downarrow) s =: \mu t \\) and variance \\( \mathbb{V}[x] = \frac{t}{\tau} s^2 =: \sigma^2 t \\).

Taking the limit \\( \tau \rightarrow 0 \\) while keeping \\( \mu \\) and \\( \sigma^2 \\) constant results in a so-called Wiener process. A change \\( \Delta x \\) after time \\( \Delta t\\) is then again drawn from a normal distribution \\( \Delta x \sim \mathcal{N}(\Delta t \mu, \Delta t \sigma^2) \sim \mu\Delta t + \sigma\sqrt{\Delta t}\,\mathcal{N}(0, 1) \\), making the derivative \\( \frac{\Delta x}{\Delta t} \sim \mu + \frac{\sigma}{\sqrt{\Delta t}}\mathcal{N}(0, 1) \\) divergent in the limit \\( \Delta t \rightarrow 0 \\) and leaving the function \\( x(t) \\) continous everywhere but differentiable nowhere.

### Itô's lemma

If one now assumes, that the price of a stock is governed by a Wiener process, and the option price \\( V(S, t)\\) depends only on the stock price \\( S \\) and time \\( t \\), with the other parameters \\( \mu, \sigma, r \\) assumed to be constant, then one can look at the change \\( \Delta V(S, t) \\) after the time \\( \Delta t \\) by using the Taylor expansion of \\( V \\):

$$
\begin{aligned}
  \Delta V(S,t) &= \frac{\partial V}{\partial t}\Delta t + \frac{\partial V}{\partial S}\Delta S + \frac{1}{2}\frac{\partial^2 V}{\partial t^2}(\Delta t)^2 + \frac{1}{2}\frac{\partial^2 V}{\partial S^2}(\Delta S)^2 + \frac{\partial^2 V}{\partial t\partial S}\Delta t \Delta S + \dots \\[1.5ex]
  &\overset{\Delta t \rightarrow 0}{\longrightarrow} \frac{\partial V}{\partial t}\Delta t + \frac{\partial V}{\partial S}\Delta S + \frac{1}{2}\frac{\partial^2 V}{\partial S^2}(\Delta S)^2
\end{aligned}
$$

Compared to the usual chain rule, there is now an additional term \\( \frac{1}{2}\frac{\partial^2 V}{\partial S^2}(\Delta S)^2 \\) due to the fact that \\( \Delta S \\) is of the order \\( \sqrt{\Delta t} \\), requiring one to include the second order term \\( (\Delta S)^2 \\) when looking at changes of the order \\( \Delta t \\).

### Delta hedging

Since there is still a \\( \Delta S \\) term in the expression for \\( \Delta V \\), the time derivative of the option price is also divergent. But this is not the case when looking at the delta-hedged portfolio \\( V - \frac{\partial V}{\partial S}S \\):

$$
  \Delta \left(V - \frac{\partial V}{\partial S}S \right) = \frac{\partial V}{\partial t}\Delta t + \cancel{\frac{\partial V}{\partial S}\Delta S} + \frac{1}{2}\frac{\partial^2 V}{\partial S^2}(\Delta S)^2 - \cancel{\frac{\partial V}{\partial S}\Delta S} \overset{!}{=} r \left(V - \frac{\partial V}{\partial S}S \right)\Delta t
$$

Since the first-order price-swings of the portfolio have been eliminated and only the (small) second-order changes remain, one can require the portfolio to grow at the risk-free interest rate \\( r \\). Further, one can maintain a portfolio of many different delta-hedged options where the remaining second-order changes (hopefully) cancel each other, making the combined portfolio resemble a risk-free portfolio even more.

### Bachelier model

The only remaining part is the expression for \\( (\Delta S)^2 \\). Assuming the stock price itself is a Wiener process, then \\( \mathbb{E}[(\Delta S)^2] = \sigma^2\Delta t \\) and \\( \mathbb{V}[(\Delta S)^2] = 2\sigma^4(\Delta t)^2 \\). Here the random part behaves much nicer and causes no divergence in the limit \\( \Delta t \rightarrow 0 \\). Because of this, we can assume by the central limit theorem that for the sum / integral the random parts will cancel each other and we can replace \\( (\Delta S)^2 \\) by its expectation value \\( \mathbb{E}[(\Delta S)^2] = \sigma^2\Delta t\\).

This model was first described in 1900 by [Louis Bachelier](https://en.wikipedia.org/wiki/Louis_Bachelier), predating Black and Scholes by seven decades. The resulting differential equation is then given by:

$$
  \frac{\partial V}{\partial t} + \frac{1}{2}\frac{\partial^2 V}{\partial S^2}\sigma^2 \overset{!}{=} r V - r\frac{\partial V}{\partial S}S
$$

> Which is solved by:
> 
>  $$
>    V(F,\tau) = (F-K)\phi\left(\frac{F-K}{\sigma\sqrt{\tau}}\right)+\sigma\sqrt{\tau}\phi'\left(\frac{F-K}{\sigma\sqrt{\tau}}\right)
>  $$
>
>  Here \\( \tau = T - t\\) is the remaining time, \\( F = e^{r-d}S \\) the forward price of the stock, \\( K \\) the strike price of the option and \\( \phi(x) = \pm\int_{-\infty}^{\pm x} \frac{e^{-t^2/2}}{\sqrt{2\pi}}\textrm{d}t \\) with positive sign for the call and negative for the put option.

In this model, the stock price itself follows a normal-distribution, whereas in a more appropriate model one would expect the stock prices to follow a lognormal-distribution, avoiding the possibility of negative prices.

### Black-Scholes model

To get to the lognormal distribution, one needs to instead assume that the logarithm of the stock price follows a Wiener process: \\( \Delta \ln S \sim \mathcal{N}(t \mu, t \sigma^2) \\). Using \\( \Delta \ln S = \frac{\Delta \ln S}{\Delta S}\Delta S = - \frac{\Delta S}{S} \implies (\Delta S)^2 = (S\Delta \ln S)^2 \\) and again using the expectation value \\( \mathbb{E}[(\Delta S)^2] = S^2\sigma^2 \Delta t \\) yields the Black-Scholes equation:

$$
  \frac{\partial V}{\partial t} + \frac{1}{2}\frac{\partial^2 V}{\partial S^2}S^2\sigma^2 \overset{!}{=} r V - r\frac{\partial V}{\partial S}S
$$

>  This differential equation is solved by:
>
>  $$
>    e^{rt}V(F,\tau) = F\phi\left(\frac{\ln(\frac{F}{K})}{\sigma\sqrt{\tau}}+\frac{1}{2}\sigma\sqrt{\tau}\right)-K\phi\left(\frac{\ln(\frac{F}{K})}{\sigma\sqrt{\tau}}-\frac{1}{2}\sigma\sqrt{\tau}\right)
>  $$
>
>  Here \\( \tau = T - t\\) is the remaining time, \\( F = e^{r-d}S \\) the forward price of the stock, \\( K \\) the strike price of the option and \\( \phi(x) = \pm\int_{-\infty}^{\pm x} \frac{e^{-t^2/2}}{\sqrt{2\pi}}\textrm{d}t \\) with positive sign for the call and negative for the put option.