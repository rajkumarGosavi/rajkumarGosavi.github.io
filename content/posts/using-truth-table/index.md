---
title: "Using Truth Table in Development."
date: "2023-02-03"
slug: "using-truth-table"
tags: [experience, engineering]
---

# Introduction

Truth tables are the foundation of boolean algebra and are required for logical reasoning and software implementation.

All the `if` `else` statements we write, irrespective of the programming languages, use
boolean algebra.

One such scenario became the inspiration for this particular short post

## The Initial Details

To display some orders on a page where users have access to multiple filters,

such as

- search box - to search the order directly using some identifier
- date range filter - to get orders with a specific date range
- status - to get orders with a specific status

and many others

If the date range is not available, the default date range of 1 month is used.

## The Use Case

The user wants to get a particular order using an identifier that was placed 3 months ago

## The Chaos

Since the user knows the particular order details and directly searches using the available
search box, the user still won't be able to retrieve the order information.
Because the default date range used will be 1 month, the orders available in the
search space will be from the previous month as of today's date.

## The Solution

Assuming that the user knows what they are searching for, the default date range
limitation seems a bit binding.

Thus, we need to prioritise the search box filter as compared to the default date filter.

This is where we use a Truth table to get the permutation of all the scenarios
between the search text and the date.

which is as follows

| date range <br /> available | search text <br /> present |                 result                 |
| :-------------------------: | :------------------------: | :------------------------------------: |
|             Yes             |            Yes             |  search data within custom date range  |
|             Yes             |             No             |  search data within custom date range  |
|             No              |            Yes             | search data without default date range |
|             No              |             No             |        Take default date range         |

In this also we need to consider only 2 cases where the search text is provided:

| date range <br /> available | search text <br /> present |                 result                 |
| :-------------------------: | :------------------------: | :------------------------------------: |
|             Yes             |            Yes             |  search data within custom date range  |
|             No              |            Yes             | search data without default date range |

Where the user is actually giving the search text

Thus, using the above table we can write the condition as follows

```go
/* if date range and the search text are not provided then use the default date range
1. If the date range is present, the first part will fail and short circuit, preventing the default date range from being used.
2. if search text is provided, the second part is evaluated as false,
so default date range will not be used
1. if neither is supplied, the default date range will be used
*/
if request.IsDateRangeEmpty() && request.SearchText == "" {
    // use default date range of 1 month
}

// search without a date range
```

## Conclusion

Thus we have seen how we can use named constants and how helpful are the CTEs to write compact, readable and efficient queries.
