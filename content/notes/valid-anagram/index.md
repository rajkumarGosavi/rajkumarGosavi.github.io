---
title: "Valid Anagram"
date: "2023-08-28"
description: "Leet code solution note for `valid anagram` problem"
tags: [basic, data structure, hash map, introduction]
ShowCodeCopyButtons: true
---

[Problem Link](https://leetcode.com/problems/valid-anagram)

## Problem Statement
-----------------

Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input**: s = "anagram", t = "nagaram"

**Output**: true

**Example 2:**

**Input**: s = "rat", t = "car"

**Output**: false


---
## Solutions:
---

One of the base case is that both the strings should have same length, if length is not same then there are extra occurrence of characters and thus they are not anagram.

### 1. Sorting

Here we will first sort both the strings, thus all the characters are now grouped.
now we just iterate over the length of the string and see 
`if s[i] == t[i]` if this is valid for the entire length of the string then they are valid anagram.

This does not require an extra memory allocation as needed for the next solution.
  
**Time complexity: O(nlogn) (sorting takes O(nlogn), iteration takes O(n))**  
**Space complexity: O(1)**

```go
func sortString(w string) string {
    s := strings.Split(w, "")
    sort.Strings(s)
    return strings.Join(s, "")
}

func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    s = sortString(s)
    t = sortString(t)

    for i := 0; i<len(s); i++ { // assuming ascii characters
        if s[i] != t[i]{
            return false
        }
    }

    return true
}
```

### 2. Hash Map

Given that we need to identify how many times the characters occur in string `s` and string `t`, hash map is an ideal way to do this. Because of the property that the key of a map is unique. Accessing the key is O(1) as well.

Here I have taken only 1 map and for each element in string `s` I populate the map and increment the count of element, and for every element in string `t` i decrement the count of the element. If the element of `t` is not present in map or the count is already zero after multiple decrements then that means the strings are not same.

We can also create 2 separate maps for each string `s` and `t` respectively, and then we simply compare the count of each element in both the maps, if the count does not match or one of the maps does not contain a key present in the other map in that case as well the two strings are not an anagram.

**Time Complexity: O(n)**

**Space Complexity: O(n)**
**(if all `n` characters are unqiue in a string the map size will be same as input)**

**Code:**

```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    m := make(map[rune]int)
    for _, r := range s {
        m[r] += 1
    }

    for _, r := range t {
        if m[r] == 0 {
            return false
        }
        m[r] -= 1
    }

    return true
}
```

Thank you for reading through, let me know if you have any feedback. You can always reach out to me on [@rajkumarGosavi](https://twitter.com/rajkumarGosavi) .