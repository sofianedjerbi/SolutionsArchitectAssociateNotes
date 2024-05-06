# Networking & VPC


## CIDR

**\<Base IP> / Subnet Mask**

- /0  <=> 0.0.0.0
- /8  <=> 1.0.0.0
- /16 <=> 1.1.0.0
- /24 <=> 1.1.1.0
- /32 <=> 1.1.1.1

*(0 = can change, 1 = cannot change)*

## Private vs Public IPs

**Private IP can only allow certain values**: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16.

## VPC Precisions

Maximum of **5 per region**. *(soft limit)*  
Min size is **/28 = 2^4 = 16 IPs**, max is **/16 = 2^16 = 65536 IPs**.  
**Only private IPv4 are allowed.**  
Your **VPC CIDR should NOT overlap with your other networks!**

## Subnet Precisions

AWS reserves **5 IP addresses** (first 4 and last 1) in each subnet. *for router, DNS, future use...*

## Internet Gateway

Gives **internet access** to a VPC.  
You need to edit **route tables** to link instances to the internet gateway.

## Route table

You can associate **many subnets** to a route table.  
Usually we split private and public subnets.

## Bastion Hosts

It is an **EC2 instance** named Bastion Host in a public subnet, with its own **Security Group** (BastionHost-SG).  
It is used to connect to **one or many private instances**. It uses SSH.  
*SSH -> Bastion Host -> SSH -> Private Instance*

*Note: Security groups must be configured correctly.*

## NAT Instance (OUTDATED !)

It is a pre-configured Amazon Linux AMI.  
**Allow private instances to connect to the internet**.  
Must be launched **in a public subnet**.  
Must disable **Source / destination check**.  
Must have an Elastic IP attached.

Route table should redirect **private instance traffic** to the NAT instance.  
It is **NOT** highly available.

## NAT Gateway

AWS Managed NAT, pay **per hour and bandwitdh**.  
Created in a specific AZ.  
Can **only be accessed from another subnets**.

You can create another gateway in another AZ for high availability.  
**No need to connect to another gateway AZ because instances are down**

## NACL & Security Groups

**Security group is stateful: Whatever is accepted IN will be accepted OUT** and vice versa.  
But the **NACL** rules are also applied to **OUT** traffic.  
NACL rules have a **priority number**. *(lower number = higher priority)*

Default NACL accept **everything in and out**.  

## Ephemeral Ports

Client connect to a **defined port** and expect a response on an **ephemeral port**. It is on client side.  
It depends on the  **OS**.

NACLs **must allow requests on the ephemeral ports** !

## VPC Peering Precisions

It is **not transitive**, you must NOT **have overlapping CIDRs**.  
You can reference a security group from another VPC in your security group rules.

## VPC Endpoints Precisions / AWS PrivateLink

Two types of endpoints:
- **Interface**: Provisions an ENI (private IP address) as an entry point. Pay per hour and per GB of data processed.
- **Gateway**: Free, only for S3 and DynamoDB, must be used as a target in a route table. *(does not use security groups)*

## AWS ClassicLink

Connect EC2-Classic EC2 instances privately to your VPC (?????)

## VPC Flow Logs Precisions

3 Levels:
- **VPC Flow Logs**
- **Subnet Flow Logs**
- **ENI Flow Logs**

Data can go to S3, CloudWatch Logs and Kinesis Data Firehose.  
Syntax:
```
version - account id - interface id - srcaddr - dstaddr - srcport - dstport - protocol - packets - start - end - action - status
```
You query VPC logs with **Athena on S3 or CloudWatch Logs Insights.**

Troubleshoot Incoming Requests:
- Inbound REJECT => NACL or SG
- Inbound ACCEPT, Outbound REJECT => NACL

Troubleshoot Outgoing Requests:
- Outbound REJECT => NACL or SG
- Outbound ACCEPT, Inbound REJECT => NACL

*Ex: Flow Logs -> CloudWatch Logs -> Contributor Insights -> Top 10 ip addresses*  
*Ex: Flow Logs -> CloudWatch Logs -> CW Alarm -> SNS*  
*Ex: Flow Logs -> S3 Bucket -> Athena -> QuickSight*

## Site-to-Site VPN Precisions

Needs two things:
- **Virtual Private Gateway (VGW):** Created and attached to the VPC
- **Customer Gateway:** On premises

VGW should be linked to the public NAT or public customer gateway on-premises.  
You need **route propagation** in the VPC for this to work.  

**ICMP** protocol should be enabled for pinging.

## VPN CloudHub

Provide secure communication **between multiple sites** with a **VPN connection**.

## Direct Connect (DX) Precisions

You need a **Direct Connect Location** with a **Direct Connect Endpoint** that will be connected with a **Private Virtual Interface**.  
A **Direct Connect Gateway allow connections in different regions** within the same account. It's like a bridge between VPCs.  

### Connection Types
- **Dedicated:** Physical ethernet port dedicated to a customer
- **Hosted Connections:** More flexible

**IT TAKES MORE THAN A MONTH TO ESTABLISH**.

### Resiliency

- **High Resiliency:** We setup multiple direct connect. One connection at multiple locations.
- **Maximum Resiliency:** Two or more connection per locations.

### Usage with VPN as a Backup

A **site-to-site VPN** can be used as a backup.

## Transit Gateway Precisions

Used for transitivity. It is regional but **can work cross-region**.  
You can define **route tables**.  
It supports **IP Multicast**.  

It can be used to increase the bandwidth of your site-to-site VPN connection with ECMP. *(Equal cost, multi-path routing)*  
It can forward **a packet to multiple paths**.  
With **multiple site-to-site VPN** into the transit gateway, you increase the throughput.

It can be used to **share direct connect between multiple accounts**.

## Traffic Mirroring

Allows you to **capture and inspect network traffic by mirroring**.  
Mirror from source ENIs to targets ENIs or NLB.

## IPv6

**Every IPv6 in AWS are public and internet-routable**.  
**IPv4 CANNOT be disabled in your VPC**. But you can enable IPv6 to operate in dual-stack mode.  

## Egress-only Internet Gateway

Used for IPv6 only.  
A **NAT Gateway + Internet Gateway** for IPv6.

## Networking Costs in AWS per GB

### General Traffic

- Traffic **IN** is **free**
- Traffic inside an **AZ is free**
- Traffic **between AZ** = **0.02$/GB** with public / **0.01$/GB** with private**
- Traffic **between Regions** = **0.02$/GB**


### S3 Traffic

*Egress traffic = To the outside*  
*Ingress traffic = From the outside = free*

- **S3 ingress**: free
- **S3 to Internet**: $0.09 per GB
- **S3 Transfer Acceleration**:
  - Faster transfer times (50 to 500% better)
  - Additional cost on top of Data Transfer Pricing: +$0.04 to $0.08 per GB
- **S3 to CloudFront**: $0.00 per GB
- **CloudFront to Internet**: $0.085 per GB (slightly cheaper than S3)
  - Caching capability (lower latency)
  - Reduce costs associated with S3 Requests Pricing (7x cheaper with CloudFront)
- **S3 Cross Region Replication**: $0.02 per GB

## AWS Network Firewall

Protect the **entire VPC**. Internally it uses the GLB.  
**Rules can be managed cross account** and across VPC.  
We can filter with regex, ip, port, protocol, domain...

## VPC Sharing

Allow a **multiple AWS accounts to share one or more subnets**.

## AWS Resource Access Manager (RAM)

Allows sharing AWS **subnet resources** **across accounts** securely.
