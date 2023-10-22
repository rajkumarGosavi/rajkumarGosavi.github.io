---
title: "Group Anagrams"
date: "2023-08-31"
description: "Leet code solution note for `Group Anagrams` problem"
tags: [medium, data structure, array, hash map, introduction]
ShowCodeCopyButtons: true
draft: true
---
https://leetcode.com/problems/top-k-frequent-elements/

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

 

Example 1:

Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

Example 2:

Input: nums = [1], k = 1
Output: [1]

 

Constraints:

    1 <= nums.length <= 105
    -104 <= nums[i] <= 104
    k is in the range [1, the number of unique elements in the array].
    It is guaranteed that the answer is unique.

 

Follow up: Your algorithm's time complexity must be better than O(n log n), where n is the array's size.



```go
func topKFrequent(nums []int, k int) []int {
    m := make(map[int]int)
    
    for i := range nums {
        m[nums[i]] += 1
    }

    freq := make([][]int, len(nums)+1)

    for k, v := range m {
        freq[v] = append(freq[v], k)
    }

    final := make([]int, 0)
    for i := len(freq)-1; i>=0; i-- {
        for j := range freq[i] {
            final = append(final, freq[i][j])
            if len(final) == k {
                return final
            }
        }
    }

    return final
}
```