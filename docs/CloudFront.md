# CloudFront

## S3 Origin Access Control (OAC) 

**Restricts access to an S3 bucket's content** to requests **from specific CloudFront distributions**.

## Origins

- **S3 Bucket:** S3 bucket policy + origin access control
- **Custon Origin (HTTP):** Can be EC2, ALB...

## CloudFront vs S3 Cross Region Replication

Cross region replication is **read only**, only on a **few regions**.  
CloudFront is **global**, but files are cached for a TTL.  

Use **CloudFront** for static content that should be accessible **everywhere**.  
Use **S3 Cross Region Replication** for content that should be available **at low latency in a few regions**.

## Geo Restriction
 
**Allowlist** for a list of allowed countries.  
**Blocklist** for a list of blocked countries.  
They cannot be enabled together.

## Pricing

Cost of **data out per edge location** varies.

## Price Classes

- **Price Class All**: All regions
- **Price Class 200**: Most regions, but excludes the most expensive regions.
- **Price Class 100**: Only the least expensive regions.

## Cache Invalidations

You can trigger a cache invalidation with an **object name** or a **prefix**.  
*Use case: update your content*

## Unicast vs Anycast

**Unicast:** Traditional IP association.  
**Anycast:** Two or more servers with the same IP, user will be sent to the closest one.

## Global Accelerator Precisions

It creates **2 anycast IPs**.  
They send traffic directly to edge locations.  
The edge locations send the traffic to your application.

It also offer **health checks** and **DDoS protection** with Shield.  
Endpoints are ALB, NLB, EC2 and Elastic IP.

*Important: Good for HTTP use cases that require static IP addresses, or fast regional failover.*

