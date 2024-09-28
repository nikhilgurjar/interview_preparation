References:
- https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations
- https://www.linkedin.com/posts/surbhi-walecha_data-engineering-interview-question-ugcPost-7242756667094773760-jERn?utm_source=share&utm_medium=member_desktop

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

Q -> What is Z ordering
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
   

Q -> How do you implement a custom partitioner in PySpark, and what are the use cases?
Q -> What are accumulators and broadcast variables? How and when would you use them?
Q -> How would you design a data pipeline to process 1 TB of data daily in real-time? 
Q -> Difference between Datamart, Datawarehouse and Deltalake
Q -> Compare performance of Managed and External Table
Q -> 
