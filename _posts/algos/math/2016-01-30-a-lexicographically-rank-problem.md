---
layout: post
title: A Lexicographically Rank Problem
modified:
categories: algos/math
excerpt: "Compute how many characters before the ith by looking forward."
tags: [algo, math]
image:
  feature:
date: 2016-01-30T23:24:03-08:00
---

You are given a string, you have to return the rank of the string amongst its permutation sorted lexicographically.
For example: with a string as "acb", all the lexicographically permutations are:

	- abc
	- acb
	- bac
	- bca
	- cab
	- cba

Thus if you are given bca, the returned result is 4.

### The Analysis

Let the length of the given string be n, and the first character of the string be a character c, then the rank must be at least m*(n-1) where m is the number of characters lexicographically before c and n - 1 is the number of characters left. 

Then we move on to process the second character (say b), and apply the same procedure. We note that m now is the number of characters that are not before b in the given string and lexicographically before b, and that n now is n - 1.

We keep applying this procedure for each character until the end of the string.
The sum of all the computed rank is the final answer. 

**Note:** The below code does not handle overflow. It is to show how the implementation looks like.

{% highlight java %}
public int findRank(String s) {
	
	int rank = 0;
	for(int i = 0; i < s.length(); i++) {
		char[] chars = s.substring(i, s.length()).toCharArray();
		Arrays.sort(chars);
		
		// This part shows the clever of implementation to compute
		// the number of elements before i.
		int numBeforeI = 0;
		for(; numBeforeI < chars.length; numBeforeI++) {
			if (chars[numBeforeI] == s.charAt(i)) {
				break;
			}
		}
		
		int numLeft = s.length() - i - 1;
		rank += numBeforeI*numLeft; 
		
	}
	
	// rank is the number of element before it so we have to
	// add 1 to get its rank.
	return rank + 1; //
}
{% endhighlight %}