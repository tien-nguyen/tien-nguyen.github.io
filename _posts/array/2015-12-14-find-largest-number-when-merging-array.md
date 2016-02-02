---
layout: post
title: Find Largest Number When Merging Array
modified:
categories: array
excerpt: "Cleverly merging and producing max numbers"
tags: [algo, array, own-comparison, tricky]
image:
  feature:
date: 2015-12-14
---

Given an array of strings, return the maximum single number when merging all the elements in the array.
For example [3, 30, 34, 5, 9] then we return 9534330.


### The Analysis
This problem sounds complex but if we note some tricks below, then it become a solvable problem.

* We always want to have the highest digit comes first if it is possible. In order to achieve this goal, we treat the given array as string, and then sort these strings in lexicographically descending order as mentioned next.
* For given two strings X and Y, we merge as XY if X > Y in lexicographically order, YX otherwise.

### Codes
{% highlight java %}
public class Item implements Comparable<Item> {
	public int value = -1;
	
	public Item(int value) {
		this.value = value;
	}
	
	@Override
	public int compareTo(Item item2) {
		String s1 = String.format("%d%d", this.value, item2.value);
		String s2 = String.format("%d%d", item2.value, this.value);
		
		return s2.compareTo(s1); // lexicographically ordering.
	}
}

public String findMax(int[] array) {
	
	if (array == null || array.length == 0) {
		return "";
	}
	
	Item[] items = new Item[array.length];
	for(int i = 0; i < array.length; i++) {
		items[i] = new Item(array[i]);
	}
	
	Arrays.sort(items);
	// if the very first item after sorting is 0, then we got passed all zeros.
	if (items[0].value == 0) { 
		return "0";
	}
	
	StringBuffer br = new StringBuffer();
	for(Item item : items) {
		br.append(String.format("%d", item.value));
	}
	
	return br.toString();
}
{% endhighlight %}

