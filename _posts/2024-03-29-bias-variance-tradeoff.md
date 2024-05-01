---
layout: post
title: "The Bias-Variance Tradeoff"
---

I originally wrote this as a standalone, but have updated it to tie in with notes on the [Linear Regression Assumptions](_posts/2024-04-15-the-linear-regression-model.md). There's a nice connection between certain assumptions (and the consequences of their violation) and the tradeoff between bias and variance in statistics and machine learning, and I wanted to make that connection more explicit.

The bias-variance tradeoff, in short, shows that - when fitting a statistical model - we can decompose the mean squared error into a bias component and a variance component (the derivation of this can be found on the [wikipedia page](https://en.wikipedia.org/wiki/Mean_squared_error#Estimator)). So our measurement of predictive error can be attributed to those two components. The challenge comes from the fact that bias is a decreasing function of model complexity, and variance is an increasing function of model complexity. When we think of a standard linear model, we can throw a lot of variables to help explain as much of the variance in our response variable as we can. But that might not be a good idea (and in fact - beyond a certain point - it's almost certainly going to be a bad idea!).