---
title: "Using Constants in PostgreSQL queries"
date: "2021-11-30"
tags: [postgres, database]
---

![An image representing database](featured-db.jpg)

# Introduction

Imagine we have a long query which has a lots of conditions, and many of the conditions has some kind of comparisions with literals instead of some other record value.

**Example:**

```sql
SELECT * FROM users u where (u.name='some name' or u.age=12 or u.email='qwe@qwe.qwe');
```

For example sake we are taking the name, age and email as constants.

Imagine these literal values used multiple times in the query. We will have very untidy query.

To make the query more readable and tidy we can use constants, like we do in normal programming languages where if some literal value is/will be used repeatedly then we define it as a constant.

## Solution

To achieve this we will be using the **`WITH`** Common Table Expression (CTE)

Common Table Expression are very powerful as they allow us to create temporary named result set which can be used within a SELECT, INSERT, UPDATE, DELETE query.

The result set is temporary and only limited to the scope of the query, it is not stored any where and exists only for the duration of the query.

Thus we are not creating any garbage after our query has been executed.

Let's see how our new query will look like

```sql
WITH consts AS (
    SELECT 'some name' as temp_name, 12 as temp_age, 'qwe@qwe.qwe' as temp_email
)
SELECT * FROM users u, consts c where (u.name= c.temp_name or u.age=c.temp_age or u.email=c.temp_email);

```

Notice that we are using the result set **`consts`** as we use a table.

## Conclusion

Thus we have seen how we can use named constants and how helpful are the CTEs to write compact, readable and efficient queries.
