---
title: "Weekend musing a Todo app"
date: "2023-09-26"
slug: "bson-object-too-large"
tags: [experience, nodejs, postgres, docker, golang]
draft: true
---

# Introduction

After an arduous week, I got some time to on my weekends to do my own thing. I wanted to build something end to end not production level but something that comes close to it.
So I began developing the thing that has been already built or atleast attempted at thousands of time by developers i.e. **The Todo App**.

Adding to the twist of it, instead of using my primary language i.e. Golang I went ahead and decided to use Node.js and instead of simply testing on my local I decided to containerize it with data persistance using PostgreSQL. 

Well there is no cost associated with thinking this high, this realisation came to me earlier than I expected. This is an article where I go through things that happened and the results of it.

## Requirements

A todo app that:

1. Allows user to create a task containing title, description, status(open, in_progress, completed), created_at, updated_at.
2. To update a particular task using its id.
3. To delete a particular task.
4. To get all the tasks.
5. To get metrics and monthly metrics. How many tasks are open, in_progress, completed.
