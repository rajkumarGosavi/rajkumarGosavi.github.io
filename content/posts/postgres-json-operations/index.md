---
title: "JSON datatype in postgreSQL"
date: "2022-08-11"
description: "JSON datatype in postgreSQL"
tags: [postgres, database]
---

![An image with json text](json-featured.jpg)

# Introduction

PostgreSQL provides us with a datatype to store JSON data.
This datatype can be helpful in lots of situations giving us a way to store some dynamic data.

Further PostgreSQL provides us with two types for storing JSON data

1. json
2. jsonb

They both take almost identical set of inputs but the major difference is that in the efficiency.

The `json` data type stores the exact copy of the input text which the processing functions must reparse
on each execution.
On the other hand `jsonb` data is stored in the decompressed binary format which makes it slightly slower
to input due to the added overhead of conversion, but significantly faster to process since no reparsing is needed.

Further `jsonb` also supports indexing which gives a significant advantage.

Because `json` type stores the exact copy of the input text, the white spaces tokens are also stored,
and the order of the keys are maintained. But if the json input contains duplicate keys the `json` datatype
will store duplicate keys as well, in such case the last key in the object will be considered, `json` type
will also accept numbers that are out of range of the PostgreSQL `numeric` type.

By contrast `jsonb` does not store the extra white spaces, does not preserve the keys order,
and if duplicate keys are present it only considers last key value pair while storing, `jsonb` rejects
number outside the range of `numeric` type.

## JSON Input and Output Syntax

Following are valid `json`(`jsonb`) expressions:

- Simple scalar/primitive values
- They can be numbers, quoted strings, true, false, null

```sql
SELECT '5'::json;
```

- Array of zero or more elements (similar to normal JSON types the elements need not be of same type)

```sql
SELECT '[1,2,"some text", null]'::json;
```

- Objects with key value pairs (with keys always being quoted strings)

```sql
SELECT '{"name": "fname", "age": 12, "hobbies": [{"name":"Singing", "proficiency": 3}, "address": null]}'::json;
```

Keep in mind the difference between the properties stated above for `json` and `jsonb` types.

`jsonb` has 2 special operator on it

1.  One checks for `containment` of values (rules are a bit complex so please refer the offical docs for the same)

    ```sql
    SELECT '["foo", "bar"]'::jsonb @> '"bar"'::jsonb; -- holds true
    -- A top-level key and an empty object is contained:
    SELECT '{"foo": {"bar": "baz"}}'::jsonb @> '{"foo": {}}'::jsonb;
    -- Order of array elements is not significant, so this is also true:
    SELECT '[1, 2, 3]'::jsonb @> '[3, 1]'::jsonb;

    SELECT '{"foo": {"bar": "baz"}}'::jsonb @> '{"bar": "baz"}'::jsonb;  -- yields false
    SELECT '"bar"'::jsonb @> '["bar"]'::jsonb;  -- yields false
    ```

2.  The `existence` operator that tests whether a string appears as an object key or an array element
    at the top level of the `jsonb` value.

    ```sql
    SELECT '["foo", "bar", "baz"]'::jsonb ? 'bar';
    -- String exists as array element:

    -- String exists as object key:
    SELECT '{"foo": "bar"}'::jsonb ? 'foo';

    -- Object values are not considered:
    SELECT '{"foo": "bar"}'::jsonb ? 'bar';  -- yields false

    -- As with containment, existence must match at the top level:
    SELECT '{"foo": {"bar": "baz"}}'::jsonb ? 'bar'; -- yields false
    ```

## Querying JSON data

PostgreSQL provides `jsonpath` type to efficiently query JSON data.

Conventions used:

- Dot(.) used to access member
- Square bracket([]) to access array

### jsonpath variables

- **$** => A variable representing the JSON value being queried
- **$varname** => A named variable
- **@** => A variable representing the result of path evaluation in filter expressions

### jsonpath Accessors

