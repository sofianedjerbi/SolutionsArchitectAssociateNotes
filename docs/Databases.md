# Databases

## RDS Supported backends

- Postgres
- MySQL
- Oracle
- MariaDB
- Microsoft SQL Server
- IBM DB2
- Aurora

## RDS Management by AWS

You **CAN'T SSH** into RDS but you can:
- Automated provisioning
- **OS Patching**
- **Backups** / point in time restore
- **Monitoring Dashboards**
- **Multi-AZ setup** for disaster recovery
- Maintenance windows for upgrades
- Scaling capability (horizontal & vertical)
- Storage backed by EBS

## RDS Storage Auto Scaling

**Increase storage on RDS dynamically.**  
You have to **set a max storage threshord**. (you cannot scale forever)
Automatically modify storage if:
  - Free storage is less than 10%
  - Low storage lasts at least 5 minutes
  - 6 hours have passed since last modification


## RDS Read Replicas

Up to 15 read replicas, **within AZ, cross AZ or cross region**.  
**Replication is asynchronous**.  Replicas can be promoted to a standalone DB.  
The application must **update the connection list to list all read replicas.**  
Cross AZ replication is free but cross region is charged.

*Use-case: Reduce workload for read only applications (reporting apps...)*


## RDS Multi AZ (Disaster Recovery)

Synchronous replication **only as a failover** in another AZ for availability.  
**Read replicas can also be used as Multi AZ** for Disaster Recovery.

## From single AZ to multi AZ

It is a zero downtime operation.

## RDS Custom

Managed **Oracle** and **Microsoft SQL Server Database** only with OS and database customization.  
You have access to the OS with **SSH**, SSM...

## Aurora Precisions

**Compatible with Postgres and MySQL.**  
It is AWS optimized and claims a 5x improvement.  
Storage **grows automatically in increments of 10GB**.  
Replication and failover are faster and native.  
It costs 20% more than RDS.

## Aurora High Availibility and Scaling

It stores **6 copies accross 3 AZ**.  
  - 4 out of 6 for writes
  - 3 out of 6 for reads
  - Self healing with P2P replication.
  - Storage stripped accross 100s of volumes.

There is a **master** instance that takes writes, with failovers.  
Master + up to 15 read replicas for reads.  

**Backtrack** allow you to restore data at any poing of time without backups.

## Aurora structure

The **writer endpoint** points to the master.  
The **reader endpoint** load balances *(horizontal / vertical)* to the read replicas that are autoscaling.  
You can add **custom endpoints** for certains queries. 

## Aurora Global Database

Feature that allow **replicas in 5 other read only regions** with **up to 16 read replicas per region**.  
Cross region replication takes less than 1 sec.

## Aurora Machine Learning

Add **ML-based predictions via SQL**.  
It is an integrattion between Aurora and AWS ML services (SageMaker & Comprehend).  
You don't need ML experience.  
*Use case: Fraud detection, ads targeting, setiment analysis, product recommandations...*

## RDS Backups

- **Daily full backup** (can be disabled)
- **Transaction logs are backup-up every 5 minutes** *(ability to restore from oldest backup to 5 min ago)*
- **Manual DB Snapshots**

*Trick: Stopped RDS database will create charges for storage.*    
*If you wanna stop it for a long time, you should snapshot it & restore it later*

## Aurora Backups

- **Automated continuous backups** *(ability to restore from any point in time up to 35 days)*
- **Manual DB Snapshots**

## Recovery

- Restore a snapshot **into a new database**.
- Restore a database **from S3**. *(backup on premises -> S3 -> New MySQL RDS)*
- Restore a **MySQL Aurora cluster from S3**. (Backup on premises with Percona XtraBackup -> S3 -> New Aurora)

## Aurora Database Cloning

Ability to clone a Aurora DB Cluster, it uses **copy-on-write protocol**.  
It is very fast and cost-effective. Used for dev.

## RDS & Aurora Security

Audit **logs can be enabled and sent to CloudWatch Logs**.

### Encryption

- **At-rest encryption** uses KMS, defined at launch time. Master need to be encrypted for read replicas.
- **In-flight encryption:** TLS-ready by default, client-side should use AWS TLS root certificates.

To encrypt an unecrypted database, you need to take a snapshot ans restore as encrypted.

### Authentication

- **IAM Authentication** (except for Oracle)
- **Security groups**
- **No SSH** *(except custom)*

## RDS Proxy

Fully managed, highly available **private** database proxy.  
It reduces **failover time**.  
**It allow us to enforce IAM Authetication for DB**.  
**Pool & share connections, reduce stess... For Lambda functions as example.*

## ElastiCache precisions

You need to **change your application** to query the cache before querying the DB.  
*(Cache miss -> Read DB -> Write to cache)*  
**Cache must have an invalidation strategy** to ensure only must used data is there.  

This enables caching of user data for retrieval across different app instances, thus supporting **stateless apps**.

## Redis vs Memcached

### Redis

- **Multi-AZ** with auto failover.
- **Read replicas** to scale reads and have high availability.
- **Data Durability** using AOF persistence.
- **Backup and restore features**
- Supports **sets and sorted sets**

## Memcached

- **Multi-node** for partitioning of data
- **No high availability**
- **No persistence**
- **No backup and restore**
- Multithreaded architecture


## ElastiCache Security

- Supports **IAM Authentication for Redis**.

### Redis

- Redis Authentication *(password / token)*
- SSL in-flight encryption.

### Memcached

- Supports SASL-based encryption.

## ElastiCache use cases

- **Lazy Loading:** All the read data is cached & stale in cache
- **Write Through:** Adds or update data in cache when written to a DB (no stale data)
- **Session Store:** Store temporary session data in cache.

### Redis Use Case

- Gaming Leaderboards: **Redis Sorted Sets**

## Ports

### Important

- FTP: 21
- SSH / SFTP: 22
- HTTP: 80
- HTTPS: 443

### RDS

- PostgreSQL: 5432
- MySQL / MariaDB: 3306
- Oracle: 1521
- MSSQL: 1433

## DocumentDB Precisions

Aurora **MongoDB semi-implementation in AWS**.  
It grows in **increments of 10GB**.  
Fully managed with replication across 3 AZ.

## Neptune Precisions

Fully managed **graph** database.  
It has replication accross 3 AZs.

## Keyspaces (for Apache Cassandra)

A managed **Apache Cassandra**-compatible db service.  
Replicated across 3 AZ.

## Timestream Precisions

Has built-in time series **analytics**.  
You can export to Grafana, SageMaker...


