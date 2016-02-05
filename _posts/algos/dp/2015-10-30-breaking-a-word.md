---
layout: post
title: Breaking a Word
modified:
categories: algos/dp/
excerpt: "Breaking a word in an elegant way."
tags: [algo, dynamic programming, ending at technique]
image:
  feature:
date: 2015-10-30
---

Given a string and a dictionary containing all valid strings, return 1 if you can break the given string into smaller valid strings. A string is considered valid if it is in the dictionary.

### The Analysis
The brute-force solution is that we examine each position that we can break the given string into two sub-strings. We then check if the first-half is valid and we repeat our approach to check the second-half. If the second-half is breakable then we return 1, otherwise we move on to the next position. However, this problem has an exponential time complexity.

The elegant solution is that we maintain a boolean array whose length equals to the length of the given string. The array flags if a substring from 0 to a given index is breakable. With this approach, we can apply the following algo:

	For a given index i indicating a substring from 0 to i, we check if there is any 
	index j <= i such that the current substring can be broken into two valid 
	strings. 

This algo helps to reduce our running time to O(n^2) time in the worst case.

### Codes
{% highlight java %}
public int isBreakable(String s, ArrayList<String> dict) {
	
	boolean[] breakable = new boolean[s.length()];
	
	// for each substring starting from 0 and ending at i
	for(int i = 0; i < s.length(); i++) {
		// we look for any index j <= i at which we can 
		// break this substring
		for(int j = i; j >=0; j--) {
			boolean prev = (j == 0) ? true : breakable[j-1];
			if (prev && dict.contains(s.substring(j, i + 1))) {
				breakable[i] = true;
				break;
			}
		}
	}
	
	return (breakable[s.length() - 1]) ? 1 : 0;
}
{% endhighlight %}
