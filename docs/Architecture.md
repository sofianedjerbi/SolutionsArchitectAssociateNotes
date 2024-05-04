# Architecture

## Golden AMI

An AMI **with everything installed**. *(for fast deployment)*

## Elastic Beanstalk Components

To deploy en Beanstalk you should enter the following parameters:
- **Application**
- **Application version**
- **Environment** *(AWS resources)*

### Tier

Available Beanstalk tiers: 
- **Web Server Tier:** Traditionnal distribution across web instances.  
- **Worker Tier:** Multiple instances pulling from an SQS Queue.

Web Server Tier can push messages into the SQS queue of the worker environment.

### Deployment modes

Beanstalk deployment modes:
- **Single Instance:** Great for dev. Uses Elastic IP.
- **High Availability with Load Balancer**: Basic ELB / ASG pattern

## Event Processing

**SQS => Lambda** (+ DLQ after N tries or 1st try if FIFO)  
**SNS => Lambda + Internal try => DLQ on SQS** 

Fan out pattern: **Deliver to multiple SQS Queues with SNS**.  

Remember you can use **EventBridge** for more **destinations** and **better filters**.  
You can also use it with **CloudTrial** to **intercept API Calls**.

## Caching Strategies

**Edge:** CloudFront with TTS
**AWS Level:** API Gateway cache
**App Level:** Redis / DAX / Memcached

## HPC on AWS

- **Data Transfer:** Direct Connect, Snow Family, DataSync  
- **Compute:** CPU/GPU optimized instances, auto scaling  
- **Network:**  
  - Cluster (same rack / AZ)
  - EC2 Enhanced Networking with Elastic Network Adapter, ENA *(can go up to 100 Gbps)*
  - Elastic Fabric Adapter (EFA), an improved ENA for Linux HPC  
- **Storage:** 
  - Attached: EBS, Interface Store
  - Network: S3, EFS, FSx for Lustre
- **Automation:** Batch jobs, AWS ParallelCluster to deploy HPC clusters.

