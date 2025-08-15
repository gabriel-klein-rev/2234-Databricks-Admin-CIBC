# Week 5 Friday - 8/15/2025

## 1. Compute Resources
**Summary**  
Compute resources in Databricks refer to the infrastructure used to execute workloads such as notebooks, jobs, pipelines, and SQL queries. This includes **clusters** (driver + worker nodes), **SQL warehouses**, and **serverless compute**.

**Key Points**
- **Clusters**: Provide the Spark runtime for distributed data processing. Can be **all-purpose**, **job clusters**, or **shared**.
- **SQL Warehouses**: Optimized compute for SQL queries and BI tools; can be **classic** or **serverless**.
- **Serverless**: Fully managed, autoscaling compute without manual configuration.
- Compute resources are billed based on type, size, and runtime.

**Implementation Steps**
1. Choose compute type based on workload (ETL → job cluster, analytics → SQL warehouse).
2. Configure runtime version and node types for performance needs.
3. Set autoscaling and idle termination to optimize cost.
4. Apply permissions to control who can attach to or manage compute.

---

## 2. Performance Tuning for Clusters/Jobs
**Summary**  
Performance tuning optimizes execution time, resource utilization, and cost for Databricks workloads.

**Best Practices**
- **Cluster sizing**: Match node type and size to data volume and workload complexity.
- **Autoscaling**: Set reasonable min/max workers to handle peak loads without overprovisioning.
- **Runtime version**: Use the latest LTS Databricks Runtime for performance and stability improvements.
- **Caching**: Use `.cache()` or `.persist()` on DataFrames reused multiple times.
- **Shuffle optimization**: Use `spark.sql.shuffle.partitions` tuned to your cluster size.
- **Broadcast joins**: For small lookup tables, use `broadcast()` to avoid large shuffles.

**Implementation Steps**
1. Profile job with Spark UI to identify bottlenecks (e.g., shuffles, skew, serialization).
2. Adjust Spark configurations:
   ```python
   spark.conf.set("spark.sql.shuffle.partitions", "200")
   ```
3. Apply partition pruning and predicate pushdown for queries.
4. Optimize Delta tables with Z-Ordering and VACUUM:
   ```sql
   OPTIMIZE my_table ZORDER BY (col1);
   ```

---

## 3. Common Issues and Troubleshooting Approaches
**Summary**  
Understanding common issues helps quickly resolve operational problems in Databricks.

**Common Issues & Solutions**
- **Cluster fails to start**: Check driver node quota, node type availability, or missing permissions.
- **Out of memory**: Increase worker size or optimize data processing logic (filter early, reduce shuffles).
- **Slow jobs**: Review query plan, repartition data, use caching, and avoid wide transformations where possible.
- **Job/task failures**: Check run logs and Spark UI; handle transient errors with retries.
- **Delta table conflicts**: Resolve write conflicts by enabling optimistic concurrency control and ensuring unique write operations.

**Troubleshooting Steps**
1. Review **driver and executor logs** from the cluster UI.
2. Inspect Spark UI for task and stage-level metrics.
3. Check **event logs** and **audit logs** for environmental or permission-related issues.
4. For Delta Lake issues, use `DESCRIBE HISTORY` to identify problematic operations.

---

## 4. Cluster Cost Optimization
**Summary**  
Cost optimization ensures workloads run efficiently without over-provisioning resources.

**Best Practices**
- **Autoscaling**: Enable and tune min/max worker limits.
- **Idle termination**: Set a short termination time for inactive clusters.
- **Job clusters**: Use ephemeral clusters for scheduled jobs instead of long-running all-purpose clusters.
- **Spot instances**: Where supported, use preemptible/spot nodes for non-critical workloads.
- **Data optimizations**: Use partition pruning, caching, and Delta optimizations to reduce compute load.

**Implementation Steps**
1. Set idle termination in cluster settings (e.g., 10–20 min).
2. Use job clusters for ETL and data processing tasks.
3. Monitor usage with **Cluster Utilization Reports** in the admin console.
4. Regularly review workloads for optimization opportunities.
