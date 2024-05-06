# Route 53

## DNS Reminders

- **Domain Registrar:** GoDaddy, Route53...
- **Domain Records:** A, AAAA, CNAME, NS...
- **Zone File:** Contains DNS Records
- **Name Server:** Resolves DNS queries
- **Top Level Domain (TLD):** .com, .us, .in, .gov...
- **Second Level Domain (SLD):** google.com, amazon.com...
- **Sub Domain:** www.example.com
- **Fully Qualified Domain Name (FQDN):** api.www.example.com, example.com...
- **Protocol:** http, https...
- **URL:** http://www.google.com  
- **Record:** (Sub)Domain name + record type + value + routing policy + TTL
- **Hosted Zones:**  DNS Records, can be public or private.

*(Computer -> Local DNS -> Root DNS -> Local DNS -> TLD DNS -> Local DNS -> SLD DNS -> Local DNS -> Computer)*  
*Local DNS Cache the final IP Address.*

## Record Types reminder

- A: IPv4
- AAAA: IPv6
- CNAME: Hostname to hostname, only for subdomains
- NS: Name Servers, how traffic is routed.

## Route 53 Precisions

It is an **authoritative DNS**.  
It is also a **Domain Registrar**.  
Only service that provides **100%** availability in SLA.  
Route 53 has the ability to **check the health** of your resources.

## Alias records

Allow to **points a hostname to an AWS Resource**.  
**Works for root and non-root domain**.  
Free of charge & provides health checks.  
*(Only for A/AAAA records, **does NOT works with EC2 DNS name**)*

## Health Checks

They are health checkers from all around the world only **for public resources**.  
For **automated DNS failover**.  
Types:

- Monitor **an endpoint**
- Monitor **other health checks** *(calculated health checks)*
- Monitor **CloudWatch Alarms**

For **private healthchecks**, create a **CloudWatch metric and associate it with an alarm that the healthchecker monitors**.

## Routing Policies

- **Simple:** Route to a single resource. If there's multiple IPs, the client will choose randomly.
- **Weighted:** Partition a % of traffic to certain resources. Supports health checks.
- **Latency Based:** Return the lowest latency resource. Measured with AWS regions. Supports health checks.
- **Failover (Active/Passive):** A failover if health check fail on the primary endpoint.
- **Geolocation:** Map locations to certain IPs. Supports health checks. *(Per country IP...)*
- **Geoproximity:** Send user to the closest endpoint. **Bias** (in/de)crease the range of an endpoint.
- **IP-Based:** Map the endpoint based on client CIDR IP addresses. *(Optimize cost and performance...)* 
- **Multi-Value Answer:** Route traffic to up to 8 healthy resources. **NOT A SUBSTITUTE TO ELBs!**
