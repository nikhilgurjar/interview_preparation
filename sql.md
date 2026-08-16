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
### explain sql normalization, its forms from 1 to 5 and how to violet each 
# write query to convert string to int data type
# Write a query to generate a time-series report for sales data, filling in missing dates with zero sales.
# Create a query to find all the products that were never purchased together in the same transaction.
# Implement a query to calculate the total time spent by each user on the platform, excluding overlapping time intervals.
# Write a query to segment customers into cohorts based on their first purchase date and calculate retention rates for each cohort.
# Identify the products that have been sold for 3 consecutive months but not for the 4th month.
# Calculate the median salary for employees grouped by department.
# Write a query to delete duplicate rows but keep the row with the earliest timestamp.
# Calculate the rolling retention rate for an app based on daily user activity.
# Identify the longest streak of consecutive days a customer has made a purchase
# Write a query to compare the sales of the same product across two different time periods.
# Find the top 3 customers who contributed the most to revenue in the last year.
# Write a query to find customers who have made purchases in every quarter of the year.
#
 6. You have account number, transaction amount, and date columns. Write a PySpark/SQL code to find the account that occurred the most in each month.
 7. You have employee names and years. Find employees who participated for 3 or more distinct years.
 8. Can we use window functions here instead of groupBy? Why or why not?
 9. Can you use lag function to check for consecutive years?
 10. Write an SQL query using window functions to find the most recent designation of each employee.
 11. Write a query to find users with 2+ purchases within 24 hours.
 12. 
