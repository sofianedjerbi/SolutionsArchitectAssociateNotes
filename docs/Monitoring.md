# Monitoring

## CloudWatch Metrics

Works for **every** AWS service.  
Metrics belong to **namespaces** (per service).  
Metrics have **timestamps**.  
You can create **CloudWatch Custom Metrics** (RAM)

### CloudWatch Metrics Streams

Continually stream metrics to a destination with **near-real-time** delivery.  
*Ex: Firehose to S3, Redshift, TimeStream*

## CloudWatch Logs

- **Log Groups:** Arbitrary name, usually representing an application
- **Log Stream:** Inside a group, logs within application

Logs are **encrypted** by default, and you can define an expiration policy.  
Logs can be sent to **S3 (Export), Kinesis Stream / Firehose, Lambda, OpenSearch**.

Sources are:
- **SDK, CloudWatch Log Agent, CloudWatch Unified Agent**  
- **Elastic Beanstalk:** Collection of logs from an application
- **ECS:** From containers
- **Lambda:** From function logs
- **VPC Flow Logs:** VPC Specific logs
- **API Gateway**
- **CloudTrail**
- **Route53**

## CloudWatch Logs Insights

**Query logs** with a special language and get **visualizations**.  
You can **export** the results or add them to a **dashboard**.  
You can query logs in multiple groups and AWS accounts.

- **CloudWatch Container Insights** for **ECS, EKS, Fargate** with a containerized version of the **agent**.  
- **CloudWatch Lambda Insights** for data in **Lambda**. *(CPU, RAM, Net...)*
- **CloudWatch Contributor Insights** to find the "top-N" contributors, analyze traffic, contributions, usage...  
- **CloudWatch Application Insights** to show potential problems with monitored applications. Uses SageMaker.

## CloudWatch Logs Subscriptions

Get real-time log events from CloudWatch logs for processing and analysis.  
You can **filter logs**.

This allows to **aggregate** logs from different accounts and regions into **a single destination**.  
You must use a **Subscription Destination**.

## CloudWatch Logs for EC2

You need an **agent:** Log Agent (Old) / Unified Agent (New)  
**Available Metrics:** CPU, Disk, RAM, Net, Processes, Swap...

## CloudWatch Alarms

We can associate **triggers** to metrics.  
**Alarm Action** are used to scale EC2 Instances, Terminate/Reboot/Recover EC2, and for SNS.  
**Composite alarms** are alarms based on **multiple metrics**. *(reduce alarm noise)*

## CloudWatch Events / EventBridge

You can **schedule jobs** or trigger functions **on events**.  
Events can be **filtered**. **Partner and custom** events are available.  
Events can be **archived** and **replayed**. Handy for debugging.  
*Events: Start instance, failed build, upload in S3, api calls, CRON...*  

**Event Buses** can receive event vrom different sources.

EventBridge can analyze the events in your bus and infer the **schema**.  
The **Schema Registry** allows you to generate code bindings that know the **data structure**.  
Schemas can be versioned.

EventBridge has **resource based policy** that manage permissions for a specific Event Bus.  
*(Allow events from another account...)*

*Pipeline Example: User delete table, Call logged in CloudTrail, Event sent in EventBridge, Trigger SNS*

## CloudTrail Precisions

It logs API calls from any agent: **AWS Services, Users, CLI, SDK**.  
Kind of events, stored for 90 days:
- **Management Events:** Operations performed on the AWS accounts *(Read/Write events)*
- **Data Events:** Not logged by default. Low level activity (get object, lambda invoke)
- **Insights Events:** Detect unusual activity in the account. It parse write events to detect patterns.  

You can send them to S3 and use Athena to analyze them.


## AWS Config Precisions

You can have **config rules**, custom or AWS. *(each instance should be t2.micro, each EBS should be gp2)*  
They do not prevent, but you can show the compliance, and use **Config Remediations** to take action.  

Can be used to **trigger events**, send configuration changes and compliance state.  
All this information is stored into an **S3 bucket**.

**Config Notifications** can send notifications, email on changes.

*(Ex: Send a message when a certificate is about to expire)*
