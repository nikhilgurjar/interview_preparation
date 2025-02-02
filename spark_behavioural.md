### You are working with a large dataset stored in a data lake. How would you decide on a partitioning strategy to optimize query performance?
1. Check query patterns
   A. Frequent Filters and Predicates
      checks columns most used in filters, group by etc
     time based partitioning is common and hence can use range partitioning or based on time column

2. Column cardianility
   Low cardianility: Columns with a small set of distinct values (such as region, country, or status flags) are usually good candidates for partitioning because they result in a limited number of larger partitions. Larger partitions help reduce the overhead of managing many small files and minimize metadata burdens.
   High cardianility: Avoid partitioning on columns with a very high number of unique values (like user IDs or full timestamp fields). Partitioning on such columns can lead to an explosion of tiny partitions, increasing the file system’s overhead and slowing down query planning.

   **if you’re dealing with event data, you might partition by date and then by a low-cardinality attribute like event type, rather than by a high-cardinality attribute such as a session ID.
  **
   Aim to have each partition hold at least around 1 GB of data. This guideline helps avoid the “small files problem,” which can lead to excessive metadata overhead and inefficient query execution.
   use Data Skipping and Clustering techniques like zorder, liquid clustering

ensure that your processing framework (for example, Delta Lake’s auto-partitioning on append) can automatically detect and create new partitions. This dynamic handling of partitions is crucial for managing incremental data loads efficiently.
While more partitions can increase parallelism, too many small partitions can burden the query planner and driver with metadata management. Strive for a balance
