

While the linear regression model is widely used, it's not always intuitive or well understood. This post aims to briefly cover the linear model, the assumptions involved, and what those assumptions allow us to do with the results of our regression.

Some sources that I will recommend on this topic (there are many) are:
1. Either _Introduction to Statistical Learning_, or _Elements of Statistical Learning_. They are both good references on linear regression from the perspective of predictive machine learning, although ISL does not get into the weeds of the linear model assumptions.
2. Hayashi's _Econometrics_ is a good resource that approaches the topic from an econometrician's perspective. I would describe this book as more explicit about the assumptions.
3. _[Linear Model and Extensions](https://arxiv.org/pdf/2401.00649.pdf)_ by Ding has a nice treatment of the linear model, and builds the assumptions into the linear piece by piece in Chapters 3-5. 

### The Model

If we have a response (or dependent) variable _Y_ and one covariate (or feature) _X_, we can model the relationship between these in the following way:

_Y = a \+ bX \+ e_

where _Y_, _X_, and _e_ are for each sample in our data.

A crucial assumption for the linear regression model is that the linear model correctly describes the data - that the true relationship between Y and X is linear. We can test this by looking at a plot of residuals vs fitted values. In many cases, we'll assume this based solely on looking at the plotted data. While not the most precise method, this will help quickly reject linear regression in cases where the relationship is clearly not linear.

### The Assumptions

Once we've plotted the data and verified that the linear model is appropraite, we then have to think about other assumptions we're making when we use linear regression.

There seems to be some confusion about the assumptions of linear regression. I hjave seen a number of articles that make incorrect claims. I'll try to set some of these straight below.

The first and most wrong claim about the assumptions of linear regression is that the data need to be normally distributed. That is not correct. We don't need any specific distribution of the data: the covariate X could be normally distributed (as might be the case with height), it could be log-normal (as might be the case for income), or many more distributions. We might even find that the covariate takes on a bimodal distribution clustered strongly around two values. None of these scenarios prevent us from using the linear model. However, if we have strongly clustered data or data that is sparse in some regions of the covariate space, we might want to think carefully about the data generating process underlying our data.

So what assumptions do we make when we use the linear model?

### Multicollinearity

First, we assume that there is no multicollinearity in our data: that we don't have features that are a linear combination of other features. This assumption is not actually prohibitive from a statistical point of view, but it does make the calculation of the model more difficult for the software. In the case that some features are highly correlated with one another, we won't have biased estimates of the coefficients, but we will widen the standard errors of our coefficient estimates. Intuitively, if two features are highly correlated, it is more difficult to precisely tease out the individual effect on the response variable.

A more technical treatment of the collinearity problem can be found [here](https://www.stat.cmu.edu/~larry/=stat401/lecture-17.pdf).

### Error Exogeneity

Second, we assume orthogonality of the errors, that __E(e) = 0__. That's to say that knowing the value of the _X_ matrix tells us nothing about the error for a specific observation. A scenario in which this assumption would fail would be the existence of an omitted variable that is correlated with our covariates (Omitted Variable Bias), __and__ has  correlation (i.e. some explanatory power) with the response variable.

If we omit such a variable, we are going to be capturing its effect on _Y_ in the coefficient we estimate for _X_. If we are