# Security

## Key Management Service (KMS)

AWS **managed encryption keys**. You can audit key usage with CloudTrail.  
Options :

- **Symmetric:** AED-256 keys
- **Asymmetric:** RSA & ECC key pairs

Keys types:

- AWS Owned: free, integrated, invisible.
- AWS Managed: free, integrated, accesible. *(aws/service-name)*
- Customer managed: 1$ / month

All keys are rotated **every year**, except for the customer imported keys that you should rotate manually.

And you pay for API call to KMS (3 cent / 10000 calls)

**KMS keys aren't cross region**

## KMS Key Policies

**Control access to KMS keys**. Default is entire account.  
Can be used for cross account access.

## Multi-Region Keys

The key is **replicated** across regions and the **key ID is the same**.  
Used to avoid reencryption in global services such as DynamoDB or Aurora.

## S3 Replication With Encryption
**Unecrypted and object encrypted with SSE-S3 are replicated by default.**  
Object encrypted with **SSE-C can be replicated**.  
For object encrypted with **SSE-KMS** you need to enable the option.  
*(Target and origin KMS keys, IAM role with decrypt for source and encrypt for target)*  
Even if the **key is multiregion, it will still be treated as two different keys**.

## Share Encrypted AMIs

0- AMI is encrypted  
1- Add **launch permission** to the target AWS account.  
2- Share the **KMS Keys** with a **key policy**.   
3- IAM User in target account should have **DescribeKey, ReEncrypted, CreateGrant, Decrypt** permissions.  
4- You can launch it, but you can also **re-encrypt the volumes**.  

## SSM Parameter Store Precisions

Have **version tracking** of configurations, optional **encryption with KMS**.  
Integration with CloudFormation.  
There can be a **hierarchy** in parameters:
- /dev/
  - /dev/app
    - /dev/app/dburl
    - /dev/app/dbpassword
- /prod/
  - /prod/dbpassword  

There are private and public parameters such as */aws/service/ami-amazon-linux-latest/amzn2-ami-hvn-x86_64-gp2*  
**Advanced Parameter** is a paid bigger parameter and have a policy.  
**Parameters Policies** allow to define a TTL. *(Then eventbridge will receive a notification)*

## Secrets Manager Precisions

Automatic regeneration of secrets on rotation with Lambda.  
Integration with **Amazon RDS**. *(DB logins are stored in SM)*  
It supports **multi-region secret replication**.  

## AWS Certificate Manager Precisions

Supports **public (free) and private** TLS certificates.  
Integration with **CloudFront, ELS, API Gateway**.  
You **can't use ACM with EC2**.

There is **automatic renewal**.  
When **importing a certificate** there is **no renewal**. *(ACM will send daily expiration events)*  

For API Gateway, **certificates must be in the same region**: *(us-east-1)*  
In Edge-Optimized, **CloudFront** is linked to **ACM**, in **regional it is API Gateway** directly.


## Web Application Firewall (WAF) Precisions

Protect in HTTP layer 7.  
You can deploy it on: **ALB, API Gateway, CloudFront, AppSync, Cognito User Pool**.  
You can then define **Web ACLs**:
- Filter based on ip addresses
- Filter on HTTP header, body, url strings
- Block countries
- Rate-based rules  

They are **regional except for CloudFront**.  
A **rule group** is a **reusable set of rules** that you can add to a web ACL.  

To get a **fixed IP** for WAF with ALB, you can use the **Global Accelerator**.

## Firewall Manager Precisions

Policies are at **region level**.  
**Rules applies to new resources**.

## Best practices against DDoS

At the edge:  
Use **cloudfront + shield, global accelerator, Route53**, the AWS infrastructure.

Inside the infrastructure:  
Balance **traffic** with ELBs and ASGs.  

At application level:  
Detect and **filter web requests** with CloudFront or WAF.  
There is Shield Advanced too.

Reduce attack surface:  
**Obfuscationg AWS resources** with Cloudfront, API Gateway, ELBs...  
**Security groups and NACLs**. Hide API endpoints.

## GuardDuty Precisions

**Input Data:** CloudTrail logs, VPC Flow Logs, DNS Logs.  
**EventBridge Rules** can notify you in case of findings.  
**It can protect against CryptoCurrency attacks**.

## Inspector Precisions

It has integration with **security hub**.  
It does a continuous package vulnerabilities and network reachability analysis.  

## Macie Precisions

Works in **S3 buckets**.


