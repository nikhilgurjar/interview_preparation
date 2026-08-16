### what are SCD and its types
   SCD -> slowly changing dimensions 
  they are used to cater things which changes from time to time
  SCD0 -> never changes when inserted like date of joining in employee table
  SCD1 -> address in customer table
        if you directly update address, you wont have historical data like where he was earlier
        another case if there is some analysis and report done on number of peples in a state, then if we change it directly and someone want to devise jan report again he cant as he will get latest data always
        so basically in SCD1 no history is tracked

  SCD2 
  - history will be retained
  - we can add 3 columns status, start date, end date
  - end date should not be kept null as it can create problems in between and join conditions/merge conditions

  ### SCD3 used in salesforce condition
  lets say there are 5 peoples under you, they goes to different cities and towns nearby for parcel
  now lets say range of parcel gicing increases
  new column are created with prior_column_name
  generally not used

  scd5 = 1+3
  scd6 = 1+2+3
  https://www.luzmo.com/blog/slowly-changing-dimensions#:~:text=Type%204,in%20a%20different%20table%20altogether.

### What is datawarehouse
data warehouse is system or platform used for the analysis andreporting of structured data from multiple sources such as point of sale transactions

### diffrence between database and datawarehouse
- multiple databases are kept at datawarehouse
- datawarehouse used to store large volume of data while database stores small volume of data
- datawarehouse is used for read heavy operations while database for write heavy operations
- datawarehouse has high latency while database has low
- datawarehouse data is denormalized while in databricks its highly normalized
- datawarehouse follows column storage while database follow row based
- prallel processing of request in supported in datawarehouse buts in databricks 

----------------------------------------------------------------------
OLAP - online analytical processing uses datawarehouse (terradata, snowflake)
OLTP - online transactional processing uses database (mysql, oracle, postgress)

----------------------------------------------------------------------------
if we save parquet in hdfs, this is also columnar and we do processing by spark this is also parallel processing why do we not use it 
- spark can handle semi and unstructured data as well while data warehouse works on structured data most of time 
- datawarehouse do not support streaming but spark does
- spark runs on commodity servers


### Fact table and dimensional table
fact -> measuremenet
dimensional table -> context
consider a sales data where there is product info, customer info, order info etc
Now fact table is table which just uses references of all above and create relationships
while product, customer, order is dimensions table 

- star schema is using of fact and dimensions as fact is in between and there are multiple dimension tables around it 
- dimensions table info changes rarely while fact table info changes frequently

Now lets say customer table is dimension table and now this customer table can have few other dimension tables 
its called snowflake schema 


- Natural key has business associated with it it can be primary key as well it can not be 
- natural key will be larger in size and may create memory issues
- surrogate key is factless key which do not change if there is any change in data

### Type of facts in ETL
1. additive 2. non additive 3. semi additive
- additive means you can add over all dimensions like gross revenue you can sum along any axis be it monthly, quaterly, over all products, over all subsiduries
- semi additive means you can only sum acorss some dimensions like account balance, can sum alongs accounts but not along time
- non additive means gross percentage revenue, can't sum as you don't know what is base value and hence it is told to not use percentage and all that info in facts. 


1. How would you implement Slowly Changing Dimensions (SCD Type 2 vs Type 3)? Explain the differences and when to use each.
2. Describe the differences between transient tables and temporary tables in data warehousing.
3. Explain how you'd model a “swipe payment” API in a relational schema.
4. What’s the difference between Change Data Capture (CDC) and Change Data Tracking (CDT)?
5. Compare data lake storage vs blob storage. When would you use one over the other?
6. Walk me through how you'd design an end-to-end ETL pipeline from an on-premise database to Azure Databricks.
7. Design an optimal schema to store event logs (like clicks/swipes) for high-velocity web traffic.
8. Given messy sales data, walk us through cleaning, transformation, and how you'd design reporting tables for business analytics.
9.  Kafka: Backpressure, design challenges
- Database internals: Indexing, partitioning, connection pooling
- - Follow-up: Design a streaming system end-to-end
  - You need to join two large datasets, but the join operation is causing out-of-memory errors. What strategies would you use to optimize this join?
  -  Describe how you would design and implement an ETL pipeline in PySpark to extract data from an RDBMS, transform it, and load it into a data warehouse.
  -  Explain the architecture of Databricks clusters. How would you optimize cluster configurations for large-scale ETL jobs?
   . Describe the internal mechanisms of Delta Lake’s transaction log. How does Delta Lake ensure ACID compliance in a distributed environment?
. Discuss advanced techniques for optimizing Spark jobs in Databricks. Include details on job scheduling, partitioning strategies, and performance tuning
Explain how to implement fine-grained access control in Databricks using Azure Active Directory or AWS IAM roles.
 Describe how you would integrate Databricks with an external data warehouse like Snowflake or BigQuery. What are the challenges and best practices?
 How would you design a system to dynamically scale Databricks clusters based on workload demands? Discuss the use of auto-scaling and job prioritization.
 . Explain the process of sharing data and notebooks across different Databricks workspaces. What are the security implications, and how would you mitigate risks?
What's the best strategy to handle schema evolution when reading streaming Delta tables?
How would you implement a time-travel audit system using Delta Lake versioning?
 Given 100 TB of zipped JSON files, how do you read, flatten, transform, and write efficiently in Databricks?
Design a hybrid pipeline to process 20M events/day (real-time + batch).
How would you optimize costs while maintaining SLA on a cluster that runs 24x7?
 How to maintain exactly-once guarantees in Spark Streaming or Kafka Streams?
 How to manage schema evolution when Kafka pushes new fields downstream?
AWS vs GCP → which services fit best for scalable data ingestion & analytics?
Design a global system to track millions of domain registrations in real time.
 Build a data lake for analytics with high availability → what formats & layers?
Create a recommendation engine for domain renewals like data flow & model serving plan.
How would you design a data platform for real-time trade processing across multiple regions?
Design a data model for multi-currency transactions and exchange rate adjustments.
 You’re asked to mask sensitive data before sending to analytics teams — how would you implement it?
A data ingestion job is lagging behind in Kafka — what steps would you take to fix it?
How do you plan a zero-downtime migration for a petabyte-scale warehouse?
 Compare Parquet vs Avro — explain use cases and performance trade-offs.
