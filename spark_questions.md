References:
- https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations


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
