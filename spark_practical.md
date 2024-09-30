1. Write code to fetch data from oracle db
2. How to handle multi delimiters
   way1 -> explod(split(df.marks, "\\|"))
   way2 -> use withcolumns
3. Avoid withcolumn chaining
4. Pivot and unpivot dataframes
5. Pivot dataframes with multiple values for aggregator column
6. Check the Count of Null values in each column
7. Final duplicate emails in a table named person with id, email, name -> select count(*), email from test group by email having count(*) > 1
8. get all customers who ordered everuthing --> groupBy(col("customerId")).agg(contDistinct(col("productKey")).alias("order_product_count"))
9. get the employees, dept id with maximum and minimum salary in each department
10. get all dataframes associated with this spark session or notebook and display them --> use global method 
    for k, v in globals().items():
             if(type(v)==Dataframe):
                   print(k)
12. find missing number in a column --> df1 = df.select(min(col("id)).alias("min_number"), max(col("id)).alias("max_number")
                                         df2 = spark.range(df1.first()[0], df1.first()[1] + 1)
                                         df3 = df.subtract(df2)
14. from a dataframe save same datatype columns into different different datafrmae
15. given a transaction table(userid, transactiontype, amount) and user table(userid, amount, minAmount). Find all users who gone below their minAmount
16. 
