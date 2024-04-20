# EC2 Storage

## EBS Delete on Termination

Used to **delete an EBS volume when an instance is terminated.**

## EBS Backups

A more **heavy / secure way of saving an EBS**.   
It is **not done incrementally**.  
Used for **long-term retention** whereas snapshots are for **quick recovery**.

## EBS Snapshot Features

Note: Snapshot captures data **incrementally**.

### Archives

You can move snapshots to an **archive tier**.  
It takes within **24 to 72 hours** for restoring the archive.

### Recycle Bin

**Retain deleted snapshots**, retention from **1 day to 1 year**.

### FSR: Fast Snapshot Restore

Force full initialization of the snapshot to have **no latency on the first use** *(expensive)*

## AMI Precisions

AMIs also take **EBS snapshots**.  
Instances should be **shutdown to keep data integrity**.

## EBS Volume Types

**Characteristics:** Size | Throughput | IOPS  
Only **gp2/gp3 & io1/io2** can be used as boot volumes.   
**Price range:** sc1 < st1 < gp2 < gp3 < io1 < io2

## GP2 / GP3: Generale Purpose SSD

Cost effective **SSD storage**, low latency.  
**Use case:** Boot volumes, virtual desktops...  
**1 GiB to 16TiB**

### GP2

Can burst IOPS to 3000.  
**Size of the volume and IOPS are linked**, max IOPS is 16000.  
3 IOPS / GP = At 5334 GB we are at max IOPS.

### GP3

Baseline of **3000 IOPS** and throughput of **125MiB/s**.  
Can **increase IOPS and throughput independently** up to 16000 and 1000MiB/s.

## IO1 / IO2: Provisioned IOPS (PIOPS) SSD

**Performant SSD** storages, lower latency.  
**Use case:** Apps that need more than 16000 IOPS, critical business apps, databases...
Supports **EBS Multi-attach**.

### IO1

**4GiB to 16TiB**  
Can increase PIOPS independently from storage size.  
*Max PIOPS: 64000 for Nitro EC2 & 32000 for others EC2 instances.*   

### IO2

**4GiB to 64TiB**  
**Sub-millisecond latency**.  
*Max PIOPS:256000 with an IOPS:GiB ration of 1000:1.*

## HDD Drives

**125GiB to 16TiB**
**Cannot be boot** volumes.

### ST1: Throughput Optimized HDD

**Use-case:** Big Data, Data warehouses, Log processing...  
*Max throughput 500 MiB/s - Max IOPS 500*

### SC1: Cold HDD

**Use-case:** Infrequently accessed data, when cost is important...  
*Max throughput: 250 MiB/s - Max IOPS 250*

## Multi-Attach

Only for **IO IO2 volumes**.  
You can attach the volume to **up to 16 instances in the same AZ**.  
**Use case:** Higher app availability  
App must manage concurrent write applications.  
Use with a [clustered FS](https://en.wikipedia.org/wiki/Clustered_file_system)

## Encrypted EBS volumes

Data at rest & in flight **is encrypted inside the volume**, handled by EBS behind the scenes.  
Minimal impact on latency.  
Uses keys from **KMS** (AES-256).  
The snapshot will be encrypted too.

### How to encrypt an unecrypted volume

- 1/ Take a snapshot *(will be unecrypted)*
- 2/ Encrypt the snapshot by doing a copy *(Shortcut available to create new encrypted volume)*
- 3/ Create a new EBS volume from the snapshot
- 4/ Attach the encrypted volume to the instance

## EFS Precisions

**More expensive** than EBS.  
**POSIX Filesystem**, **scales automatically** (only pay for what you use).  
Uses NFSv4.1 protocol.  
Encryption at rest with KMS.

### Scale

**About 1000s concurrent** clients. 10+ GB throughput.  
Grow to Petabyte-scale NFS. Automatically.

### Modes

- **Performance Mode** (Defined at creation time)
  - **General purpose:** Low latency use cases
  - **Max IO:** Higher latency, throughput, highly parallel

- **Throughput Mode**
  - **Bursting:** Correlated with size, 1TB = 50MiB/s + burst to 100MiB/s
  - **Provisioned:** Uncorrelated throughput from storage.
  - **Elastic:** Automatically scales throughput based on workload.

### Storage Tiers

- **Standard:** Frequently accessed files
- **Infrequent Access:** Cost to retrieve files. Lower price to store, must use a lifecycle policy.

### Availability

- **Standard:** Multi-AZ, for prod
- **One Zone:** One AZ, backup enabled by default, for dev



