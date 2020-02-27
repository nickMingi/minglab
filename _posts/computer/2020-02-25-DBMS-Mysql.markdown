---
layout: "post"
title: "All about Mysql"
author: "mingi hong"
date: 2020-02-25 10:23:45 -0700
categories: "computer dbms"
permalink: /:categories/:title.html
---

## MYSQL

- mysql -u root -p
- Show database
- Use database

![mysqldivide](/minglab/assets/mysqldivide.png)

What determines the sequence of operations?
- Order of precedence
- Parentheses

![mysqlconcat](/minglab/assets/mysqlconcat.png)

Terms to know
- function
- parameter
- argument
- concatenate

![mysqlleft](/minglab/assets/mysqlleft.png)

![mysqlformat](/minglab/assets/mysqlformat.png)

![mysqldistinct](/minglab/assets/mysqldistinct.png)

Dont't equal zero
- where credit_total <> 0
- where credit_total != 0

The order of precedence for compound conditions
- NOT
- AND
- OR

![mysqlin](/minglab/assets/mysqlin.png)

![mysqllike](/minglab/assets/mysqllike.png)

![mysqlreg1](/minglab/assets/mysqlreg1.png)

![mysqlreg2](/minglab/assets/mysqlreg2.png)

![mysqlreg3](/minglab/assets/mysqlreg3.png)

The default sequence for an ascending sort
- null values
- special characters
- numbers
- letters

note
- null values appear first in the sort sequence, even if you
are using desc

Don't count first two

![mysqllimit](/minglab/assets/mysqllimit.png)

why do we use join?
- same columns in two different tables

![mysqljoin](/minglab/assets/mysqljoin.png)

To use another database(scheme)

![mysqljoindatabase](/minglab/assets/mysqljoindatabase.png)

Using self join to get common

![selfjoin](/minglab/assets/selfjoin.png)

We can join N-1 tables to one table

Also we can implicitly join tables not using join

![implicitjoin](/minglab/assets/implicitjoin.png)

# Terms to know about inner join
1. join
2. join condition
3. inner join
4. ad hoc relationship
5. qualified column name
6. table alias
7. schema
8. self-join
9. explicit syntax
10. implicit syntax

Outer join retrieve unmatched data as well as matched data from left or right

![outerjoin](/minglab/assets/outerjoin.png)

Combine join

![combinejoin](/minglab/assets/combinejoin.png)

Simpler join using

![mysqlusing](/minglab/assets/mysqlusing.png)

What is the meaning of natural here?
- Based on common field for example vendor ID

![naturaljoin](/minglab/assets/naturaljoin.png)

Cross join
- join two table with all possible combinations
- If we have 5 rows in A, 9 rows in B then we will get 45 rows
- It is basically same as select * from A, B

# Terms to know about other types of join
1. Outer join
2. Left outer join
3. Right outer join
4. Equijoin
5. Natural join
6. Cross join
7. Cartesian product

Rules for a union
1. each result set must return the same number of columns
2. the corresponding columns in each result set must have compatible data types
3. the column names in the final result set are taken from the first select clause

![mysqlunion](/minglab/assets/mysqlunion.png)

If you want to use full outer join, you have to use union

![mysqlfullouterjoin](/minglab/assets/mysqlfullouterjoin.png)






