---
layout: post
title: More Points, More Candies
modified:
categories: algorithms 
excerpt: "Give children the candies they deserve."
tags: [algo, greedy approach]
image:
  feature:
date: 2015-11-15
---

It's Christmas time and children are lined up by a random order to receive candies. Children earned points during the year and now they expect some candies. They should receive more candies than their neighbors if their points are higher than their neighbors' and they must receive at least one candy. Return the number of candies received by each child such that the total candies are minimal.

### The analysis
The brute-force solution is to find the child with the lowest point and give him the lowest possible number of candies (not necessarily 1). However there is an elegant solution for this.

The elegant solution is to simplify the problem by only looking at left neighbors first then at right neighbors later.

- Looking-left-neighbor step: we start from the beginning of the array by assigning 1 to the first child. Then for a given child we assign an amount that equals to 
	- his left-neighbor's plus 1 if his point is higher than his left-neighbor's
	- his left-neighbor's if his point equals to his left-neighbor's
	- 1 otherwise
- Looking-right-neighbor step: we start from the end of the array. For a given child we compare his point to the right-neighbor's. If his point is greater than the right-neighbor then his point now equals to the max of his left-neighbor's and right-neighbor's + 1.

### Codes
{% highlight java%}
public ArrayList<Integer> findMinNumCandies(ArrayList<Integer> points) {
	
	if (points == null || points.size() == 0) {
		return 0;
	}
	
	ArrayList<Integer> candies = new ArrayList<Integer>();
	
	// Looking-left-step.
	candies.add(1);
	for(int i = 1; i < points.size(); i++) {
		int hisPoint = points.get(i).intValue();
		int leftPoint = points.get(i-1).intValue();
		
		if (hisPoint > leftPoint) {
			candies.add(candies.get(i-1) + 1);
		} else {
			candies.add(1);
		}
	}
	
	// Looking right-step
	for(int i = points.size() - 2; i >= 0; i--) {
		int hisPoint = points.get(i).intValue();
		int rightPoint = points.get(i+1).intValue();
		
		if (hisPoint > rightPoint) {
			int rightCandies = candies.get(i+1).intValue();
			int hisCandies = candies.get(i).intValue();
			candies.set(i, Math.max(hisCandies, rightCandies + 1)); // set to minimal points.
		}
	}
	
	return candies;
}
{% endhighlight %}










