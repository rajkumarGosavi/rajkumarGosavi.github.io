---
title: "Group Anagrams"
date: "2023-08-31"
description: "Leet code solution note for `Group Anagrams` problem"
tags: [medium, data structure, array, hash map, introduction]
ShowCodeCopyButtons: true
draft: true
---
```go
package main

func main() {
	prefix := 1
	op := make([]int, len(i))
	for i := range op {
		op[i] = 1
	} 
	

	for i := range ip {
		op[i] = prefix
		prefix *= ip[i]
	}
	postfix := 1

	for i:= len(ip) - 1;i >= 0;i-- {
		op[i] *= postfix
		postfix *= ip[i]
	}

	return op
}
```