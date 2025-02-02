References:
- https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations
- https://www.linkedin.com/posts/surbhi-walecha_data-engineering-interview-question-ugcPost-7242756667094773760-jERn?utm_source=share&utm_medium=member_desktop

### What is apache spark?
Apache Spark is an open-source, distributed processing system used for big data workloads. It utilizes in-memory caching, and optimized query execution for fast analytic queries against data of any size. It provides development APIs in Java, Scala, Python and R, and supports code reuse across multiple workloads—batch processing, interactive queries, real-time analytics, machine learning, and graph processing.
### What are best optimization techniques for apache spark?

### Difference between apache spark and map reduce?
https://arismuhandisin.medium.com/mapreduce-vs-spark-choosing-the-right-framework-for-your-big-data-needs-2b01e3a51dcd
### what are spark lazy operations and which trigger operations?
https://medium.com/@john_tringham/spark-concepts-simplified-lazy-evaluation-d398891e0568
https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations

### Diamond problem in java and spark? (inheritance problem) and how does it scala helps us to rectify it?
https://medium.com/@shrutibce/diamond-problem-solution-in-scala-b9af7f54a32b
### Yield keyword in scala
### Optimizations technichques in hive? what are they?
### Optimizations technichques in hive? what is bucketing, clustering?
### When to you row based file format and when to use column based file format?
### Where we should use partitioning and where to use bucketing?
### What config you used for your hardest project?
### What is liquid clustering?
### Hardest thing you faced and how did you resolved?

### How to choose cluster size based on input data
https://medium.com/analytics-vidhya/simple-method-to-choose-number-of-partitions-in-spark-75315c636a94
default partition size = 128MB
number of partititions = input_data/128MB
number of cores == number of partitions or number of partitons = multiple of number of cores


### Sparksession vs sparkcontext vs hivecontext vs sqlcontext?
Sparksession .builder() is a design pattern which gives us interface to access sparkcontext, hivecontext, sqlcontext via single entry point
https://medium.com/@rganesh0203/sparkcontext-vs-othercontexts-98e54895197e
<code>
   SparkSession.builder().appName("test_app").master("local").config("spark.sql.warehouse.dir", "C:/users/nikhil/spark").enableHiveSupport().getOrCreate()
   
</code>

1. **What is AQE**  
   Adaptive Query Execution (AQE) is a feature introduced in Apache Spark 3.0 that optimizes Spark SQL queries dynamically at runtime. AQE performs three main optimizations:  
   1. Coalescing Shuffle Partitions: AQE can combine small partitions during a shuffle to reduce the overhead of scheduling and executing many small tasks. For example, if a DataFrame with 250MB of data is shuffled into 200 partitions, but only 5 partitions are filled with data, AQE can coalesce these 5 partitions into fewer partitions to ensure all tasks complete at the same time.
   2. Switching Join Strategies: AQE can dynamically switch the join strategy based on the size of the shuffled data. For instance, if two tables A (8GB) and B (5GB) are to be joined, but after applying filters, A has 5GB and B has only 50MB, AQE can change the join strategy to a broadcast join at runtime. This switch happens after shuffling, so the shuffle operation cannot be avoided.
   3. Optimizing Skew Joins: AQE can handle skewed data by dividing the skewed partition into smaller partitions and replicating the corresponding partition from the other side of the join. For example, if a join operation between two tables (10GB and 20GB) results in a skewed partition (e.g., 8GB for 'sugar'), AQE can split this skewed partition into smaller partitions to avoid an OutOfMemory (OOM) error.  

   The rules for splitting a skewed partition are:  
      - The size of the skewed data is more than 5 times the median partition size.
      - The size of the skewed data is greater than 256MB.
   
   In summary, AQE is a powerful feature in Spark 3.0 that can significantly improve the performance of Spark SQL queries by dynamically adjusting the query plan based on runtime statistics.

