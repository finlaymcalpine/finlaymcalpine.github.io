---
layout: post
title: "Andrew Huberman & Probability"
tags: ["probability"]
---

There's been a fair bit of discussion online in the past couple of days about a recent probability slip-up made by Andrew Huberman on his podcast. I'm not going to pile on the criticism (I avoid public off-the-cuff maths at all costs), but I do want to think about the topic a little. I'll start by briefly covering the mistake Huberman made, and then going on to consider an interesting extension I saw posted on X (or, _the platform formerly known as Twitter_).

A brief clip of the slip-up can be found [here](https://x.com/bcrypt/status/1788406218937229780)[^1]. In short: Huberman says that the probability of conceiving on any one attempt is 20%, each additional attempt increases the total probability of conceiving by 20%, so after 5 attempts you should expect to have conceived, if you haven't then you should think about seeing a fertility expert. Enough (virtual) ink has been spilled in explaining why this is incorrect so I won't do that here. Huberman provides his own correction in a later [post](https://x.com/hubermanlab/status/1788964558758965281) - much to his credit.

I do want to explore the idea in a way rased by [@trading_noise](https://x.com/trading_noise/status/1789033639587536978) based on the discourse. They set out the following problem:

_A couple learns that for a "typical" couple, the probability of conception is 20% per attempt, and that attempts are considered independent. This couple decides they will consult a fertility specialist if they have yet to conceive by attempt n, where n is the least number of attempts for which a "typical" couple would have at least a 99% probability of conceiving. What is the value of n?_

To answer this question, as well as the original question posed by Huberman, we have to follow some interesting logic that's not always clear at first glance. When we're presented with the probability of 20% (or 0.2), the first thought might be to add or multiply this number in some way. But that would be the wrong approach![^2] In fact, the 20% is something of a red herring here - while it's an important piece of information, it's not actually the number we need.

Let's lay this problem out using the idea of a [binomial model](https://en.wikipedia.org/wiki/Binomial_distribution). An attempt is a _trial_ with probability of _success_ (i.e. conception) of 0.2. Thus the probabilty of _failure_ (i.e. not conceiving) in a trial is 0.8. The question we're being asked revolves around the probability of conceiving at least once[^3], versus the probability of conceiving not at all. The latter is the key to answering these questions. What does it mean to not conceieve at all in a series of _n_ trials? It means exactly that, for every trial 1..._n_, each trial results in a failure. We know this happens in each independent trial with probability 0.8. Since we've assumed the independence of trials, we calculate this as $$0.8^{n}$$.

So when we are asked the question of how many trials we need for a minimum 99% chance of conceiving, the dual of this problem is to ask how many trials we need to have a less than 1% chance of failing to conceive. And since we know the general form for the probability of failing to conceive ($$0.8^{n}$$), we can set out to solve for the _n_ that makes this probability less than 0.01.

$$
0.8^{n} \lt 0.01 \implies n\ln(0.8) \lt \ln(0.01) \implies n \gt \frac{\ln (0.01)}{\ln (0.8)}
$$

Since the last value is approximately equal to 20.638 - but we can't have a fractional attempt - we can see that it will take at least 21 attempts for the typical couple to have a probability of failing to conceive of less than 1%. The mirror of this is that it will take 21 attempts for the couple to have a 99% probability of conceiving.

[^1]: Many posts of this mistake have rather unkind/critical captions, and this is the least unkind post I could find
[^2]: In fact, multiplying 0.2 by itself a number of times is going to give us the probability of conceiving _every_ time
[^3]: We can assume that, if conception occurred on the first attempt,