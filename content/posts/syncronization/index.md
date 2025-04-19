---
title: "Mutex"
date: "2025-04-14"
slug: "share-by-communicating-2"
tags: [concurrency, channel, golang]
draft: true
---
No task left waiting,
a pool of patient workers
grind through sleepless loops.
## Introduction

In the previous [article](https://rajkumargosavi.github.io/posts/share-by-communicating-1/) we saw how Go differs in way of communicating between co-routines, where instead of "Sharing memory" we "Share by Communicating". This leads to an inquiry on how exactly we share our data with multiple routines and feel safe about all the issues arising when working with concurrent systems.

### Challenges in Syncronization
1. Deadlock
2. Race Condition

## Solutions in Go
1. Mutex
2. RWMutex
3. WaitGroup
4. Channels


### Mutex

A mutex (from mutual exclusion) is a synchronization primitive that *prevents state from being modified or accessed by multiple threads of execution at once*. Locks enforce mutual exclusion concurrency control policies.

```go
package main

import (
	"fmt"
	"strings"
	"sync"
	"time"
)

type User struct {
	name string
	m    sync.Mutex
}

func mutexExample() {
	u := User{
		name: "John",
		m:    sync.Mutex{},
	}

	go greet(&u)
	go title(&u)
	go addMR(&u)

	time.Sleep(1 * time.Second)

}

func title(u *User) {
	fmt.Println("calling title")
	u.m.Lock()
	u.name = strings.ToTitle(u.name)
	u.m.Unlock()
	fmt.Println("title call ended")
}

func addMR(u *User) {
	fmt.Println("calling addMR")
	u.m.Lock()
	u.name = "Mr. " + u.name
	u.m.Unlock()
	fmt.Println("addMR call ended")
}

func greet(u *User) {
	fmt.Println("Hello", u.name)
}

func main() {
	mutexExample()
}
```

In this example we have 2 go routines which are trying to update a shared `User` value. Notice how both the `title` and `greet`
functions take pointer to the `User` this is because we have the user details and the mutex that will handle the locking and unlocking 
for these fields together in the struct. This allows us to keep the lock and the things that it locks close to each other, thus it becomes easy to read and debug.
Imagine instead we would have kept a mutex a global level in main and passed it separately in all the functions, and due to some random function which was not meant to use this mutex
now the program is in a deadlock state, because it forgot to `Unlock` after updating.

Notice how `greet` does not need a `mutex` `Lock` and `Unlock` to use the `name` field, as `greet` is just reading the value.

Now between `title` and `addMR`, based on which go routine runs first and which acquires lock one of them can be a output.

In any case you will see that one of the writer functions is able to update the value of `name`. The critical section that is our field `name` in `User` is now safe.


Responses:
```go
1 ---
calling addMR
Hello John
addMR call ended
calling title
title call ended

2 --

calling addMR
addMR call ended
Hello Mr. John
calling title
title call ended

3 --
calling addMR
calling title
Hello John
addMR call ended
title call ended

4 --
calling addMR
addMR call ended
calling title
title call ended
Hello MR. JOHN
```