2. Difference between spark and hadoop
      Hadoop and Spark are both big data frameworks, but they serve different purposes and have different strengths. Here's a detailed summary of the points you mentioned:
      
      1. **Hadoop is not a database**: Hadoop is a distributed data processing framework. It uses the Hadoop Distributed File System (HDFS) for storage and MapReduce for processing. Hive, a component of Hadoop, provides a SQL-like interface to query data stored in HDFS, but it's not a traditional relational database. The data is stored in files, and Hive maintains metadata about the structure of these files.
      
      2. **Spark's speed compared to Hadoop**: It's often said that Spark is 100 times faster than Hadoop. This is not always true and depends on the specific use case. Spark can be faster than Hadoop's MapReduce because it performs computations in-memory and can do processing in a single stage, while MapReduce writes intermediate data back to disk, which can be slower. However, for very large datasets that don't fit into memory, Spark may need to spill to disk, reducing its speed advantage.
      
      3. **Data processing in RAM**: Both Hadoop and Spark can process data in RAM. Hadoop's MapReduce writes intermediate results to disk, while Spark tries to keep as much data in memory as possible for faster access. However, Spark can also spill to disk if the data doesn't fit into memory.
      
      4. **Batch vs. Streaming Processing**: Hadoop is primarily designed for batch processing. It's excellent for tasks where data can be processed in parallel, and time isn't a critical factor. Spark, on the other hand, supports both batch and streaming processing. It can process data in real-time, making it suitable for tasks where low latency is required.
      
      5. **Resource Managers**: Spark can run on various cluster managers like Hadoop YARN, Apache Mesos, Kubernetes, or its standalone cluster manager. This flexibility allows Spark to be deployed in various environments, including on-premises and cloud platforms like AWS and GCP.
      
      In addition to these points, here are a few more that might come up in an interview:
      
      - **Fault Tolerance**: Both Hadoop and Spark are fault-tolerant. Hadoop achieves fault tolerance through data replication in HDFS, while Spark achieves it through a data abstraction called Resilient Distributed Datasets (RDDs).
      
      - **Ease of Use**: Spark provides high-level APIs in Java, Scala, Python, and R, and an interactive shell in Scala and Python. This makes it easier to develop and run applications compared to Hadoop.
      
      - **Data Sources**: Spark can easily integrate with various data sources like HDFS, NoSQL databases, relational databases, and data warehouses. It also supports a variety of file formats like CSV, JSON, Parquet, and others.
      
      - **Machine Learning and Graph Processing**: Spark includes MLlib for machine learning and GraphX for graph processing, making it a versatile tool for a wide range of use cases.

Apache Spark provides three ways to manipulate data: SQL, DataFrames, and Datasets. Regardless of the method you choose, your code goes through the Catalyst Optimizer, which is essentially the Spark SQL engine. This engine transforms your code into Java bytecode, which can be executed on the JVM.

The Spark SQL engine operates in four phases:

1. **Analysis**: In this phase, Spark verifies the syntax of your code and checks if the tables and columns you're referring to actually exist. If there's an issue, it throws an AnalysisException. This process involves converting an unresolved logical plan (which might have unknown attributes) into a resolved logical plan.

2. **Logical Planning**: The engine generates a logical plan for the query. This plan represents a series of transformations that produce the result, but it doesn't specify how to perform those transformations.

3. **Physical Planning**: The logical plan is converted into one or more physical plans. These plans describe how to execute the query on the cluster. The engine chooses the most efficient plan based on cost estimation.

4. **Code Generation**: The chosen physical plan is then compiled into Java bytecode to run on the JVM.

The Resilient Distributed Dataset (RDD) is a fundamental data structure of Spark. It's an immutable distributed collection of objects. Each dataset in an RDD is divided into logical partitions, which may be computed on different nodes of the cluster.

The term "Resilient" means that RDDs can recover from node failures. "Distributed" means that data is distributed across multiple nodes in a cluster. "Dataset" is your actual data.

RDDs support two types of operations: transformations, which create a new RDD from an existing one, and actions, which return a value to the driver program after running a computation on the dataset.

The main disadvantage of RDDs is that they don't take advantage of Spark's optimization. This is because when you're working with RDDs, you're working with the data at a low level.

On the other hand, RDDs are great for unstructured data and give you more control when you need to perform complex, low-level transformations and actions.

DataFrames, on the other hand, are designed for processing large collections of structured or semi-structured data. They're easier to use than RDDs and offer better optimization for queries.

In summary, while RDDs provide a powerful and flexible API, they're often harder to work with and lack the optimizations provided by DataFrames and Datasets. Therefore, it's generally recommended to use DataFrames and Datasets for big data processing tasks.


**Difference b/w cache and persist**:

         `df.cache()` and `df.persist()` are methods in PySpark used to persist the DataFrame in memory and speed up subsequent computations. 
         
         - `df.cache()`: This method is a shorthand for `df.persist(StorageLevel.MEMORY_AND_DISK)`. It persists the DataFrame with a default storage level (MEMORY_AND_DISK). This means that the DataFrame will be stored in memory if there is enough space, and will spill to disk if the memory is full.
         
         - `df.persist()`: This method is more flexible as it allows you to specify a custom storage level. You can choose to persist the data in serialized form, on disk only, on memory only, or a combination of these.
         
         Storage levels in PySpark:
         
         - MEMORY_ONLY: Store the data in deserialized form in JVM memory. If the size of data is bigger than the memory capacity, it will not store the excess data and recompute it every time it's needed. This level provides the fastest data access but can be memory-consuming.
         
         - MEMORY_AND_DISK: Store the data in deserialized form in JVM memory. If the size of data is bigger than the memory capacity, it will store the excess data on disk. This level provides a good balance between memory utilization and computation speed.
         
         - MEMORY_ONLY_SER (Memory Only Serialized): Similar to MEMORY_ONLY, but the data is stored in serialized form. This level is more space-efficient than MEMORY_ONLY but requires more CPU time because data needs to be deserialized before computation.
         
         - MEMORY_AND_DISK_SER (Memory and Disk Serialized): Similar to MEMORY_AND_DISK, but the data is stored in serialized form. This level is more space-efficient than MEMORY_AND_DISK but requires more CPU time because data needs to be deserialized before computation.
         
         - DISK_ONLY: Store all the data on disk. This level is the slowest but it's the most space-efficient.
         
         When you use `.show()`, it only shows the first few rows of the DataFrame, not just one partition. The number of rows displayed can be specified as an argument to the `show()` function.
         
         Remember, choosing the right storage level depends on your specific use case, the size of your data, and the resources available in your cluster.

