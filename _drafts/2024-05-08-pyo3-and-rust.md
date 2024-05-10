---
layout: post
title: "Bond Pricing in Python with PyO3"
tags: ["rust", "finance", "python"]
---

I recently posted some code that used Rust to calculate the price of a bond (given the yield to maturity) and the yield to maturity of a bond (when given the price). One reason for writing this code in Rust - as opposed to, say, Python - was to create something that's closer to the type of code one would use in a real pricing system[^1]. But in many cases, it might be useul to write the calculation code in Rust and then call it from a higher-level language, like Python. It's this scenario that I want to cover here.




[^1]: While there's no question that the speed benefits of a language like Rust are of little use in a simple case like this, it's an entry point into the general concept of using high-performance languages under the hood. And that's 