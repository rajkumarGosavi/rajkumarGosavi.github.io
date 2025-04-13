---
title: "Contains Duplicate"
date: "2023-08-27"
description: "Leet code solution note for `contains duplicate` problem"
tags: [basic, data structure, hash map, introduction]
ShowCodeCopyButtons: true
---

[Problem Link](https://leetcode.com/problems/contains-duplicate/)

## Problem Statement
-----------------

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**

**Input:** nums = \[1,2,3,1\] **Output:** true

**Example 2:**

**Input:** nums = \[1,2,3,4\] **Output:** false

**Example 3:**

**Input:** nums = \[1,1,1,3,3,4,3,2,4,2\] **Output:** true

---
## Solutions:
---

### 1. Brute force

Given that we have an array of elements to identify if a given element is duplicated, we will compare the element with the rest of the array and this needs to be done for all the elements in the array.

**Time complexity: O(n^2) Space Complexity: O(1)**

**Code:** 

```go
var a []int
for i := range a { // select an element `e`
	// check through the remaining array for occurrence of `e`
	for j := i+1; j< len(a); j++ { 
		if a[i] == a[j] { // if it occurs then array contains duplicate
			return true
		}
	}
}

return false // all elements are distinct
```

### 2. Sorting

Here we will sort the array first, thus given an array \[1,2,3,1\] after sorting we will get \[1,1,2,3\], Now we iterate over the array and just check the adjacent elements are same or not as they will now be grouped together after sorting.  
  
**Time complexity: O(nlogn) (sorting takes O(nlogn), iteration takes O(n))**  
**Space complexity: O(1)**

```go
var a []int

sort.Ints(a) //check sort pkg

for i := 0; i<len(a)-1; i++ {
	if a[i] == a[i+1]{
		return true
	}
}

return false
```

### 3. Hash Map

Given that we need to identify how many times a **distinct** element occurs, hash map is an ideal way to do this. Because of the property that the key of a map is unique. Accessing the key is O(1) as well.

**Time complexity: O(n)** 

**Space Complexity: O(m) where m is the number of unique elements (m ≤n)**

**Code:**  
 

```go
var a []int
m := make(map[int]struct{})

for i := range a {
	_, exists := m[a[i]] // check if the key already exists in the map
	// if so then this is the 2nd time we are encountering this element
	if exists {
		return true
	}
	
	m[a[i]] = struct{}{}
}

return false
```

Check https://dave.cheney.net/2014/03/25/the-empty-struct for more info on `empty struct`.

Thank you for reading through, let me know if you have any feedback.