### What is the difference between Azure Blob Storage and ADLS Gen2 Storage?
   z ordering is used to optimize range queries involving multi dimensions. Example, in a railway passenger data lets say, if you zorder by train number and boarding location, it will try to colocate rows with train number and boarding locationions in a same path. 
   repartition by range however runs on multiple columns but sorts on 1 column only. 
z order tries to remove overlap of given columns. 
you can run Deltalake.forpath().executeZorder()

### What is the difference between Azure Blob Storage and ADLS Gen2 Storage?

**ADLS Gen2 = Azure Blob Storage + ADLS Gen1**

#### Structure
- **Blob Storage**: Flat namespace object store.
- **ADLS Gen2**: Hierarchical namespaces (much like a File System).

#### Purpose
- **Blob Storage**: General purpose object store for a wide variety of storage scenarios, including big data analytics.
- **ADLS Gen2**: Optimized storage for big data analytics workloads.

#### Performance (Analytics Workload)
- **Blob Storage**: Good storage retrieval performance.
- **ADLS Gen2**: Better storage retrieval performance.

**Explanation**:
- **Hierarchical namespaces** in ADLS Gen2 organize blob data into directories and store metadata about each directory and the files within it. This organization yields better storage and retrieval performance for analytical use cases and lowers the cost of analysis. Operations such as directory renames and deletes can be performed in a single atomic operation.
- **Flat namespaces** in Blob Storage require several operations proportionate to the number of objects in the structure.

**Cost-
 -** Blob: High cost for Analysis.
 - **ADLS: Low cost for Analysis.

### What is the difference between map and flatMap transformations? Provide use cases for each?
   val array1d = Array ("1,2,3", "4,5,6", "7,8,9")   <br/>
   //array1d is an array of strings <br/>
   
   val array2d = array1d.map(x => x.split(",")) <br/>
   //array2d will be : Array( Array(1,2,3), Array(4,5,6), Array(7,8,9) ) <br/>
   
   val flatArray = array1d.flatMap(x => x.split(",")) <br/>
   //flatArray will be : Array (1,2,3,4,5,6,7,8,9) <br/>
   You want to use a flatMap when,
   your map function results in creating multi layered structures 
   but all you want is a simple - flat - one dimensional structure, by removing ALL the internal groupings <br/>
   
### How do you implement a custom partitioner in PySpark, and what are the use cases?

#### Types of Partitions in PySpark
1. **Hash Partitioning**: 
   - Assigns a unique hash value to each record based on a specified column and places the record in the corresponding partition.
   - **Use Case**: Useful when you need to evenly distribute data across partitions based on a key.

2. **Range Partitioning**:
   - Uses `repartitionByRange()`. Example: `df = df.repartitionByRange(3, "age")`
   - **Use Case**: Ideal for range queries where data is partitioned based on a range of values.

3. **Custom Partitioner using `partitionBy`**:
   - You can create a custom partitioner by extending `pyspark.Partitioner` and implementing your partitioning logic.
   - **Use Case**: When you need a specific partitioning strategy that is not covered by default partitioners.

### Can you explain a scenario where hash partitioning might not be ideal?
Hash partitioning might not be ideal in scenarios where:
- **Skewed Data Distribution**: If the data is not uniformly distributed, hash partitioning can lead to some partitions being overloaded while others are underutilized. This can cause performance bottlenecks.
- **Range Queries**: For queries that need to access a range of values, hash partitioning is inefficient because it scatters related data across multiple partitions, leading to increased read times.

### How does range partitioning improve query performance for range-based queries?
Range partitioning improves query performance for range-based queries by:
- **Localized Data Access**: It organizes data into contiguous ranges, so queries that access a specific range of values can quickly locate and retrieve the relevant data.
- **Reduced I/O**: Since related data is stored together, fewer partitions need to be scanned, reducing the amount of I/O operations and improving query speed.
- **Efficient Indexing**: Range partitioning allows for more efficient indexing and searching within each partition, further speeding up range-based queries.

