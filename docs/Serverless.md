# Serverless

## Lambda Supported Languages

**Python, Node.js, Java, C#, Go, Ruby, Cystom Runtime API (Rust..), Lambda Container Image with Lambda Runtime API.**

## Lambda limits

**15 Minutes, up to 10GB in /tmp, up to 10GB for any kind of memory.**  
1000 Concurrent executions (can be increased).  
**4KB of env variables, 50MB in compressed size, 250MB of uncompressed deployment.**

## Lambda SnapStart

Up to **10x performance** with no extra cost on **Java 11+**.  
Avoid calling **JVM initialization phase** by saving the initialized state.

## CloudFront Functions & Lambda@Edge

**Edge functions** for low latency serverless functions.  
- **CloudFront Functions** are in **JavaScript**. Used to change **viewer requests and responses**.
- **Lambda@Edge** in **NodeJS / Python**. Used to change **CloudFront requests and responses**.  

CloudFront Functions are lighweight functions *< 1ms*, Lambda has better margins *up to 5 secs*.   
*(CloudFront Functions Use-Cases: Cache Key normalization, Header Manipulation, Redirects...)*  
*(Lambda@Edge Use-Cases: HTTP Body customization)*

## Lambda in VPC

**Lambda is launched outside the VPC by default.**  
You can define the VPC ID, the subnets and the security groups to launch in VPC.  
Lambda will create an ENI in subnets. 

By default, **Lambda don't have access to the internet**. It need a NAT Gateway.

## Lambda with RDS Proxy

To avoid opening **too many connections, you can use an RDS Proxy** that will pool the connections.  
Lambda functions must be created **in the VPC**.

## Lambda from RDS & Aurora

Lambda will react to data events *(insert..)*.  
Should be setup from within the database by **connecting to it**.  
For **RDS for PostgreSQL and Aurora MySQL**.  
DB must have the same functions.  

*(Note: RDS Event Notifications does NOT offer access to data or data information)*

## DynamoDB Precisions

DynamoDB is **highly available across multiple AZs** and **integrated with IAM**.  
It is **always available**. It comes with **Standard and Infrequent Access** table classes.  
The maximum size of an **item is 400KB**.  
There are two modes: **provision mode and on-demand mode**.  
**WCU and RCU are decoupled.**

## DynamoDB Stream Processing

A stream of **item-level modifications** in a table *(create / update / delete)*.  
**DynamoDB Streams** for basic usage or **Kinesis Data Streams** for more customization.

## DynamoDB Global Tables

A **two way** replication. An **active/active setup** that requires **DynamoDB Streams**.

## DynamoDB TTL

Automatically **delete items after some time**.

## DynamoDB Backups

You can have **on-demand backups** or **point-in-time recovery (PITR)**.

## DynamoDB and S3

With **PITR** you can export to S3 in **JSON or ION** format.  
You can also import from S3 *(CSV format supported)*.

## API Gateway

Allow us to create **Rest APIs** that will redirect requests to AWS services / any HTTP server.  
It handle **keys, Swagger, OpenAPI, caching...**

## API Gateway Endpoint Types

- **Edge Optimized:** For global clients with edge locations.
- **Regional:** For clients in the same region.
- **Private:** Within a VPC with an ENI.

*Note: The API Gateway lives in one region no matter the endpoint type.*

## API Gateway security 

You can use **IAM Roles, Cognito, a Custom Authorizer**.  
It also brings **HTTPS security** with **ACM**.

## Step Functions Precisions

They can integrate with **EC2, ECS, On-Premises servers, API Gateway, SQS...**  
*Use-case: Order fulfillment, data processing...*

## Cognito User Pools

Provides a **sign-in** functionality for app users.  
Integrate with **API Gateway and ALP**.  
It creates a **serverless database of app users**.  
Supports **Password reset, MFA, Email & Phone verification, Federated Identities...**  

## Cognity Identity Pools

Provide **temporary AWS credentials** so they can access **AWS resources directly**.  
Integrate with **User Pools** as an identity provider.  
**Users source can be User Pools, 3rd party login...**  
**IAM Policies applied are defined in Cognito** and can be customized based on user_id.  
You can define a **default IAM role**.

You can use Identity Pools for **low level security**.



