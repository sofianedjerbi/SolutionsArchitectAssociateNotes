# EC2

## IPs

Instance IP change on restart.

- **IPv4 / IPv6**
- **Private IP:** Local IP accessible by servers within the network. Unique within the network.
- **Public IP:** Public IP accessible by the internet. Globally unique.
- **Elastic IP:** Fixed public IPv4, used to mask instance dynamic ip when failing. 
  - **Should be avoided.**
  - **Prefer a DNS or a load balancer.**

## Placement Groups

Placement withing the AWS infrastructure.

- **Cluster:** 
  - **No specific limit on the number of instances**
  - Group instances within a single AZ.
  - High-performance apps.

- **Spread:** 
  - **Limit: 7 running instances per group per AZ.**
  - Physically spread between hardware.
  - Critical apps.

- **Partition:** Spread accross partitions.
  - **Limit: Up to 7 partitions per AZ**
  - Partitions should be isolated in term of failure.
  - Partitions do not share the same physical rack.
  - HDFS, HBase, Cassandra, Kafka
  - Apps that need a lot of instances and high availability. 

## ENI: Elastic Network Interface

**Virtual network card.** Instances comes with an ENI.

Can have:

- A primary **Private IPv4**, and other secondary IPv4s
- **One Elastic IPv4** per private IPv4
- **One public IPv4**
- One or more security groups
- A MAC address
- ...

You can **create and move ENI on the fly**.  
Only available **within an AZ**.  
*(Usages: Quick failover by attaching to a working instance)*  
*(Like an ethernet cable. You can move it, you cannot attach it to 2 devices.)*  
*(They are the bridge that link an instance to a subnet)*  

[I strongly recommend to read this](https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/)

## EC2 Hibernate

**The in-memory state is preserved.**  
RAM is dumped into an **EBS** only, then reloaded later.  
EBS should have **enough space** and **be encrypted**.  
Thanks to this, the **booting process is much faster**.

Not supported for bare metal instances.  
**You cannot hibernate more than 60 days.**
