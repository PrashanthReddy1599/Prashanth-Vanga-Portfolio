# Cloud Data Warehouse Optimization

## 📄 Project Overview
Led a comprehensive data warehouse optimization initiative redesigning schemas, implementing advanced indexing strategies, and optimizing queries across Snowflake and BigQuery platforms. The project delivered significant improvements in query performance, cost efficiency, and data modeling for analytics workloads.

## 🔧 Tech Stack
- **Data Warehouses:** Google BigQuery, Snowflake
- **Data Processing:** Google Cloud Dataflow, Google Cloud Dataproc
- **Transformation & Modeling:** dbt (Data Build Tool), Python, SQL
- **Orchestration:** Apache Airflow, Google Cloud Composer
- **Monitoring & Observability:** Google Cloud Stackdriver, Snowflake Query History & Performance Monitoring
- **Languages:** SQL, Python

## 🎯 Objectives
- Improve query performance across 500+ tables and views
- Reduce cloud infrastructure costs through intelligent resource utilization
- Redesign data models using dimensional modeling best practices
- Implement partitioning, clustering, and indexing strategies
- Enable faster reporting and analytics for business stakeholders

## 🛠️ Technologies & Tools

### Data Warehouses
- **Snowflake** - Cloud data warehouse with automatic scaling and compute separation
- **BigQuery** - Google Cloud serverless data warehouse with built-in ML capabilities

### Data Processing & ETL
- **Google Cloud Dataflow** - Serverless stream and batch data processing
- **Google Cloud Dataproc** - Managed Spark and Hadoop service for big data processing
- **Python** - Custom data processing and automation scripts

### Data Transformation & Modeling
- **dbt (Data Build Tool)** - SQL-based data transformation framework
- **SQL** - Complex queries, optimization, and performance tuning

### Orchestration
- **Apache Airflow** - Workflow orchestration for ETL/ELT pipelines
- **Google Cloud Composer** - Managed Airflow service on GCP

### Monitoring & Optimization
- **Google Cloud Stackdriver** - Performance monitoring, logging, and alerting
- **Snowflake Query History** - Query performance analysis and optimization insights

## 📊 Architecture Improvements

### Before Optimization
```
[Raw Data Sources]
         ↓
   [Basic ETL]
         ↓
[Flat Tables] (Minimal indexing)
         ↓
[Slow Queries] (4-6 minute execution)
```

### After Optimization
```
[Raw Data Sources]
         ↓
[Optimized ELT Pipeline]
  (Dataflow + Composer)
         ↓
[Star Schema Design]
    - Dimension Tables (SCD Type 2)
    - Fact Tables (Partitioned)
         ↓
[Clustering & Indexing Layer]
  (BigQuery: Partitioning/Clustering)
  (Snowflake: Clustering Keys)
         ↓
[Fast Queries] (<2 minute execution)
```

## 🚀 Key Optimization Strategies

### 1. Dimensional Modeling (Star Schema)
- Created fact and dimension tables separating business metrics from attributes
- Implemented Slowly Changing Dimensions (SCD Type 2) for historical tracking
- Denormalized dimension tables for query performance improvements
- Reduced complex joins by up to 60% through proper star schema design

### 2. Partitioning & Clustering

**BigQuery Optimizations:**
- Partitioned large tables by date (e.g., transaction_date, created_date)
- Applied clustering on high-cardinality columns (customer_id, product_id)
- Reduced data scanned per query by 70% using partitioning

**Snowflake Optimizations:**
- Implemented clustering keys on frequently filtered columns
- Leveraged automatic micro-partitions for optimal data organization
- Applied zone maps for metadata-based query pruning

### 3. Query Optimization
- Analyzed and rewrote 200+ complex queries removing redundant operations
- Implemented materialized views for frequently accessed aggregations
- Used query result caching in both Snowflake and BigQuery
- Applied query hints and optimization techniques for complex joins

### 4. Storage Optimization
- Implemented data lifecycle policies using Google Cloud Storage archival tiers
- Leveraged Snowflake Time Travel for data recovery (7-day retention)
- Applied compression techniques reducing storage costs by 35%
- Archived historical data older than 3 years to cold storage

### 5. Cost Management
- Implemented Snowflake resource monitors and warehouse auto-suspend
- Used BigQuery slot reservations for predictable pricing
- Set up cost allocation labels and budget alerts in Google Cloud
- Optimized compute resources based on workload patterns

## 📈 Implementation Process

### Phase 1: Analysis & Assessment
1. Profiled query performance using Snowflake Query History and BigQuery Query Plans
2. Identified top 50 slowest and most frequently run queries
3. Analyzed table structures, data volumes, and access patterns
4. Documented current performance baselines and pain points

### Phase 2: Design & Planning
1. Designed star schema with 15 dimension tables and 8 fact tables
2. Created partitioning and clustering strategy for each platform
3. Developed migration plan with minimal downtime requirements
4. Established testing framework using dbt for data validation

### Phase 3: Implementation
1. Built new dimensional models using dbt transformations
2. Implemented partitioning and clustering on BigQuery tables
3. Applied clustering keys on Snowflake tables
4. Migrated ETL pipelines to use Google Cloud Composer and Dataflow
5. Created optimized views and materialized views for common queries

