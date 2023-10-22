---
title: "Group Anagrams"
date: "2023-08-31"
description: "Leet code solution note for `Group Anagrams` problem"
tags: [medium, data structure, array, hash map, introduction]
ShowCodeCopyButtons: true
---

[Problem Link](https://leetcode.com/problems/group-anagrams/)

## Problem Statement
-----------------

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** strs = ["eat","tea","tan","ate","nat","bat"]

**Output:** [["bat"],["nat","tan"],["ate","eat","tea"]]

**Example 2:**

**Input:** strs = [""]

**Output:** [[""]]

**Example 3:**

**Input:** strs = ["a"]

**Output:** [["a"]]

**Constraints:**
```
1 <= strs.length <= 104
0 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.
```

---
## Solution:
---

Here we have input as a slice of string, and we need to group the strings that are anagrams.

Following is the logic of the below solution:
- for each string s
    - find the frequency of each character in `s`
    - create a key in a hash map using the frequency
    - append the string in the slice of string associated with the frequency key.
- for each key generate the expected response as slice of slice of string.


Here the important part is that for given 2 strings if they are anagrams then
the frequency of the characters in them will match.

For example:
If s1 = "eat", s2 = "ate" are 2 strings then,

the frequency of characters in s1 is 

`e - 1, a - 1, t - 1`

the frequency of characters in s1 is 

`a - 1, t - 1, e - 1`

Thus the key generated for both of them will be 
`1#1#1#`

and both the strings will be part of the same group,

so the map will represent it as
```
{
    "1#1#1#": ["eat", "ate"] // order doesn't matter 
}
```


**Code:**

```go
func groupAnagrams(strs []string) [][]string {
    m := make(map[string][]string)
    for i := range strs {
        key := getKey(strs[i]) // get frequency key
        m[key] = append(m[key], strs[i])
    }

    // generate response
    var res [][]string
    for k := range m {
        res = append(res, m[k])
    }

    return res
}

// getKey will take a string and will return a string which will 
// have the frequency of each character in a-z order separated by a `#`
func getKey(s string) string {
    // since the constraint is only small case english alphabets
    counts := [26]int{} 
    for i := range s {
        idx := s[i] - 'a'
        counts[idx] += 1
    }

    var key string
    for i := range counts {
        key += fmt.Sprint(counts[i]) + "#" 
        // adding a break point of `#` because frequency can be more than 1 digit, 
        // helping us to identify whether its a new frequency count or not.
    }

    return key
}
```

Interestingly this frequency counting has a whole different level of applications, you can check [Frequency Analysis](https://en.wikipedia.org/wiki/Frequency_analysis) for more details.

To see how to know whether 2 strings are anagrams of each other checkout 
[Valid Anagrams](/practice/valid-anagram)

Thank you for reading through, let me know if you have any feedback. You can always reach out to me on [@rajkumarGosavi](https://twitter.com/rajkumarGosavi) .