# SQL

---

If you use SQL day-in and day-out, you can skim this section. 

Below notes are mostly from Mode Analytics (skim most, study window functions)

- A hash join has an expected complexity O(M + N). The classic hash join algorithm for an inner join of two tables first prepares a hash table of the smaller table. The hash table entries consist of the join attribute and its row. The hash table is accessed by applying a hash function to the join attribute. Once the hash table is built, the larger table is scanned and the relevant rows from the smaller table are found by looking in the hash table.
- This logarithmic time complexity is true for query plans where an **Index Scan** or clustered index scan is performed.
- LEFT JOIN and RIGHT JOIN each return unmatched rows from one of the tables—**FULL (outer) JOIN returns unmatched rows from both tables.** It is commonly used in conjunction with aggregations to understand the amount of overlap between two tables.
- Round down dates: DATE_TRUNC('year' , cleaned_date) AS year
- Pull out date component: EXTRACT('year' FROM cleaned_date) AS year
- Set timezone: CURRENT_TIME AT TIME ZONE 'PST' AS time_pst
- Window function: A  performs a calculation across a set of table rows that are somehow related to the current row. This is comparable to the type of calculation that can be done with an aggregate function. But unlike regular aggregate functions, use of a window function does not cause rows to become grouped into a single output row — the rows retain their separate identities. Behind the scenes, the window function is able to access more than just the current row of the query result.
    
    window function
    
    - Running total example: `SUM(duration_seconds) OVER (ORDER BY start_time) AS running_total`
    - narrow the window from the entire dataset to individual groups within the dataset: `PARTITION BY`
    - **`SUM**(duration_seconds) **OVER** (**PARTITION** **BY** start_terminal **ORDER** **BY** start_time)`
- **`ROW_NUMBER()** OVER (PARTITION BY start_terminal ORDER BY start_time) AS row_number`
- two identical values are given the same **rank**, whereas ROW_NUMBER() gives them different numbers. (arbitrary tie-breaker)
- **RANK()** would give the identical rows a rank of 2, then skip ranks 3 and 4, so the next result would be 5. **DENSE_RANK()** would still give all the identical rows a rank of 2, but the following row would be 3—no ranks would be skipped.
- NTILE(*# of buckets*)
- **LAG** pulls from **previous** rows and LEAD pulls from following rows:
    - duration_seconds -**LAG**(duration_seconds, 1) OVER (PARTITION BY start_terminal ORDER BY duration_seconds) AS difference
- Pivoting table: [https://medium.com/analytics-vidhya/pivot-table-in-aws-athena-or-presto-764661eaa0fd](https://medium.com/analytics-vidhya/pivot-table-in-aws-athena-or-presto-764661eaa0fd)
    - Cross join
    - Nested join
    - Inner join
- SQL Anti-patterns:
- `AND, OR, NOT` do not use index therefore are slow; use `IN, BETWEEN` in WHERE clause
- Put the largest table last in JOIN
- Common clarifying questions:
    - What do you mean by X? Is my understanding correct? (e.g. engagement, growth, best, etc.)
    - Is [column] unique?
    - Do we have all the data? Or specific samples (top N)?
    - If sampled - how’s sampling done?
    

## Functions I needed to look up

- `CROSS JOIN` : If you add a `WHERE` clause (if table1 and table2 has a relationship), the `CROSS JOIN` will produce the same result as the `INNER JOIN` clause
- `DATE_ADD()`: DATE_ADD(date, INTERVAL X DAY)
- `%` find remainder: n % 2 = 1
- `SELECT DATE_FORMAT("2017-06-15", "%Y-%m")`; %M will give the month in full name (e.g. June)
- not equal: `!=`  ; `<>` should be preferred, all things being equal, since it accords with the sql standard and is technically more portable. `!=` is non-standard, but most db's implement it.
- `LAG()` (find previous row): `Lag(col, 1, [default value]) OVER (PARTITION BY __ ORDER BY ___ ) as previous_value`
- `RANK` and `DENSE_RANK` are deterministic in this case, all rows with the same value for both the ordering and partitioning columns will end up with an equal result, whereas `ROW_NUMBER` will arbitrarily (non deterministically) assign an incrementing result to the tied rows.
- Rolling Total (cumulative sum)

```sql
SUM(quantity) OVER (
        ORDER BY order_date
    ) AS rolling_sum
```

- Just sum with previous row:

```sql
SUM(quantity) OVER (
        ORDER BY order_date
				ROWS BETWEEN 1 PRECEDING AND CURRENT ROW
    ) AS rolling_sum
```

## General Tips

- Need to consider edge case for IF/CASE WHEN and run thru test cases in head
- Make sure NULL is properly handled (filter out, coalesce, etc.)
- don’t forget sorting order, and GROUP BY
- self join will sometimes give duplicate records (especially if using `NOT` in join condition)
- always good to clarify how to handle ties (`rank` vs. `dense_rank` vs. `row_number`)
- always good to clarify if we want to count(1) or count distinct

---
## Practice
- Free practice problems: [https://leetcode.com/studyplan/top-sql-50/](https://leetcode.com/studyplan/top-sql-50/) 
- See [this repo](https://github.com/mrinal1704/SQL-Leetcode-Challenge/tree/master/Hard) for all Hard level questions on Leetcode
- Check out Hackerrank and [Mode Analytics](https://mode.com/sql-tutorial/sql-window-functions/) too. 
