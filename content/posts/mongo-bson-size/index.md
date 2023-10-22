---
title: "Handling Bson object size in Mongo"
date: "2023-09-16"
slug: "bson-object-too-large"
tags: [experience, engineering, mongo, golang]
draft: true
---

# Introduction

This week I faced an issue while running our mongo queries in one of the development environments. The issue itself is is nothing new but the way it got introduced in the system and how I decided to tackle it is something I would like to give importance to. I am using Golang as my programming language.

## The genesis

Here we have a requirement for an API saying to get the list of a resource for our dashboard. And this response should be paginated, so that on the UI we can show only X items on a single page and then we have the page numbers to navigate to the other pages.

To acheive this we decided to use a mongo aggregation pipeline, which is quite flexible and can structure our query into multiple stages.

Here are the list of things that our query must do
1. Filter the relevant data
2. Get the total count of filtered documents used to show the total matched docs and to derive how many pages are needed.
3. Sort documents
4. Skip documents based on the page number (offset)
5. Limit the number of documents to only the page size.
6. Project/Format documents

1,3,4,5,6 these are straight forward and have a 1-to-1 mapping with respective stages, the only activity that is tricky here is the 2nd one.

### How to count the documents in a mongo aggregation pipeline?
Well if we would have used the in built mongo function of `CountDocuments` we can simply pass the filter and could have gotten the count of filtered documents.
But along with the count we also wanted to do other operations to get the actual data.

Thus I 


```
BSONObjectTooLarge: BSONObj size: XXX is invalid. Size must be between 0 and 16793600(16MB)
```



