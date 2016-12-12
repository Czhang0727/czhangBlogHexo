---
title: Thinking about binary search
date: 2016-11-30 23:48:15
tags: ["code", "algorithm"]
---

I am current re-new my algorithm using Python. I had to say that Python makes algorithm much easier LOL.
I will keep update my algorithm experience.

Today I would like to review binary search, it used a lot in search.

Think about the following question:
``` python
	# I want to find a number in an array!
```
Normally, the first idea that comes out of my mind is loop over the array. Just like this:

``` python
	arr = [1,2,3,4,5,6,7]
	target = 6
	for index,num in enumerate(arr):
		if num == target:
			return index
	# Yea! It is working 
```
It took O(n) to find what we need, but what if we noticed the arr is sorted. Lets try binary search.

``` python
	arr = [1,2,3,4,5,6,7]
	target = 6
	
	# binary search
	low = 0
	high = len(arr) - 1
	mid = 0
	while low + 1 < high:
		mid = (low + high) / 2
		if arr[mid] > target:
			high = mid
		elif arr[mid] < target:
			low = mid

	if arr[low] == target:
		return low
	if arr[high] == target:
		return high
```
It is straight forward, saved some time in search, because we skipped some elements. When should we use binary search? Hmm...

Normally we spend O(nlogn) to sort, if the data will be more easy to deal with after search, we do need to use binary search.

Thinking about such a problem, we have a really really large 2-D array, we are trying to find a specific node in side, if we sort the array, we don't need double loop to find the node.