### What are accumulators and broadcast variables? How and when would you use them?
   - **Broadcast Variables - Instead of sending these variables with every task, Spark distributes them to each executor only once, thus reducing overhead. 
   - **Accumulators - Accumulators are variables used for aggregating information across all tasks in a Spark job. They provide a way to update a shared variable across tasks in parallel while providing only limited forms of communication and synchronization. 
   - Accumulators should be used in parallel processing as many parallel task will try to update accumulator which can lead to data corruption. 
   see code for it
   
### What are the potential pitfalls of using accumulators in a distributed environment?
The potential pitfalls of using accumulators include:
- **Data Corruption**: If multiple tasks try to update the accumulator simultaneously, it can lead to inconsistent or corrupted data.
- **Limited Communication**: Accumulators provide limited forms of communication and synchronization, which can be a drawback if more complex coordination is needed.
- **Delayed Updates**: The updates to accumulators are not immediately visible to the driver program, which can lead to delays in reflecting the current state of the accumulated value.

### How do broadcast variables help in reducing the communication overhead in a Spark job?
Broadcast variables help reduce communication overhead by:
- **Single Distribution**: Instead of sending the variable with every task, Spark distributes the broadcast variable to each executor only once. This reduces the amount of data transferred over the network.
- **Efficient Access**: Executors can access the broadcast variable locally, which speeds up task execution and reduces latency.
- **Consistency**: Broadcast variables ensure that all tasks use the same read-only data, maintaining consistency across the distributed environment.


### Difference between Datamart, Datawarehouse and Deltalake
![image](https://github.com/user-attachments/assets/1d4d36ed-db01-4552-8d8d-4bc19e680772)
need video

Q -> bucketing vs partioning 
https://medium.com/@ashwin_kumar_/spark-partitioning-vs-bucketing-partitionby-vs-bucketby-09c98c5b40eb
Q -> what is liquid clustering
Liquid Clustering ( abbreviated as LC in this article) automatically adjusts the data layout based on clustering keys. In contrast to a fixed data layout as in Hive-style partitioning, the flexible (“liquid”) layout dynamically adjusts to changing query patterns, addressing the problem of suboptimal partitioning, column cardinality, etc. Clustering columns can be changed without rewriting the data.

Q -> How would you design a data pipeline to process 1 TB of data daily in real-time? 
Q -> 
Q -> Compare performance of Managed and External Table
Q -> 

### Notes on z ordering
https://medium.com/@tsiciliani/liquid-clustering-with-databricks-delta-lake-57dc251d7870

### If Spark can spill the data to disk, why would it fail with the OOM – out-of-memory exception?
### Spark serialization and deserialization
   we do transformations and actions on rdd and datasets
   Now transformations are naroow and wide
   in wide transformation there is shuffling between partitions and hence between nodes
   and it can't be trasferred without serialization as internally everything is JVM objects
   serialization is process of converting java object to stream of bytes to transfer over network
   type of serialization 
- java serialization - default 
- kyro serialization - 10x faster than java serializer and is more performant however do not support for all types


  9) How do you connect ADLS Gen 2 with databricks? In where we mention the role assignments?
10) If you are using Service Principal to connect with ADLS from Azure Databricks explain the steps and how would you code it?
11) Why using service principal? How would you create it?
12) What is Databricks runtime? Why we need it?
13) What are Workflows?
14) Explain about the Medallion Architecture in brief.
15) Explain about Delta file format briefly.
16) Consider you are working in Facebook. User is writing data for each record. Since each record is been written, a new json transaction log will get created for each write. But we can use Datalake only right. Why do we require delta file format? It can decrease the performance every now and then right?
17) Consider a job is running very slow in Azure Databricks. How would you approach the issue and make it faster?
18) What are the optimization techniques you have worked on? Explain them in brief.
19) How would you optimize the job with respect to memory management in azure databricks?
### How can you identify and resolve memory bottlenecks in a PySpark
application?
https://medium.com/@saipavanguduri10/ways-to-detect-bottlenecks-in-spark-code-3129c8ce1dac

### A shuffle spill occurs when a Spark task can't process the data in memory, causing it to write data temporarily to disk. If the shuffle spill is too high (e.g., 50-60 GB), it overwhelms Spark's memory, leading to slow performance and potential crashes. This results in excessive disk I/O, which can significantly degrade performance, especially when working with Delta Lake.
Solutions to Spill issues
1. Increase Memory Allocation
2. Partitioning and Bucketing
3. Adaptive Query Engine (AQE)
4. Broadcast Join Tuning
5. Reduce Data Collection to Driver
https://selectfrom.dev/spark-performance-tuning-spill-7318363e18cb
https://medium.com/@biswas.upasana/spark-performance-tuning-spill-838c357ac935

