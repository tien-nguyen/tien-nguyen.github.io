---
layout: post
title: Flip to Gain Max 1s
modified:
categories: array
excerpt: Flipping by applying a cost function.
tags: [algo, array, applying cost, dynamic programming]
image:
  feature:
date: 2015-09-28
---

Given a binary string (0, 1), you need to find two positions i & j (i <= j) such that when you flip all the characters from i to j, the number of 1 in the binary string is maximized. Flipping here means that 0 becomes 1 and 1 becomes 0. Return an empty array of integer if you cannot find the two indices.

### The Analysis
The tricky part of this problem is to know how to assign the cost at each position. When we flip a 0 to 1, we will gain one 1, but when we flip a 1 to 0 we lose one 1. Thus the cost for 0 is 1 and the cost for 1 is -1.
Once we are done with assigning costs, we can easily find the subarray that gives us the highest value.

### Codes
{% highlight java %}
public ArrayList<Integer> flip(String a) {
	
	ArrayList<Integer> res = new ArrayList<Integer>();
	if (a == null || a.length() == 0) {
		return res;
	}
	
	int n = a.length();
	int[] costs = new int[n];
	int countOnes = 0;
	for(int i = 0; i < n; i++) {
		costs[i] = (a.charAt(i) == '1') ? -1 : 1;
		countOnes += (a.charAt(i) == '1') ? 1 : 0;
	}
	
	// if the string is all 1s, then we cannot flip.
	if (countOnes == n) {
		return res;
	}
	
	int max = 0; // we want to flip such that the max > 0
	int left = -1; // this is the left index.
	int right = -1; // this is the right index.
	int minIndex = -1; // point to where the index of the min is.
	int min = 0;
	int[] sum = new int[n];
	
	for(int i = 0; i < n; i++) {
		sum[i] = costs[i] + ((i == 0) ? 0 : sum[i-1]);
		
		int sumSubArray = sum[i] - min;
		
		if (sumSubArray > max) {
			max = sumSubArray;
			left = minIndex + 1;
			right = i;
		}
		
		if (sum[i] < min ) {
			min = sum[i];
			minIndex = i;
		}
	}
	
	res.add(left);
	res.add(right);
	
	return res;
}
{% endhighlight %}