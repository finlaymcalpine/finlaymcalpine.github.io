---
layout: post
title: "Assumptions of the Linear Model"
categories: [Statistics, Econometrics]
---

The linear regression model is widely used, but it's not always intuitive or well understood. This post aims to briefly cover the linear model as laid out for Ordinary Least Squares, the assumptions involved, and what those assumptions allow us to do with the results of our regression.

Some sources that I will recommend on this topic (there are many) are:
1. Either _Introduction to Statistical Learning_, or _Elements of Statistical Learning_. They are both good references on linear regression from the perspective of predictive machine learning, although ISL does not get into the weeds of the linear model assumptions.
2. Hayashi's _Econometrics_ is a good resource that approaches the topic from an econometrician's perspective. I would describe this book as one of the most explicit I've seen about the assumptions, but the level of depth can require several passes. For those not planning to pursue econometric theory, the depth is likely too much. But the presentation of the way that assumptions fold into the model is nice.
3. _[Linear Model and Extensions](https://arxiv.org/pdf/2401.00649.pdf)_ by Ding (which I have only recently skimmed early sections of) has a nice treatment of the linear model, and builds the assumptions into the linear piece by piece in Chapters 3-5. 

### The Model

If we have a response (or dependent) variable _Y_ and one or more covariates (or features) _X_, we can model the relationship between these in the following way:

_Y = a \+ bX\+ e_

where _Y_, _X_, and _e_ are for each sample in our data.

A crucial assumption for the linear regression model is that the linear model correctly describes the data - that the true relationship between Y and X is linear. In many cases, we'll assume this based solely on looking at the plotted data. While not the most precise method, this will help quickly reject linear regression in cases where the relationship is clearly not linear. While it this is probably the most important assumption, it is often easiest to see violations.

### The Primary Assumptions

Once we've plotted the data and verified that the linear model is appropraite, we then have to think about other assumptions we're making when we use linear regression.

There seems to be some confusion about the assumptions of linear regression. I have seen a number of articles that make incorrect claims.

The most wrong claim about the assumptions of linear regression is that the data need to be normally distributed. That is not correct. We don't need any specific distribution of the data: the features X could be normally distributed (as might be the case with height), it could be log-normal (as might be the case for income), or many more distributions. We might even find that the feature takes on a bimodal distribution clustered strongly around two values. None of these scenarios prevent us from using the linear model. However, if we have strongly clustered data or data that is sparse in some regions of the feature space, we might want to think carefully about the data generating process underlying our data.

So what assumptions do we make when we use the linear model?

### Multicollinearity

First, we assume that there is no multicollinearity in our data: that we don't have features that are a linear combination of other features. A possible cause of a failure here would be that a column of our data is accidentally duplicated.

This assumption is not actually prohibitive from a statistical point of view, but it does make the calculation of the model more difficult for the software. In the case that some features are highly correlated with one another, we won't have biased estimates of the coefficients, but we will widen the standard errors of our coefficient estimates. Intuitively, if two features are highly correlated, it is more difficult to precisely tease out the individual effect on the response variable.

So while multicollinearity isn't going to bias our coefficient estimates, it will make them less efficient (in the statistical sense). The topic of 

A more technical treatment of the collinearity problem can be found [here](https://www.stat.cmu.edu/~larry/=stat401/lecture-17.pdf).

### Error Exogeneity

Second, we assume orthogonality of the errors, that __E(e) = 0__. That's to say that knowing the value of the _X_ matrix tells us nothing about the error for a specific observation. A scenario in which this assumption would fail would be the existence of an omitted variable that is correlated with our features (Omitted Variable Bias), __and__ has  correlation (i.e. some explanatory power) with the response variable. In that case, knowing something about _X_ can give us some information about the error term, because there's an omitted variable that exists 'inside' that error and it's correlated with _X_.

If we omit such a variable, we are going to be capturing its effect on _Y_ in the coefficient we estimate for _X_. That leads our coefficient estimate, _b_ to be biased and we no longer have an unbiased estimator.

There is a nice StackExchange answer [here](https://stats.stackexchange.com/a/210088) that provides an interesting example of how an omitted variable can bias our coefficient estimates.

A useful discussion of the difference between OVB and Multicollinearity is to be found [here](https://are.berkeley.edu/courses/EEP118/current/handouts/OVB%20versus%20Multicollinearity_eep118_sp15.pdf). 

### Homoskedasticity (or Spherical Error Variance)

This assumption more or less boils down to all the errors having a covariance structure that shares a variance for all errors and has no covariance between errors.

There are two components to this assumption, the first of which is that errors conditional on _X_ are similarly distributed across the values of a feature. When we plot our residuals, we _don't_ want to see that the residuals have a higher variance at some points in the range of _X_. In that case, we'd be concerned that the error in our model is in fact heteroskedastic.

The second component of the assumption is that there is no serial correlation between errors. That breakdown most often occurs in time series applications, where we are concerned about correlation through time, or in spatial correlation across groups in cross-sectional data.

The failure of this assumption, on the structure of the error variance, is that our coefficient estimates will no longer be efficient, and any hypothesis testing we do on those estimates will be erroneous. The standard errors given by the standard OLS procedure are based on homoskedasticity. In order to correct for the presence of heteroskedasticity, we use robust standard errors (under the framework of Generalised Least Squares or Weighted Least Squares).

### Normality of the Errors

This assumption is not necessary for unbiased estimation of the coefficients or the efficiency of our estimates. However, the addition of this assumption has a couple of consequences: one is that the OLS coefficient estimator is then equivalent to the Maximum Likelihood Estimator of the coefficients.

If we don't have normally distributed errors, there can be issues with point estimates and confidence intervals for our coefficients. However, the effect of non-normal errors is usually pretty quick to wash out as the sample size increases, and it is not a primary concern when we're working with linear models.



_I appreciate any corrections or feedback to feedback@finlaymcalpine.com_