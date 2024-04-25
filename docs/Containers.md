# Containers

## ECS EC2 Launch Type

You must **provision and maintain** the infrastructure yourself.  
Each EC2 instance must run the ECS Agent to **register in the ECS Cluster**.  
AWS takes care of **starting / stopping containers**

### ECS Fargate Launch Type

The infrastructure is **fully managed** by AWS.  
You just have to create **task definitions** *(= Image, Storage, RAM & CPU resources)*.

### ECS Instance Profile for EC2 Launch Type

Used by the **ECS Agent** and makes API calls to ECS, send logs to CloudWatch Logs, pull docker image, use SSM Parameter Store or Secrets Manager...

### ECS Task Role

Allow each task to have a specific role defined in the **task definition**.

### Load Balancer Integrations

You have integration **with ALB and NLB**.

### Data Volumes

You can use **EFS** if you need storage. **S3 Cannot be used**  
*Use-case: Multi-AZ Shared storage for containers*

## ECS Service Auto Scaling

ECS Auto Scaling uses **AWS Application Auto Scaling**.  
Based on **RAM, CPU or requests**. **Step, target and scheduled scaling** are available.  
It is easier in **Fargate**.    

## ECS Cluster Capacity Provider

A more advanced alternative to **basic ASG scaling**, that automatically scales a **paired ASG**.  
It add **EC2 instances** when you're missing capacity. Should be preferred over **raw ASGs**.

## EKS

EKS supports **EC2 and Fargate** modes.  
**Kubernetes is cloud-agnostic**.

### EKS Pods

Equivalent of **ECS Tasks**.

### EKS Node

Equivalent of EC2 instances.
Could be:
- **Managed Node Groups:** AWS create and manages nodes.
- **Self-Managed Nodes:** Manage nods yourself, use prebuilt AMIs.
- **Fargate Node:** Fully managed by EKS

### Data Volumes

You use a **storage class manifest** for the EKS cluster.  
It leverages a **Container Storage Interface (CSI)** compliant driver. *(= Used storage)*  
Supports **EBS, EFS, FSx Lustre, FSx NetApp ONTAP**

### App Runner

Fully managed service to deploy **web applications and APIs**.  
Does **not require** any infrastructure experience.  
Start with **source code or container image**.  
Automatically **build and deploy the web app** with scaling, load balancing, encryption.




