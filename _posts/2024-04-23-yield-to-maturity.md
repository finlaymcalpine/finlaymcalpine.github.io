---
layout: post
title: "Yield to Maturity, with calculation in Rust"
tags: ["finance", "fixed-income", "rust"]
---

Following on from the note about bond pricing (which is [here](../_posts/2024-04-22-bond-basics.md)) I wanted to consider the other side of the below equation[^1] - which we took as given in calculating the price.

$$P =  \frac{F}{(1 + ( \lambda / m))^ {n}} + \frac{C}{\lambda}\left\{ 1 - \frac{1}{(1 + ( \lambda / m))^ {n}} \right\} $$

The Yield to Maturity ($$\lambda$$) is the interest rate that equates the price of the bond to the present value of the cashflow obtained by holding the bond until maturity. It's the answer to the question: if we pay price _P_ for this bond, what is the implied interest rate based on the predetermined cashflow?

As I tried to motivate in the last post, the market mechanism that keeps the equation in balance is the fact that an investor will expect the yield from this bond to be an equivalent interest rate to any other _equivalent_ investment (a bond of equivalent maturity and default risk).

We need to use a some computational method to solve for the yield to maturity here, since there is no closed form solution. We set the present value equal to the price and find $$\lambda$$ such that the equation is zero, i.e. find the roots of:

$$y = f(\lambda) = \frac{F}{(1 + ( \lambda / m))^ {n}} + \frac{C}{\lambda}\left\{ 1 - \frac{1}{(1 + ( \lambda / m))^ {n}} \right\} - P$$

One mechanism that can be used is [Newton's Method](https://en.wikipedia.org/wiki/Newton's_method), which involves differentiating the above function and running an iterative procedure, where $$x_{n+1} = x_{n} - \frac{f(\lambda)}{f'(\lambda)}$$. We continue this updating step until the changes between $$x_{n}$$ and $$x_{n+1}$$ are sufficiently small[^2].

```rust
fn solve_yield_to_maturity(&mut self, mut x0: f32, iter: i32, tolerance: f32, epsilon: f32) -> f32 {
        let max_iter: i32 = iter;
        let coupon_periods = self.frequency * self.maturity;

        for _ in 1..max_iter {

            let y = (self.face_value - ((self.coupon * self.face_value) / x0))
                * ((1.0 + (x0 / self.frequency)).powf(-1.0 * coupon_periods))
                + ((self.coupon * self.face_value) / x0)
                - self.price;
            
            let y_prime = ((self.coupon * self.face_value) / x0.powf(2.0))
                * (1.0 + (x0 / self.frequency)).powf(-1.0 * coupon_periods)
                - ((self.face_value - ((self.coupon * self.face_value) / x0))
                    * (coupon_periods
                        * (1.0 + (x0 / self.frequency)).powf(-1.0 * coupon_periods - 1.0))
                    / self.frequency)
                - ((self.coupon * self.face_value) * x0.powf(-2.0));

            if y_prime.abs() < epsilon {
                break;
            };

            let x1: f32 = x0 - (y / y_prime); // Newton's Method Step

            if (x1 - x0).abs() <= tolerance {
                self.yield_to_maturity = x1;
                break;
            }
            x0 = x1;
        }

        self.yield_to_maturity
    }
```

We can now check that the yield given by our calculation is the same as the yield we supplied when we used the price function last time:

```rust 
let mut bond2 = SimpleBond {
        face_value: 1000.0,
        coupon: 0.04,
        frequency: 2.0,
        maturity: 10.0,
        yield_to_maturity: 0.0, // we have to give some float to fill out the struct.
        price: 953.5723,
    };
```

This gives us a yield to maturity of `0.04583999`, which aligns with the number we gave the `solve_price()` function to get the price `$953.5723` in the pricing post. The fact that we can move between the price and yield using these functions is a sign that they're doing what we want.

So we can now, whether given the price or the yield to maturity, calculate the other to fully specify the key elements of the pricing equation for a bond.

The code used here can be found in [this repo](https://github.com/finlaymcalpine/bond_pricing/tree/main), although this is a quick implementation of the main idea. It should be taken as an outline. I plan to clean it up in order to attach it to a PyO3 library.

_As always, I appreciate any corrections or feedback to feedback@finlaymcalpine.com_

[^1]: As before, I'll refer to Luenberger's _Investment Science_ textbook

[^2]: The choice of 'sufficiently small' depends on the context: in this case the function accepts an argument to define this 'tolerance'. There are also concerns about convergence with numerical algorithms like this. I won't cover that here, but those are discussed in the Wikipedia page linked and in other sources