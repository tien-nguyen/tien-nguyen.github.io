---
layout: post
title: Dynamic programming - maximize your profits.
excerpt: "A clever way of using dymanic programming."
tags: [algo, dynamic programming]
modified: 2015-12-08
comments: true
---

The problem sounds very simple. You are given an array of non-negative integers where the ith element is the price of a stock on day i. Design an algorithm to find the maximum profit. You can do at most two pairs of transactions (buy-sell), and you can not buy and sell on the same day.

### An O(n) approach

A clever way to solve this problem is to break this problem into two subproblems. Then we apply dynamic programming technique to solve each subproblem.

**Problem 1**: we ask what the maximum profit we can gain till a given day. 

**Problem 2**: given the price of a day, when should we sell the stock (in the future) so that we can
achieve the maximum profit?

Then the solution is simply the sum of the solutions of the above two problems.

Notes that we can solve the two sub-problems in O(n) time. Thus time complexity is O(n).
Space complexity is also O(n).

One tricky part here is that we need to reason why this approach does not violate a rule set in
the problem - that is you can not buy and sell on the same day. I leave this out for you to think.

### Codes
{% highlight java %}
public int maximizeProfit(final ArrayList<Integer> a) {

	if (a == null || a.size() == 0) {
		return 0;
	}
	
	int[] maxProfitBeforeI = new int[a.size()];
	int min = Integer.MAX_VALUE;
	int maxprofit =  0;
	for(int i = 0; i < a.size(); i++) {
		if (a.get(i).intValue() < min) {
			min = a.get(i).intValue();
		}
		maxprofit = Math.max(maxprofit, a.get(i).intValue() - min);
		maxProfitBeforeI[i] = maxprofit;
	}

	maxprofit = 0;
	int i = a.size() - 1;
	int maxEle = 0;
		
	while (i >= 0) {
		int curPrice = a.get(i).intValue();
		if (curPrice > maxEle) {
			maxEle = curPrice;
		}
		
		maxprofit = Math.max(maxprofit, maxEle - curPrice + maxProfitBeforeI[i]);
		i--;
	}
	return maxprofit;
}
{% endhighlight %}