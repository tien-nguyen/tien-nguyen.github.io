---
layout: post
title: Finding the Max Gap in an Array
modified:
categories: array
excerpt: "Finding the maximum gap in an array."
tags: [array, binary search, frontier approach, sorting]
image:
  feature:
date: 2016-02-02T22:25:38-08:00
---
Given an array A of integers, find the maximum of j - i such that A[i] <= A[j]. 
If there is no solution possible, return -1.

### The Analysis
The brute-force solution is simple with two loops nested. However, we are looking for a better solution. 
A O(nlog n) solution should come in mind after an O(n^2) solution. An O(n log n) approach is usually searching on a sorted array. But we need to have at least a sorted array first. How?

We apply a frontier approach technique from both ends. We create an array to keep the minimum element (including the current one) from left to right. We create another array to keep the maximum element (including the current one) from right to left. Thus the first array is non-increasing from the left and the second array is non-decreasing from the right. Let's call the first array minLeft and the second array maxRight.

Then, for each element in the maxRight from right to left, we need to find the first position in minLeft that is smaller than the element. Then we update the distance. (**If it helps, we can visualize two non-increasing curves running from left to right - one curve will be above the other where x-axis is the index and y-axis is the value**). 

Because the the minLeft is non-increasing, thus when we apply a binary search technique, we should watch out for the sides we perform the next search.

### Codes
{% highlight java %}
public int maximumGap(ArrayList<Integer> a) {
	
	if (a == null || a.size() == 0) {
		return 0;
	}
	
	// build minLeft
	int[] minLeft = new int[a.size()];
	for(int i = 0; i < a.size(); i++) {
		int v = a.get(i).intValue();
		minLeft[i] = (i == 0) ? v : Math.min(minLeft[i-1], v);
	}
	
	// build maxRight
	int[] maxRight = new int[a.size()];
	for(int i = a.size() - 1; i >= 0; i--) {
		int v = a.get(i).intValue();
		maxRight[i] = (i == a.size() - 1) ? v : Math.max(maxRight[i+1], v);
	}
	
	int max = 0;
	
	for(int i = a.size() - 1; i >= 0; i--) {
		int v = maxRight[i];
		int j = findIndex(minLeft, v);
		// if j > i thus i - j < 0, will be ignored by Math.max
		max = Math.max(i - j, max); 
	}
	
	return max;
}

public int findIndex(int[] minLeft, int value) {
	
	int hi = minLeft.length - 1;
	int lo = 0;
	int index = -1;
	
	// an equal sign here is important.
	while (lo <= hi) {
		int mid = lo + ((hi - lo) >> 1);
		int midV = minLeft[mid];
		
		if (midV > value) {
			lo = mid + 1;
		} else {
			index = mid;
			hi = mid - 1;
		}
	}
	
	return index;
}
{% endhighlight %}