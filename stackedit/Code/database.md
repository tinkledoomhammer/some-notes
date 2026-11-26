
# Normal forms
## 1nf
1. don't use row order to convey information
1. don't mix types within a column
1. have a primary key for every table
1. don't repeat groups of columns
## 2nf
1. 1nf
1. each non-key attribute depends on the entire primary key

## 3nf
1. 2nf
1. no dependencies between non-key attributes
** every non-key attributes depends on the key, the whole key, and nothing but the key **

## Boyce-Codd (bcnf)
* every attribute depends on the key, the whole key, and nothing butt the key*
* including the key attributes 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTg5NTI5MDU0LC0xNzE5NTQwODQ3LDkyND
cyODgwXX0=
-->