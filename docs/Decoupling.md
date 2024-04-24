# Decoupling

## SQS Precision

**Unlimited number of messages** in queue, **< 10 ms latency**, 256kb per message limit.  
Warning: Can have **duplicate messages** or messages delivered **in the wrong order**.

**Producer:** AWS SDK...
**Consumers:** Lambda, EC2, servers...

## SQS Security

### Encryption

**In-flight** with **HTTPS** API, at-rest with **KMS**, client-side possible.

### Access

**IAM** for access to the API.  
**SQS Access Policies** allowing other services to write / cross account access to SQS.

## SQS Message Visibility Timeout

When a message it polled, it becomes **invisible to other consumers**.  
Default = 30 secs = **The message has 30 secs to be processed**.  
There's the **ChangeMessageVisibility** API to get **more time**.

## SQS Long Polling 

The consumer just **wait** for messages between 1 to 20 sec.  
**Decreases the number of API calls** made to SQS while increasing the efficiency and latency of the application.  
Can be enabled at **API level with WaitTimeSeconds**.

## SQS FIFO

**First In First Out ordered SQS queue**.  
Limited throughput: 300 msgs / sec without batching, 3000 msgs with.  
**Deduplication protection using a **Deduplication ID**.
*Queue name should end with .fifo*

## SQS + ASGs

We can **increase ASG capacity** based on **SQS queue length** with **CloudWatch Metric + Alarm**.  
Metric is **ApproximateNumberOfMessages**.  

For DBs, if the load is too big, some messages may be **lost**.  
To solve this, we can use **SQS as a buffer to database writes**.  
With an **enqueue ASG** and a **dequeue ASG**.  
**We delete from the queue only if it's written**  
*ReceiveMessage -> Insert In DB*

## SNS Precision

**A topic is a channel** you can subscribe to.  
Up to 100k topics, 12.5M subscriptions per topic.

## SNS Security

**Same as SQS.**

## SNS + SQS: Fan Out Pattern

**Push once in SNS**, receive in all SQS queues that are subscribers.  
**Cross-Region delivery is possible**.

*Use-case: Send an event that is issued a single time (S3 Event) to multiple queues.*

You can send SNS to S3 through Kinesis Data Firehose.

## SNS FIFO

Ordering by **Group ID** (all messages in the same group are ordered).  
**Deduplication protection using a **Deduplication ID**.

## Message Filtering

A **JSON Policy** to filter messages sent to SNS.  
If a **subscription doesn't have a filter policy, it receives every message**.

## Kinesis Data Streams Precision

Data streams **can scale the number of shards**.  
Retention is from 1 day to 365 days.  
You can **reprocess** data. Inserted data **can't be deleted**.

### Producers

Application (Kinesis Client Library / AWS SDK), AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics.

### Consumers

Data Firehose, API, SDK, Agent, CLI.

### Shard

Determined ahead of time.  
They determine the **capacity** of the data stream.  
It is a single **FIFO queue**.

### Record

Is made with a **partition key** (shard destination) and a **data blob** (content, up to 1MB).  
The **consumer record also has a sequence number**. *(ordered UUIDs)*

### Modes

- **Provisioned mode:** 
  - Choose the number of shards, scale manually or with API.
  - Each shard gets 1Mb/s in (or 1000 records per sec)
  - Each shard gets 2Mb/s out (classic or enhanced fan-out consumer)
  - Pay per shard provisioned per hour
- **On-demand mode:**
  - No need to provision or manage the capacity
  - Default capacity provisioned (4 Mb/s or 4000 records/s)
  - Scales automatically based on the observed throughput peak during the last 30 days
  - Pay per stream per hour & data in/out per GB

### Security

**Same as SNS & SQS.**

## Kinesis Data Firehose Precision

You pay for data **going through** Firehose.  
Near real-time processing.  
Supports **conversion (custom with Lambda), transformations, compression**.  

### Producers

Kinesis Data Streams, CloudWatch Logs & Events, AWS IoT, Kinesis Agent, SDK, CLI.

### Batch write destination

**S3, Redshift, OpenSearch, Partner Destinations, Custom HTTP Endpoint**.  
We can send failed / all data to a S3 Bucket.

## Stream vs Firehose

**Stream:** Ingest data at scale, real-time, replay, custom producer / consumer. Manage Scaling.   
**Firehose:** Near real-time, store processed data. Automatic scaling. Buffer system. No replay.

## Kinesis vs SQS FIFO: Data Ordering

Kinesis **partition** data, whereas SQS **you can use the Group ID to partition data**.  
Kinesis can only have **the same number of shards and consumer**, **SQS is not limited**.  
Kinesis can receive 5MB/s and SQS 300 messages (3000 with batching)

## SQS vs SNS vs Kinesis

**SQS:** Consumer pull and delete data, unlimited consumers, no need to provision throughput, FIFO mode.  
**SNS:** Push data to many subscribers, data is not persisted. Pub/Sub, no need to provision throughput.  
**Kinesis:** Shard system, possibility to replay data, real time big data analytics, ordering at shard level.  

## Amazon MQ

Queue but backed by **RabbitMQ** or **ActiveMQ**, for on-premises app transition.  
Has both a **queue feature** and a **topic feature**.  
Supports standard protocols: MQTT, AMQP, STOMP...  

For high availability you have a **standby** MQ Broker, with an **EFS**, used to store data.

##

