---
layout: post
title: Sliding Window With Queue
modified:
categories: 
excerpt: "A double queue in action."
tags: [algo, data structure, queue, double-queue, index-tracking]
image:
  feature:
date: 2015-12-01
---

You are given an array of integers and a sliding window of size k moving from left to right. You need to return the maximum value of each movement of the sliding window.

### The analysis

It's obvious that if k is not less than array.length, then we return only the max of the array. What happens when k is less than array.length?

Because we need to track the maximum value after adding an element. The first data structure comes to mind is max-heap. However, we need to remove the left most element of the sliding window from the heap when sliding. Thus it involves finding the element in the heap and reconstructing the heap. This approach does not sound efficient.

Ok, so max heap does not work, we can think about brute-force approach by maintaining an integer array of size k. When we slide the window, we add the new element at the beginning of the array and remove the last element of the array. It now sounds like we are going to use a double-ended queue. 

Assuming that we still use an array as a double-ended queue, whenever we add a new element and remove the last element, we need to scan through the array to find the max element. Can we do much better than that?

Apparently yes. We do not need to keep all the elements in the array that are less than an incoming element. Thus whatever elements are left in the array, they are should be not less than the incoming one.

With the above approach, the array size does not necessarily equal to k. Therefore we can no longer remove the last element of the array. This problem can be solved by maintaining indices of elements instead of the elements.

In summary, our approach is to

- use a double-ended queue data structure (using ArrayList is fine too).
- store the indices instead of the elements in the queue.
- maintain only the indices of elements that are not less than the incoming one.
- remove all the indices that are out of boundaries of the sliding window.
- record the element of the index at the end of the queue as the max value of the current sliding window.

### Codes
{% highlight java %}
public ArrayList<Integer> findMaxOfSlidingWindow(ArrayList<Integer> a, int k) {

	ArrayList<Integer> res = new ArrayList<Integer>();
	
	if (a == null || a.size() == 0) {
		return res;
	}
		
	Deque<Integer> deq = new LinkedList<Integer>();
	int n = a.size();
	int i = 0;
		
	// Init the aux Array with size of at most k.
	// This also takes care of the case where k >= n.
	for(; i < Math.min(k, n); i++) {
		int v = a.get(i).intValue();
		while (!deq.isEmpty() && v > a.get(deq.getFirst().intValue()).intValue()) {
			deq.removeFirst();
		}
		deq.addFirst(i);
	}
	res.add(a.get(deq.getLast().intValue()));
		
	// For each step we add a new element, remove all element that is less
	// the new one, and add the element at the back of the array into the 
	// result array.
	for (; i < n; i++) {
			
		int v = a.get(i).intValue();
		
		// remove all element less than v
		while (!deq.isEmpty() && v > a.get(deq.getFirst().intValue()).intValue()) {
			deq.removeFirst();
		}
		deq.addFirst(i);
			
		// remove all indices that is out of bound
		while (!deq.isEmpty() && deq.getLast().intValue() < i - k + 1) {
			deq.removeLast();
		}
			
		res.add(a.get(deq.getLast().intValue()));
	}
		
	return res;
}
{% endhighlight %}

### Variants.
- How about min instead of max?
- How about sliding window from right to left?


