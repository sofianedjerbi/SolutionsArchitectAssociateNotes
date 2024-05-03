# Disaster Recovery

## Type of Disaster Recovery

- **On-Premises => On-Premises:** Expensive
- **On-Premises => AWS:** Hybrid recovery
- **AWS Region A => AWS Region B:** Cloud recovery

### Terms
**RPO:** Recovery point objective: The latest state save that we can recover to.  
**RTO:** Recovery time objective: The time we are going to be back online

### Backup and Restore

High RPO/RTO but cheap: **We backup at a certain frequency** then we load a **backup** when there's a disaster.

*EBS snapshots, S3 saves, regular pushed to S3...*

### Pilot Light

A **small version of the app** is **always running in the cloud**.  
We switch whenever we need. Pretty expensive, but low RTO / RPO.

### Warm Standby

Full system is **up and running** but at **minimum size**.  
We scale when needed. More costly but lower RPOs and RTOs.

### Multi-Site Approach (Hybrid)

Setup the app in multiple sites in an **active/active setup**.
Almost no downtimes.  
Might have very expensive costs related to network, infrastructure...

*Direct Connect, Site to Site VPN...**

### AWS Multi Region

Deploy in **multiple AWS regions**.  
Medium cost and very low RTO / RPO.

*RDS Replication, Route53, CloudFront*

## Database Migration Service (DMS) Precisions

It needs an **EC2 instance**, it will pull the data **continuously**.  
It supports **self healing** and **multi-AZ deployment**, with a standby replica.  

Source can be **on-premises**.  
Target can be OpenSearch, Kinesis DataStream...

For heterogenous migrations, you need **Schema Conversion Tool** *(SCT)*.  

## RDS MySQL to Aurora MySQL

Use **DMS** is both databases are up and running.

### Internal
**Option 1:** Take a DB **Snapshot** and restore it in Aurora. Cause downtime.  
**Option 2:** Create an Aurora Read Replica from the RDS and promote it as its own DB cluster.

### External
**Option 1:** Use Percona XtraBackup, load into S3 and create an Aurora MySQL from S3
**Option 2:** Create an Aurora MySQL DB and use mysqldump to migrate into Aurora

It is the same with PostgreSQL

## On-Premise Strategy with AWS

You can **download Amazon Linux 2 AMI as an .iso**.  
You can **Import/Export VMs**.  
There is DMS and Server Migration Service (SMS), Application Discovery Service.

## AWS Backup Precisions

**Supports:** EC2 / EBS / S3 / RDS / DynamoDB / DocumentDB / Neptune / EFS / FSx / Storage Gateway and more.  
It supports **cross-region** and **cross-account** backups. PITR is also supported if the service supports it.

You can create backup policies **Backup Plans**. Works with tags.  
*Backup frequency, backup window, Transition to cold storage, retention period*

## Backup Vault

Store **undeletable backups**.

## Application Discovery Service Precisions

Two types of discovery:  
- **Agentless Discovery (Connector):** VMs, Contiainers, Configuration, Performance, Disk Usage.
- **Agent-based Discovery (Application Discovery Agent):** System performance analysis, running processes, network usage.

Results can be viewed **within Migration Hub**

## Application Migration Service (MGN)

**Rehost** *(lift-and-shift)* solution which simplify **migrating applications** to AWS.  
The replication agent is doing a **continuous replication into the cloud**.  
When everything is replicated, you can switch on the cloud.

## Large amount of data into AWS

**Huge data:** Snow Family
**On-going replication:** Site to site VPN, or DX with DMS or DataSync.

## VMWare Cloud on AWS

It is a tool to manage on-premises data center.  
It allow to keep **VMware cloud software with AWS compatibility**.
