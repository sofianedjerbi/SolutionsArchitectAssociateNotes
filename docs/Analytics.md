# Analytics

## Athena Precisions

A **serverless SQL** tool to analyze in **S3 buckets** and store the result in **S3**.  
Supports CSV, JSON, ORCm Avro, Parquet.  
You pay **5$ per TB of data scanned**.  
Commonly used with QuickSight for dashboards.

### Cost / Performance Improvements
- Use optimized columnar data with Parquet or ORC.  
- Compress data.
- Partition data in S3 for easy querying. *(path: year/month/day)*
- Use bigger files.

### Federated Query

**Run SQL queries across data stored in DBs** (AWS or On-Premises)  
It uses Lambda functions. You can store the result **in S3**.

## Redshift

It's an **OLAP SQL** database that can scale to **PBs** of data.  
Dashboard tools integrate with it. It has **faster joins / queries / aggregations**.

### Cluster

Composed of a **leader node** and **compute nodes**.  
You can use **reserved instances** for cost saving.

### Snapshots & Disaster Recovery (DR)

Redshift has **multi-az** mode and snapshots that are **PITR** backups stored in S3.  
Automated and manual snapshots are available. You can copy snapshots to another region.

### Import Data

You can import data from Firehose, S3 buckets and EC2 instances.  

### Redshift Spectrum

With an **available Redshift cluster**, you can query data in S3 **without loading it in a database**.

### Enhanced VPC Routing

Routes Redshift traffic through your VPC.

## OpenSearch (ElasticSearch)

You can search **any field in another database**.  
Available in managed / serverless modes.  
Does not support SQL by default but can support it with a plugin.  
**It still requires data to be loaded inside.**  
You can load logs, analyze in real time.

*DynamoDB -> DynamoDB Stream -> Lambda -> OpenSearch*  
*Then we can query data in OpenSearch and load the full data with DynamoDB*

## EMR Precisions

Create clusters of **hundreds of EC2 instances**. *(Spark, HBase, Presto...)*  
Master/Primary node, core node and task node. Task nodes aren't required to be persistent.  
There are **on-demand and reserved offers**. You can use **spot instances for task nodes**.

## QuickSight Precisions

Create **interactive dashboards**. Per-session pricing.  
Integrated with AWS services and on-premises databases / files data.  
The **SPICE** engine is an in-memory computation engine if data is imported into QuickSight.

You can share dashboards with users and groups.

## Glue Precisions

It is a **serverless ETL** service (Extract, Transform and Load).  
*S3 CSV -> EventBridge -> Glue Parquet conversion -> Send into S3 for Athena analysis*  

### Glue Data catalog

With a **crawler** it reads data in multiple AWS databases.  
It writes metadata in a **centralized catalog**, then Glue Jobs, Athena, EMR, Redshift, can use this data later.

### Glue Job Bookmarks

Prevent re-processing old data

### Glue Elastic Views

Combine and replicate data across multiple data stores with SQL to provide an **unified view of all the data**.

### Glue DataBrew

Clean and normalize data.

### Glue Studio

GUI to create run and monitor Glue ETL jobs.

### Glue Streaming ETL

Run ETL as streaming jobs. Compatible with Kafka, Kinesis Data Streaming...

## Lake Formation

Help create **data lakes** and automate complex transformations.  
It has **blueprints** to import data from almost anywhere.  
Provides fine-gained **access control**. Built on top of Glue.  
Used of analytics.

It also **centralizes security**.

## Kinesis Data Analytics

Allow to create real time analytics.  
You pay for actual consumption rate.

### For SQL Applications

Sources are Kinesis **firehose or data streams** or S3.  
You can send data to a Data Stream, or Firehose.

### For Apache Flink

Use Flink (Java, Scala, SWL) to process and analywe streaming data.

## Managed Streaming for Apache Kafka (MSK)

**Alternative to Kinesis**, a fully managed Apache Kafka cluster on AWS.  
Multi AZ setup and EBS storage without expiration. Available in **serverless** mode.  
Provides more customisation than Kinesis, bigger data.

*(MSK Consumers = Glue, Lambda, Data Analytics)*
