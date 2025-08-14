# Week 5 Thursday - 8/14/2025

## 1. Clusters 
**Summary**  
Clusters are the compute resources in Databricks used to run jobs, notebooks, and other workloads. They consist of a **driver node** and one or more **worker nodes**.

**Key Points**
- **Shared Clusters**: Multiple users share compute; good for collaborative work. Permissions control who can attach notebooks.
- **Personal Clusters**: Dedicated to a single user; ideal for sandbox development.
- **Job Clusters**: Ephemeral clusters that are created automatically for a specific job run and terminated when the job finishes.
- **Worker Nodes**: Machines that run the workload tasks; size and count determine performance.
- **Autoscaling**: Automatically adjusts worker count between a configured min/max based on workload.

**Implementation Steps**
1. In the workspace, navigate to **Compute → Create Cluster**.
2. Choose **cluster type** (all-purpose vs. job cluster).
3. Select **Shared** or **Personal** in advanced options.
4. Configure worker type and min/max workers for autoscaling.
5. Set permissions (who can attach/detach notebooks).
6. Start and monitor cluster performance.

---

## 2. Databricks SQL Warehouses 
**Summary**  
SQL Warehouses provide compute for running SQL queries and BI dashboards. They are optimized for SQL workloads and can be **classic** or **serverless**.

**Key Points**
- **Classic SQL Warehouse**: Runs on customer-managed compute; you configure scaling and sizing.
- **Serverless SQL Warehouse**: Fully managed by Databricks; instant start, auto-scaling, and optimized resource allocation without manual cluster management.
- **Billing**: Based on Databricks SQL Compute Units (DBCU) per hour.

**Implementation Steps**
1. Go to **SQL → SQL Warehouses → Create SQL Warehouse**.
2. Select **Serverless** if available (reduces startup time and maintenance).
3. Configure scaling: set min and max clusters for concurrency.
4. Assign permissions for query execution and table access.
5. Connect BI tools using JDBC/ODBC with warehouse credentials.

---

## 3. Monitoring and Logging
**Summary**  
Monitoring and logging are essential for performance optimization, troubleshooting, and auditing.

**Key Points**
- **Cluster Metrics**: View CPU, memory, and storage utilization in the cluster UI.
- **Spark UI**: Detailed job, stage, and task metrics for performance tuning.
- **Driver/Worker Logs**: Access stdout, stderr, and event logs for debugging.
- **Audit Logs**: Capture workspace and Unity Catalog activity for compliance.

**Implementation Steps**
1. Enable **Audit Logs** in Account Console (requires cloud storage location).
2. Access cluster logs: **Compute → Cluster → View Logs**.
3. Use Spark UI for job-level insights: **Cluster → Spark UI**.
4. For external monitoring, integrate with tools like Azure Monitor, AWS CloudWatch, or Prometheus.
5. Regularly review logs for errors and performance bottlenecks.

---

## 4. Orchestration Resources
**Summary**  
Orchestration resources automate workflows and data pipelines, managing dependencies and scheduling.

**Key Points**
- **Jobs**: Databricks-native orchestration for running notebooks, JARs, or Python scripts on a schedule.
- **Tasks**: Steps within a job; can have dependencies for DAG-style orchestration.
- **Workflows**: Combine Databricks jobs with external orchestrators like Airflow, Azure Data Factory, or AWS Step Functions.

**Implementation Steps**
1. Create a job: **Workflows → Create Job**.
2. Define tasks: specify notebook/script, cluster, and parameters.
3. Set dependencies between tasks for proper execution order.
4. Schedule job frequency (cron, periodic, or triggered).
5. Optionally, connect to external orchestrators via APIs or connectors.
