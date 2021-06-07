---
title: "Buying a money printer"
date: 2021-06-07
description: Valuing assets with different volatilities
---

Assuming Jerome Powell approached you and offered to sell you one of the Feds money printing presses. He tells you they had a great experience with them, but now they are no longer needed, so the Fed is willing to sell them. Now, how much should you be willing to pay for them?

Intuitively one might estimate that he can get 10% returns p.a. on the stock market, so 10x the amount of dollars printed yearly would seem like a reasonable price. Now, in the following we will look deeper into this question: first for a riskless return and later with added volatility.

Independent of the price you might be willing to pay, the relevant question is: What would the market value of such a printer be? If you can get it cheaper, then you can simply sell it to the market, otherwise you can decline the offer and buy at the market. Whether you would hold such an investment yourself doesn't really matter as long as there is a market you trade it with.

Let us first assume that money printing is legal and Jerome Powell trustworthy, making it a low-risk invesment - but you are the first private person to own a money printer and have to estimate its value. The best thing to compare would be Treasury Bonds, since the debt can be repaid with printed dollars, so the risk of default is low. Now how are government bonds valued?

$$
  P = \frac{C}{1+i} + \frac{C}{(1+i)^2} + \frac{C}{(1+i)^3} + ... \frac{C}{(1+i)^N} + \frac{F}{(1+i)^N}
$$

One way is the above formula of the [discounted cash flow](https://en.wikipedia.org/wiki/Discounted_cash_flow) where the price \\( P \\) depends on the coupon \\( C \\), the current interest rate \\( i \\) and face value \\( F \\). We simply discount all the cash received by a factor \\( \frac{1}{(1+i)^n} \\), assuming that we could have alternatively gotten the same cash by compounding the interest at a rate of \\( i \\). Writing the coupon as \\( C = F i_0 \\) where \\( i_0 \\) is the actual interest paid by the bond we have now:

$$
  P = F \left(\frac{i_0}{1+i} + \frac{i_0}{(1+i)^2} + \frac{i_0}{(1+i)^3} + ... \frac{i_0}{(1+i)^N} + \frac{1}{(1+i)^N}\right)
$$

Rewriting the [geometric series](https://en.wikipedia.org/wiki/Geometric_series) gives us:

$$
  P = F \left(\frac{i_0}{i}+\frac{1-i_0/i}{(1+i)^N}\right)
$$

