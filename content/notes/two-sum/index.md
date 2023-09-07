---
title: "Two sum"
date: "2023-08-31"
description: "Leet code solution note for `Two sum` problem"
tags: [easy, data structure, hash map, introduction]
ShowCodeCopyButtons: true
---

[Problem Link](https://leetcode.com/problems/two-sum/)

## Problem Statement
-----------------

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9

**Output:** [0,1]

**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

**Input:** nums = [3,2,4], target = 6

**Output:** [1,2]

**Example 3:**

**Input:** nums = [3,3], target = 6
**Output:** [0,1]

---
## Solutions:
---

### 1. Brute force
Given that we have to find the target value by summing two elements in the array, we can achieve this by simple brute force, where we loop over all the n elements in the array and the we have an inner loop iterating over the remaning n-1 elements, we compare the sum `arr[i] + arr[j]` with the `target` and if it satisfies the condition we return the indexes i,j.

**Time complexity: O(n^2)**  
**Space complexity: O(1)**

**Code:**

```go
func twoSum(nums []int, target int) []int {
    for i := 0; i < len(nums); i++ {
        for j := i+1; j<len(nums);j++ {
            if nums[i]+nums[j] == target {
                return []int{i,j}
            }
        }
    }

    return []int{}
}

```

### 2. Hash map
In this approach we will have a hash map that will contain array elements that we have traversed so far along with their indexes. Here we are going to use the property of

```
if a + b = c
then
b = c - a

```
where 
- a is the current candidate element,
- b are the elements that we have already traversed,
- c is the target that is known variable.

Thus given an array [1,2,3] and target 5, this approach will work in this manner:

Map<Key,Value> = Map<element, index>

1. a: 1, b: {1:0}, c: 5 (here we check if c-a (5-1 = 4) is present in the hash map b, else add `1` to the map b of traversed elements)
2. a: 2, b: {1:0, 2:1}, c: 5 (here 5-2 = 3 is it present in the map?, if not then add the current element `2` in map and continue)
3. a: 3, b: {1:0, 2:1}, c: 5 (here 5-3 = 2, `2` is present in the map of previously traversed elements), that implies that the current element + the earlier encountered element gives us the target value).

Since we have the index of the previous element in the map and we are standing at the current element we know both the indexes that we need to return in the output

**Time Complexity: O(n)**

**Space Complexity: O(n)**

**Code:**

```go
func twoSum(nums []int, target int) []int {
    prev :=  make(map[int]int)

    for i := range nums {
        diff := target - nums[i]
        _, exists := prev[diff] // check if the difference exists in the map of already traversed elements
        if exists { // if exists then we have our pair we can now return our result
            return []int{prev[diff], i}
        }

        prev[nums[i]] = i // else add the current element to the map and move to the next element
    }

    return []int{}
}
```

Thank you for reading through, let me know if you have any feedback. You can always reach out to me on [@rajkumarGosavi](https://twitter.com/rajkumarGosavi) .