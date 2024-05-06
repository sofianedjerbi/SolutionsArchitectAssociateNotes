# IAM

## Organizations

The main account is the **management account**. The other are **member accounts**.  
You can share reserved instances and savings plans across accounts.

You can split an organization in **organizational units**, with SCP hierarchy (the root OU global by default).  
**Deny policies take over allow policies**.

## IAM Advanced Policies

### Conditions

You can **restrict IPs** with **aws:sourceIp** condition. Same with regions with **aws:RequestedRegion**.  
You can allow / restrict based on **tags** with **ec2:ResourceTag**.  
You can enforce MFA with **aws:MultiFactorAuthPresent**  
You can restrict resource access to accounts that are member of an organization with **PrincipalOrgID**.

### IAM for S3

There are **bucket-level permissions** and **object-level permissions**.
*ListBucket vs GetObject*

## IAM Roles vs Resource Based Policies
For cross-account access to S3 you have two choices:

- Attach a resource-based policy to a resource *(Account A -> Bucket Policy -> S3)*
- Use a role as a proxy *(Account A -> Role account B -> S3)*

**Assuming a role** make you take all permission of the role **and lose your permissions**.  
With a resource-based policy you **don't have to give up permissions**.

When using EventBridge, you **should use resource-based policy OR an IAM role to allow EventBridge to write**.

## IAM Permissions Boundaries Precisions

Only for users and roles, **not groups**.  
It defines **maximum permission limit**. Giving a permission **outside the boundary** have **no effect**.

## IAM Identity Center Precisions

Login in a **single place** for **multiple AWS accounts**, with **external login integration** *(active directory...)*.  
You can create a **permission set** that allow access to accounts **within an OU**.  
**Multi-Account Permissions** to manage access across accounts in the organization.  
**Attribute Based Access Control (ABAC)** allow to define permissions based on users attributes. *(Center, Titre, Locale)*

## Directory Services Precisions

- **AWS Managed Microsoft AD:** Create AD in AWS, share users and objects with on-premises AD.
- **AD Connector:** A proxy to redirect to on-premises AD.
- **Simple AD:** AD-compatible managed directory on AWS. Cannot be joined with on-premises.

## Control Tower Precisions

It **automatically** creates accounts for you.  
You can monitor everything with a dashboard.  
**Preventive Guardrail** with **SCPs** can prevent someone to do something. *(restrict regions across accounts)*  
**Detective Guardrail** to trigger something is something is not compliant. *(identity untagged resources)*
