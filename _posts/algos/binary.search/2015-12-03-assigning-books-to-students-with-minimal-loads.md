---
layout: post
title: Assigning Books to Students With Minimal Loads
modified:
categories: algos/binary.search
excerpt: "Book assigning with binary search technique."
tags: [algo, binary search]
image:
  feature:
date: 2015-12-03
---

Given a list of n books with at least one page. A teacher needs to assign readings to m students such that:

- each student reads at least one book
- a book is read by one student only
- the number of pages read by the student assigned to read the most is minimal.

return the minimal number of pages read by the students who read the most or -1 if we cannot assign the readings.

Note: in this problem you cannot sort books based on numbers of pages. If you are given 3 books whose numbers of pages are [2,5,4] by this order and you have two students. There are two possible assigments as follow: {[2,5], [4]} or {[2], [5,4]}.


### The Analysis
The problem sounds complicated at first but all it says is that we do not want to overload our students while being as fair as possible. Let's analyze our approach.

First, if the number of books is less than the number of students then obviously we cannot assign books to students, we return -1. 
With a brute-force approach, we need to examine C(number of books - 1, number of students - 1) options.

If we look closer, we see that the answer is bounded by two numbers. The lower bound is the maximum number of pages a book has, and the upper bound is the total pages of all the books. Our task is now to find the right answer using binary search technique.

We can apply the technique as follow. First we set m = a number page as the middle of the range. Then we need to find s = a number of students to whom we can assign all the books such that they read at most m pages. If s is less than b that means  there are b - s students who are not assigned with any books, therefore we can decrease the upper bound. If s is greater than b that means at least one student must read more than m pages, hence we need to increase the lower bound. We stop with the normal stop condition of a binary search technique.

### Codes
{% highlight java %}
public int findMinOfMaxLoad(ArrayList<Integer> books, int m) {
	
	if (books == null || books.size() < m) {
		return -1;
	}
	
	// Initalize the boundaries.
	int low = Integer.MIN_VALUE;
	int high = 0;
	for(int i = 0; i < books.size(); i++) {
	    low = Math.max(low, books.get(i).intValue());
		high += books.get(i).intValue();
	}
	
	// we search for the right answer.
	while (low < high) {
		int numPage = low + ((high - low) / 2);
		
		int numStudent = findNumStudent(books, numPage);
		
		if (numStudent <= m) {
			high = numPage;
		} else {
			low = numPage + 1;
		} 
			
	}
	
	return low;
}

public int findNumStudent(ArrayList<Integer> books, int numPage) {
	
	int numStudent = 1; // we should have any least one student.
	
	int total = 0; // the number of pages this student can read
	
	for(Integer book : books) {
		total = total + book.intValue();
		
		// Check if this student is overloaded. If yes we add one student
		// and assign this book to this current student.
		if (total > numPage) { 
			numStudent++;
			total = book.intValue();
		}
	}
	
	return numStudent;
}

{% endhighlight %}






