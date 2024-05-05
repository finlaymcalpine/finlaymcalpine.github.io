---
layout: post
title: "Pricing a fixed income bond using Rust"
tags: ["finance", "fixed-income", "rust"]
---

This post is a quick primer on pricing a bond in fixed income markets, and some accompanying Rust code to calculate that price. I plan to extend to some other concepts (yield, duration) in future.

I'm going to reference the book _Investment Science_ by Luenberger, which is a really nice reference for the basics of financial markets from a quantitative viewpoint. A more detailed reference focused on empirical applications is _The Econometrics of Financial Markets_ by Lo, et. al. Finally, an overview of bonds by [PIMCO](https://www.pimco.com/en-us/marketintelligence/bond-basics/what-impacts-the-price-and-performance-of-bonds/) is a good quick overview of the ideas.

A standard bond for our purposes is a bond that has a principal to be repaid at maturity and makes coupon payments of a predetermined amount, on a predetermined schedule (let's choose every 6 months, as is the case for US Treasury Bonds). The bond therefore has a fully deterministic cashflow through its lifetime: we are not looking at inflation-linked bonds (TIPS or Linkers) or callable bonds that can be redeemed prior to their maturity date.

While I won't get far into the detail of the nature of bonds, it's clear that we're thinking of bonds here as a predefined cash flow. So how do we value that? In much the same way that we value any cashflow: by discounting according to the time value of money. In the same way that a discounted cashflow model for an equity can be used to assess the 'value' of a stock, we can do the same thing on the more deterministic cashflow associated with a bond.

The general formula for the price of a bond is:

$$P =  \frac{F}{(1 + ( \lambda / m))^ {n}} + \sum_1^n  \frac{C/m}{(1 + ( \lambda / m))^ {n}} $$

which, given that the second part of the sum is the present value of an annuity[^1], is equivalent to:

$$P =  \frac{F}{(1 + ( \lambda / m))^ {n}} + \frac{C}{\lambda}\left\{ 1 - \frac{1}{(1 + ( \lambda / m))^ {n}} \right\} $$

In the above, the bond has exactly _n_ coupon periods remaining to maturity, _F_ is the face value of the bond, _C_ is the annual coupon payment, _m_ is the number of coupon payments per year, and $\lambda$ is the yield to maturity.

The yield to maturity (YtM) here is the return we'd get if we bought and held the bond until it matures. This is the discount rate we apply to the cashflow. An interesting aspect of pricing a bond is that market forces are trying to balance the price and YtM to maintain the equality of this equation, _and_ maintain the YtM in line with other interest rates. 

Suppose that the price of a bond fell and the associated YtM rose above the interest rate available by purchasing other assets with an equivalent payoff profile[^2]. Since investors recognise that they can obtain a higher return by purchasing this particular bond, they will bid up the price (and consequently reduce the YtM) to align the returns. This feedback leads to the price of a bond being more than just a simple formula: it is the product of a complex marketplace in which the principle of not paying a different price for the same cashflow generates prices and yields that move together to keep interest rates in balance.

In Rust, we can price the bond - if given a yield - as follows:

```rust
struct SimpleBond {
    face_value: f32,
    coupon: f32,
    frequency: f32,
    maturity: f32,
    yield_to_maturity: f32,
}

impl SimpleBond {
    
    // We'll calculate the principal and coupon flow present values separately, and then combine them
    fn price(&self) -> f32{
        let pv_principal = self.face_value/((1.0 + (self.yield_to_maturity/self.frequency)).powf(self.maturity*self.frequency));

        let pv_coupon = ((self.coupon*self.face_value)/self.yield_to_maturity) * (1.0 - (1.0/((1.0 + (self.yield_to_maturity/self.frequency)).powf(self.maturity*self.frequency))));

        let price = pv_principal + pv_coupon;

        return price
    }
}
```
Here, we've created a SimpleBond struct to hold the data we need for the bond (we'll add more data later, along with methods to calculate them), and then an implementation for a price function that takes the data and generates the price according to the formula above.

We can test the approximate performance of this calculator by using the example of the current 10 year UST bond on 5/2/2024. The bond has a maturity of 2/15/2034, so an accurate price would have to account for accrued interest and the less than full remaining coupon period.

```rust
fn main() {

    // details for TMUBMUSD10Y bond on 5/2/2024
    let bond1 = SimpleBond {
        face_value: 1000.0,
        coupon: 0.04,
        frequency: 2.0,
        maturity: 10.0,
        yield_to_maturity: 0.04584,
    };

    println!("10 year UST price on 5/2/2024: ${}", bond1.price());
}
```

This gives the output `10 year UST price on 5/2/2024: $953.5723`. Compare this to the market published price of `95 4/32`, which equals to `(4/32 * 100) = 12.5bp => 1000 * 0.95125 = $951.25`. So our simple implementation gives an approximately correct result.

[^1]: See Luenberger pg. 46

[^2]: I'm going to gloss over the concept of no-arbitrage, but see the slides [here](https://pages.stern.nyu.edu/~jcarpen0/courses/b403333/01zero.pdf) for more detail