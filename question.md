# Big Data

Big Data refers to large and complex datasets that require advanced techniques for storage, processing, and analysis. It often involves distributed and parallel computing frameworks.

## Distributed Computing (Hadoop / HDFS / MapReduce)

- **Hadoop**: An open-source framework for processing large datasets in a distributed manner across multiple computers.
- **HDFS (Hadoop Distributed File System)**: The storage system of Hadoop, which splits large files into smaller blocks and distributes them across a cluster.
- **MapReduce**: A programming model for parallel processing of large data across a distributed system.

## Parallel Computing (Machine Learning → Deep Learning) (AI & Data Science)

- **Parallel computing** enables multiple processors to execute tasks simultaneously, speeding up computation.
- **Machine Learning (ML) and Deep Learning (DL)** use parallel computing to train models efficiently.
- **AI & Data Science** leverage parallel processing for complex computations like image recognition, NLP, and predictive analytics.

## Spark / Hadoop Ecosystem

- **Spark**: A fast, in-memory data processing framework, improving upon Hadoop’s MapReduce.
- **RDD (Resilient Distributed Dataset)**: The core data structure of Spark, enabling distributed computing.
- **SparkSQL**: Allows querying structured data using SQL within Spark.
- **PySpark**: The Python API for Apache Spark, used for processing big data with Python.

# ETL / ELT / ETL Tools

- **ETL (Extract, Transform, Load)**: Extracts data, transforms it, and loads it into a target system like a data warehouse.
- **ELT (Extract, Load, Transform)**: Loads data first and then transforms it within the target system.
- **Example**: Extract data from a data lake (Hadoop), transform it, and import it into a data warehouse.

## Popular ETL Tools

- **Talend**
- **SSIS (SQL Server Integration Services)**
- **Oracle Data Integration**
- **Azure Data Factory**
- **Pentaho**
- **Jenkins**

# Data Structure Types

- **Structured Data** → Stored in relational databases (SQL). Example: MySQL, PostgreSQL.
- **Semi-structured Data** → Includes some structure but does not follow a strict schema. Example: JSON, XML.
- **Unstructured Data** → Data with no predefined format. Example: Videos, Images, Text documents.

## DBMS & Data Lakehouse

- **DBMS (Database Management System)**: Manages structured data (SQL databases).
- **NoSQL DBMS**: Handles semi-structured and unstructured data.
- **Data Lakehouse**: Combines the features of data lakes (raw data storage) and data warehouses (structured processing).

# Cloud-Based Data Solutions

- **Azure Synapse Analytics** → Microsoft's cloud data analytics solution.
- **Google BigQuery** → A fully managed, serverless data warehouse.
- **AWS Athena** → A query service that allows running SQL queries on S3 data.
- **Databricks** → A cloud-based big data analytics platform built on Apache Spark.

# Big Data Frameworks

Big Data processing follows this flow:

1. **DBMS** → Storage
2. **Processing (Hadoop, Spark)** → Transformation
3. **Analytics (Power BI, Tableau, Kibana, Grafana, Superset, Splunk)** → Visualization

## Common Frameworks

- **Hadoop → Spark → Big Data Visualization/Analytics**
- **Hadoop → Spark → SQL Server → Power BI**
- **ELK Stack (ElasticSearch → Logstash → Kibana)** → Used for log and real-time data analysis.

## Big Data Visualization Tools

- **Kibana** → Data visualization for Elasticsearch.
- **Grafana** → Open-source visualization for monitoring.
- **Apache Superset** → Business intelligence tool.
- **Splunk** → Enterprise log analysis and monitoring tool.

# Big Data Analytics & Machine Learning

- **Tabular Data** → Structured data in rows & columns.
- **ML/DL/AI Data** → Represented as tensors or arrays for computations.
- **Feature Engineering** → Data transformation to fit ML models.
- **Spark MLlib** → Machine learning library in Spark for large-scale processing.

# Real-time Analytics (Spark Streaming)

- **Batch Processing** → Processes data in fixed-size chunks.
- **Streaming Processing** → Processes data in real-time as it arrives.
- **Spark Streaming** → A real-time data stream processing engine.

# OLTP vs. OLAP (Data Organization in a Data Lake)

- **OLTP (Online Transaction Processing)** → Optimized for real-time transactional data.
- **OLAP (Online Analytical Processing)** → Optimized for large-scale analytical queries.

## Data Modeling Approaches

- **Star Schema** → A central fact table connected to multiple dimension tables.
- **Snowflake Schema** → Similar to star schema but with normalized dimension tables.

## Database Normalization & Denormalization

- **Normalization** → Reduces data redundancy by organizing data into multiple tables (1NF, 2NF, 3NF, BCNF, 4NF).
- **Denormalization** → Merges tables to improve query performance at the cost of redundancy.

# SQL Query Performance Optimization

If an SQL query is running slowly, consider these optimizations:

1. **Query Optimization** → Use `EXPLAIN PLAN` to check the cost of execution.
2. **Indexing** → Ensure indexes exist on frequently queried columns.
3. **Partitioning** → Divide large tables into smaller partitions for better performance.
4. **Process Monitoring** → Use `SHOW PROCESSLIST` to check bottlenecks.
5. **Server Resources** → Check CPU/RAM usage (`htop`, Task Manager).
6. **Network Latency** → Cloud-based databases may suffer from slow network speeds.
7. **Table Structure** → Ensure `PRIMARY KEY` and `FOREIGN KEY` constraints exist.
8. **Data Type Matching** → Avoid mismatched data types in joins/filters.
9. **Using `WHERE` & `LIMIT`** → Reduce output size to speed up queries.

# Indexing vs. Partitioning

- **Indexing** → Improves query search speed by creating an ordered structure.
- **Partitioning** → Divides large tables into smaller sections to enhance performance.
- **Data Partitioning vs. Data Distribution** →
  - **Partitioning** is within a single system.
  - **Distribution** spreads data across multiple systems.

# Cloud Computing

- **Cloud is not magical** → It is just open-source software optimized by third parties.
- **Cloud providers (AWS, Google, Microsoft, IBM, Oracle)** manage infrastructure so users don’t have to.
- **On-Premise vs. Cloud**:
  - **On-Premise** → You manage everything (hardware, software, maintenance).
  - **Cloud** → A provider manages the infrastructure for you.

## Major Cloud Platforms

- **Google Cloud Platform (GCP)** → [Google Cloud Console](https://console.cloud.google.com/)
- **Microsoft Azure** → [Azure Portal](https://portal.azure.com/)
