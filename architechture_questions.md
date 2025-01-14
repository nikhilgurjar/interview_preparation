### what are SCD and its types
   SCD -> slowly changing dimensions 
  they are used to cater things which changes from time to time
  SCD0 -> never changes when inserted like date of joining in employee table
  SCD1 -> address in customer table
        if you directly update address, you wont have historical data like where he was earlier
        another case if there is some analysis and report done on number of peples in a state, then if we change it directly and someone want to devise jan report again he cant as he will get latest data always
        so basically in SCD1 no history is tracked

  SCD2 
  history will be retained
  we can add 3 columns status, start date, end date
  end date should not be kept null as it can create problems in between and join conditions/merge conditions

  SCD3 used in salesforce condition
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
multiple databases are kept at datawarehouse
datawarehouse used to store large volume of data while database stores small volume of data
datawarehouse is used for read heavy operations while database for write heavy operations
datawarehouse has high latency while database has low
datawarehouse data is denormalized while in databricks its highly normalized
datawarehouse follows column storage while database follow row based
prallel processing of request in supported in datawarehouse buts in databricks 

OLAP - online analytical processing uses datawarehouse (terradata, snowflake)
OLTP - online transactional processing uses database (mysql, oracle, postgress)

if we save parquet in hdfs, this is also columnar and we do processing by spark this is also parallel processing why do we not use it 
spark can handle semi and unstructured data as well while data warehouse works on structured data most of time 
datawarehouse do not support streaming but spark does

spark runs on commodity servers


### Fact table and dimensional table
fact -> measuremenet
dimensional table -> context
consider a sales data where there is product info, customer info, order info etc
Now fact table is table which just uses references of all above and create relationships
while product, customer, order is dimensions table 

star schema is using of fact and dimensions as fact is in between and there are multiple dimension tables around it 
dimensions table info changes rarely while fact table info changes frequently

Now lets say customer table is dimension table and now this customer table can have few other dimension tables 
its called snowflake schema 


Natural key has business associated with it it can be primary key as well it can not be 
natural key will be larger in size and may create memory issues
surrogate key is factless key which do not change if there is any change in data

### Type of facts in ETL
1. additive 2. non additive 3. semi additive
   additive means you can add over all dimensions like gross revenue you can sum along any axis be it monthly, quaterly, over all products, over all subsiduries
   semi additive means you can only sum acorss some dimensions like account balance, can sum alongs accounts but not along time
   non additive means gross percentage revenue, can't sum as you don't know what is base value and hence it is told to not use percentage and all that info in facts. 

  