| Accessor Operator               | Description                                                                          |
| ------------------------------- | ------------------------------------------------------------------------------------ |
| .key                            | Member accessor that returns the object member with the specified key                |
| ."$varname"                     | Member accessor that returns the object member with the specified key                |
| .\*                             | Wildcard member accessor that returns the values of all members located at top level |
| .\*\*                           | Recursive Wildcard accessor that processes all levels                                |
| .\*\*{level}                    | Like \_.\*\*\_ but upto the level specified                                          |
| .\*\*{start_level to end_level} | Like \_.\*\*\_ but upto the level specified                                          |
| [subscript, ...]                | Array element accessor can take an index or start_index and end_index                |
| [\*]                            | Wildcard array element accessor that returns all array elements                      |

## Examples

```sql
-- create
CREATE TABLE EMPLOYEE (
  empId INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  dept JSONB NOT NULL
);

-- insert
INSERT INTO EMPLOYEE VALUES (0001, 'Clark', '{"name": "Sales", "posts": ["clerk", "HOD"]}');
INSERT INTO EMPLOYEE VALUES (0002, 'Dave', '{"name": "marketing", "posts": ["clerk", "HOD", "manager"]}');
INSERT INTO EMPLOYEE VALUES (0003, 'Ava', '{"obj1": {"key1": {"key2": "value2"}}, "posts":["consultant"], "name":"advertising"}');

-- fetch
SELECT jsonb_path_query(dept, '$.obj1') FROM EMPLOYEE;
--  {"key1": {"key2": "value2"}}

SELECT jsonb_path_query(dept, '$.obj1.key1') FROM EMPLOYEE;
--  {"key2": "value2"}

SELECT jsonb_path_query(dept, '$.posts[*]') FROM EMPLOYEE;
-- "clerk"
-- "HOD"
-- "clerk"
-- "HOD"
-- "manager"
-- "consultant"

SELECT jsonb_path_query(dept, '$.**') FROM EMPLOYEE;
--  {"name": "Sales", "posts": ["clerk", "HOD"]}
--  "Sales"
--  ["clerk", "HOD"]
--  "clerk"
--  "HOD"
--  {"name": "marketing", "posts": ["clerk", "HOD", "manager"]}
--  "marketing"
--  ["clerk", "HOD", "manager"]
--  "clerk"
--  "HOD"
--  "manager"
--  {"name": "advertising", "obj1": {"key1": {"key2": "value2"}}, "posts": ["consultant"]}
--  "advertising"
--  {"key1": {"key2": "value2"}}
--  {"key2": "value2"}
--  "value2"
--  ["consultant"]
--  "consultant"

SELECT jsonb_path_exists(dept, '$.** ? (@ == "marketing")') FROM EMPLOYEE;
-- jsonb_path_exists
-------------------
--  f
--  t
--  f
SELECT jsonb_path_query_array(dept, '$.posts[0]') FROM EMPLOYEE WHERE dept::jsonb ->>'name'='advertising';
--  ["consultant"]

SELECT jsonb_path_query(dept, '$.name'), name FROM EMPLOYEE WHERE dept->'posts'->>0='clerk';
-- jsonb_path_query | name
-- ------------------+-------
--  "Sales"          | Clark
--  "marketing"      | Dave

SELECT jsonb_path_query(dept, '$.name'), name FROM EMPLOYEE WHERE dept->'posts' ? 'clerk';
--  jsonb_path_query | name
-- ------------------+-------
--  "Sales"          | Clark
--  "marketing"      | Dave

SELECT jsonb_path_query(dept, '$.name'), name FROM EMPLOYEE WHERE dept->'posts' ?| array['consultant', 'manager'];
-- jsonb_path_query | name
-- ------------------+------
--  "marketing"      | Dave
--  "advertising"    | Ava

SELECT jsonb_path_query(dept, '$.name'), name FROM EMPLOYEE WHERE dept->'posts' ?& array['consultant', 'manager'];
-- jsonb_path_query | name
-- ------------------+------
-- (0 rows)
```

