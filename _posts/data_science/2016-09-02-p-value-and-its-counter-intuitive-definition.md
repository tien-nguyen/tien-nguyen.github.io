---
layout: post
title: P-value and Its Counter-intuitive Definition
excerpt: "Not counter-intuitive at all once you understand it."
tags: [data science, hypothesis testing]
image:
  feature:
date: 2015-09-02
---

For me the definition of p-value was counter-intuitive at first. I want to make it more intuitive.

According to this article [^1], when we compute a p-value we compute *the probability - when assuming the null hypothesis were true - of observing a more extreme test statistic in the direction of the alternative hypothesis than the one observed*. That is when p-value is small, the probability is small and vice versa. If p-value is smaller than a predefined alpha threshold, we can reject the null hypothesis in favor of the alternative hypothesis.

First time I read this statement, I was very confused. How can we reject the null hypothesis when the chance of observing more extreme test statistics in the direction of the alternative hypothesis is also small?

In order to understand this definition, let's look at an example. We want to test *the average height of men is not 1.5 meters*. Hence:

* The null: the average height is 1.5 meters
* The alternative: the average height is not 1.5 meters.

We collect our data and our test statistics is the mean. We do summary statistics and see that the mean of the heights of the data we collected is 1.75. According to the definition, we need to assume that the null hypothesis is true. Based on the t-statistic we compute p-value and it turns out p-value equals to 0.01. That is we only have 1% chance of seeing test statistics in **the direction of the alternative hypothesis**. 

But we already see that the observed test statistic is already in the direction of our alternative hypothesis (1.75 meters > 1.5 meters). That means either:

- a) the null hypothesis is wrong. Why? Because our chance of seeing what we believe is 100% given that we only collect data once.
- b) the null hypothesis is not wrong and we are just lucky to observe that data.

Because we are not sure if (a) or (b) happens to us. Therefore before computing p-values, we need to declare a threshold alpha to minimize the chances of us being just lucky. In technical term, it means to avoid Type I error (i.e. rejecting null hypothesis when it is true).


[^1]: <https://onlinecourses.science.psu.edu/statprogram/node/138>