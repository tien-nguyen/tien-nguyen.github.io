---
layout: post
title: P-value and Its Counter-intuitive Definition
excerpt: "Not counter-intuitive at all once you understand it."
tags: [data science, hypothesis testing]
image:
  feature:
date: 2015-09-02
---

For me the definition of p-value was counter-intuitive at first. I now take this chance to refresh myself about the definition of p-values and how to make a counter-intuitive definition become an intuitive one.

According to [^1] when we compute a p-value we compute *the probability - when assuming the null hypothesis were true - of observing a more extreme test statistic in the direction of the alternative hypothesis than the one observed*. That is when p-value is small, the probability is small and vice versa. If p-value is smaller than a predefined alpha threshold, we can reject the null hypothesis in favor of the alternative hypothesis.

First time I read this statement, I was very confused. It's because based on the definition when the probability is small then the chance of observing more extreme test statistics in the direction of the alternative hypothesis is also small. How can we reject the null hypothesis?

In order to understand this definition, we need to go through an example.

Let's assume there is a belief which states: *The average height of men is 1.5 meters*. However, we as scientists believe otherwise that *the average height of men is not 1.5 meters*. Thus we form two hypotheses:

* The null: the average height is 1.5 meters
* The alternative: the average height is not 1.5 meters.

We run around to measure the heights of all men we can see or carry out surveys, and the end of the day, we have tons of data. We pick our test statistics as the mean. We do summary statistics and see that the height mean of our collected data is 1.75. Now let's look at the definition.

According to the definition, we need to assume that the null hypothesis is true. Then we based on the t-statistic compute p-value and it turns out p-value equals to 0.01 (wow very small). That is we only have 1% chance of seeing test statistics in **the direction of the alternative hypothesis**. 

But we already see that the observed test statistic is already in the direction of our alternative hypothesis (1.75 meters > 1.5 meters). That means either:

- a) the null hypothesis is wrong. Why? Because our chance of seeing what we believe is 100% given that we only collect data once.
- b) the null hypothesis is not wrong and we are just damn super lucky to get the data where things only happen with the probability of 0.01.

Because we are not sure if (a) or (b) happens to us. Therefore before computing p-values, we need to declare a threshold alpha to minimize the chances of us being just lucky. In technical term, it means to avoid Type I error (i.e. rejecting null hypothesis when it is true).


[^1]: <https://onlinecourses.science.psu.edu/statprogram/node/138>