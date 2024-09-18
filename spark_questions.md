1. **What is AQE**  
   Adaptive Query Execution (AQE) is a feature introduced in Apache Spark 3.0 that optimizes Spark SQL queries dynamically at runtime. AQE performs three main optimizations:  
   1. Coalescing Shuffle Partitions: AQE can combine small partitions during a shuffle to reduce the overhead of scheduling and executing many small tasks. For example, if a DataFrame with 250MB of data is shuffled into 200 partitions, but only 5 partitions are filled with data, AQE can coalesce these 5 partitions into fewer partitions to ensure all tasks complete at the same time.
   2. Switching Join Strategies: AQE can dynamically switch the join strategy based on the size of the shuffled data. For instance, if two tables A (8GB) and B (5GB) are to be joined, but after applying filters, A has 5GB and B has only 50MB, AQE can change the join strategy to a broadcast join at runtime. This switch happens after shuffling, so the shuffle operation cannot be avoided.
   3. Optimizing Skew Joins: AQE can handle skewed data by dividing the skewed partition into smaller partitions and replicating the corresponding partition from the other side of the join. For example, if a join operation between two tables (10GB and 20GB) results in a skewed partition (e.g., 8GB for 'sugar'), AQE can split this skewed partition into smaller partitions to avoid an OutOfMemory (OOM) error.  

   The rules for splitting a skewed partition are:  
      - The size of the skewed data is more than 5 times the median partition size.
      - The size of the skewed data is greater than 256MB.
   
   In summary, AQE is a powerful feature in Spark 3.0 that can significantly improve the performance of Spark SQL queries by dynamically adjusting the query plan based on runtime statistics.
