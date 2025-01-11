### Questions and Answers

#### 1. What is the difference between `UNION ALL` and `UNION`?
- **`UNION`**: This operation combines the results of two queries and removes duplicate rows. It is slower because it performs a distinct operation to eliminate duplicates.
- **`UNION ALL`**: This operation combines the results of two queries and includes all duplicates. It is faster because it does not perform the distinct operation.

Example:
```sql
SELECT requester_id FROM RequestAccepted
UNION ALL
SELECT accepter_id FROM RequestAccepted;
```


#### 2. Explain behaviour of order in different windows?
- you can use window functions without partition by
- but when you use order by it takes rows between RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
- Note that some window functions such as LAG, LEAD, ROW_NUMBER and RANK operate on the entire partition (by design) and behave differently.

### how to calculate rolling averages efficiently
use window function along with rows between 2 preceding and current row
https://stackoverflow.com/questions/56063397/how-to-understand-the-results-of-rows-between-2-preceding-and-current-row
https://www.linkedin.com/pulse/understanding-rows-between-clause-sql-rahma-hassan/

### sql query to concate text in two rows
### sql query to find all tables which has a particular column in it
