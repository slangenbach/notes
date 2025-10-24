# The ultimate MySQL Bootcamp

Notes on the 2023 edition of the course from [Udemy][1]

## General

- `DISTINCT` operates on columns which are selected
- You can order by multiple columns, i.e. using `ORDER BY col1, col2 DESC`, etc.
- `LIKE` can use [wildcards][2]
- Reminder: `COUNT(*)` counts all the rows
- Make sure to start reasoning at the end of the query when using GROUP BY
- Subqueries always run first and need to be aliased (AS subquery)
- Use `BETWEEN` to filter for ranges in `WHERE` clauses
- Cast values into different data types using `CAST(<value> AS <datatype>)`
- Use `CASE` to apply if-else logic to `SELECT` statements
  ```sql
  CASE
      WHEN <condition> THEN <value>
      ELSE <value>
  END AS <alias>
  ```
- Views help to make
- Use `ROLLUP` to compute summary statistics for entire tables

[1]: https://covestro.udemy.com/course/the-ultimate-mysql-bootcamp-go-from-sql-beginner-to-expert
[2]: https://dev.mysql.com/doc/refman/8.0/en/pattern-matching.html