### Phase 4: Testing & Validation
1. Ran parallel systems ensuring data consistency between old and new schemas
2. Conducted performance benchmarking comparing query execution times
3. Validated data accuracy using dbt tests and custom Python scripts
4. Obtained stakeholder sign-off on report accuracy and performance

### Phase 5: Deployment & Monitoring
1. Executed cutover to new optimized data models
2. Implemented monitoring dashboards using Google Cloud Stackdriver
3. Set up automated alerts for query performance degradation
4. Conducted knowledge transfer sessions with analytics teams

## 🎯 Results & Impact

### Performance Improvements
- **Query Execution Time:** Reduced average query time from 4-6 minutes to < 2 minutes (67% improvement)
- **Dashboard Load Time:** Improved BI dashboard refresh from 10 minutes to 3 minutes (70% faster)
- **Data Processing:** Accelerated ETL pipeline execution by 50%
- **Concurrent Users:** Supported 3x more concurrent users without performance degradation

### Cost Savings
- **Infrastructure Costs:** Reduced monthly cloud costs by 40% ($15,000/month savings)
- **Storage Optimization:** Decreased storage footprint by 35% through compression and archival
- **Compute Efficiency:** Optimized Snowflake warehouse usage reducing compute costs by 45%
- **BigQuery Slot Usage:** Reduced on-demand query costs by 60% through partitioning

### Business Value
- Enabled real-time analytics for executive decision-making
- Reduced analyst wait time for report generation by 70%
- Improved data quality and consistency across reporting systems
- Enhanced user satisfaction scores from 6.2 to 8.9 out of 10

## 🔍 Technical Highlights

### BigQuery Specific Optimizations
```sql
-- Partitioned and Clustered Table Example
CREATE TABLE `project.dataset.sales_fact`
PARTITION BY DATE(transaction_date)
CLUSTER BY customer_id, product_category
AS SELECT * FROM source_table;

-- Materialized View for Aggregations
CREATE MATERIALIZED VIEW `project.dataset.daily_sales_summary`
AS
SELECT 
  DATE(transaction_date) as sale_date,
  product_category,
  SUM(revenue) as total_revenue,
  COUNT(DISTINCT customer_id) as unique_customers
FROM `project.dataset.sales_fact`
GROUP BY 1, 2;
```

### Snowflake Specific Optimizations
```sql
-- Clustered Table with Automatic Reclustering
ALTER TABLE sales_fact 
CLUSTER BY (transaction_date, customer_id);

-- Resource Monitor for Cost Control
CREATE RESOURCE MONITOR data_warehouse_monitor
WITH CREDIT_QUOTA = 1000
  TRIGGERS
    ON 75 PERCENT DO NOTIFY
    ON 90 PERCENT DO SUSPEND
    ON 100 PERCENT DO SUSPEND_IMMEDIATE;

-- Zero-Copy Cloning for Testing
CREATE TABLE sales_fact_test 
CLONE sales_fact;
```

### dbt Transformation Example
```sql
-- models/marts/sales/fact_daily_sales.sql
{{ config(
    materialized='incremental',
    unique_key='sale_id',
    cluster_by=['transaction_date', 'customer_id']
) }}

SELECT 
  s.sale_id,
  s.transaction_date,
  c.customer_key,
  p.product_key,
  s.quantity,
  s.revenue,
  s.profit
FROM {{ source('raw', 'sales') }} s
LEFT JOIN {{ ref('dim_customer') }} c ON s.customer_id = c.customer_id
LEFT JOIN {{ ref('dim_product') }} p ON s.product_id = p.product_id
{% if is_incremental() %}
WHERE s.transaction_date > (SELECT MAX(transaction_date) FROM {{ this }})
{% endif %}
```

## 📚 Key Learnings

1. **Platform Selection Matters:** BigQuery excels at ad-hoc analytics with automatic scaling, while Snowflake provides better control over compute resources and cost predictability
2. **Dimensional Modeling is Crucial:** Proper star schema design reduced query complexity and improved performance more than any indexing strategy
3. **Partitioning Strategy:** Date-based partitioning provided the highest ROI for time-series data
4. **Monitoring is Essential:** Proactive monitoring prevented query performance regressions and caught issues early
5. **Incremental Migration:** Parallel systems during migration ensured data accuracy and reduced risk

## 🔄 Ongoing Optimization

- Weekly review of query performance using Stackdriver and Snowflake dashboards
- Monthly cost analysis and resource optimization reviews
- Quarterly schema refinement based on new business requirements
- Continuous dbt model development for emerging analytical needs
- Regular training sessions for analysts on query optimization best practices

## 📊 Metrics Dashboard

**Performance KPIs tracked:**
- Average query execution time (daily)
- 95th percentile query latency (hourly)
- Dashboard load times (real-time)
- ETL pipeline success rates (per run)

**Cost KPIs tracked:**
- BigQuery slot usage and on-demand query costs (daily)
- Snowflake credit consumption by warehouse (hourly)
- Storage costs across hot/warm/cold tiers (weekly)
- Cost per query and cost per dashboard (monthly)

---

**Project Duration:** 6 months  
**Team Size:** 3 data engineers, 2 analytics engineers, 1 solution architect  
**Impact:** $180,000 annual cost savings, 67% query performance improvement
