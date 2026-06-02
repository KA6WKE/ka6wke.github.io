# VPC Labs

Hands-on troubleshooting labs for Amazon VPC.

## Why These Labs

Amazon VPC is the networking foundation for nearly every AWS workload. VPC misconfigurations
are among the most common — and most frustrating — issues in real AWS environments. A route
missing from a table, a misconfigured NACL, or an overlooked subnet setting can silently
block traffic in ways that are hard to diagnose without hands-on experience.

These labs give you that experience. Each lab deploys a realistic but broken VPC environment
using CloudFormation. Your job is to diagnose the problem using the AWS Console, identify the
root cause, and apply the fix — the same way you would in a production environment.

---

## Labs

| #   | Lab                                  | Topic                                  | Level        | Difficulty   |
| --- | ------------------------------------ | -------------------------------------- | ------------ | ------------ |
| 01  | [VPC Lab 01](lab-01-igw-routing/)    | VPC Route Tables and Internet Gateways | Associate    | Intermediate |
| 02  | [VPC Lab 02](lab-02-nacl/)           | Network ACLs                           | Associate    | Intermediate |
| 03  | [VPC Lab 03](lab-03-sg/)             | Security Groups                        | Associate    | Intermediate |
| 04  | [VPC Lab 04](lab-04-rt-assoc/)       | Route Table Associations               | Associate    | Intermediate |
| 05  | [VPC Lab 05](lab-05-vpc-settings/)   | VPC Settings                           | Associate    | Intermediate |
| 06  | [VPC Lab 06](lab-06-private-subnet/) | Private Subnets                        | Professional | Advanced     |
| 07  | [VPC Lab 07](lab-07-nat-gateway/)    | NAT Gateways                           | Professional | Advanced     |
| 08  | [VPC Lab 08](lab-08-vpc-peering/)    | VPC Peering                            | Associate    | Intermediate |
| 09  | [VPC Lab 09](lab-09-vpc-endpoint/)   | VPC Endpoints                          | Associate    | Intermediate |
| 10  | [VPC Lab 10](lab-10-nacl/)           | Network ACLs                           | Associate    | Intermediate |

---

## Prerequisites

- An AWS account with access to the AWS Console
- Basic familiarity with EC2 (instances, security groups)
- Some exposure to VPC concepts is helpful but not required

---

## Cost

All labs use a **t2.micro** instance (Free Tier eligible — 750 hours/month for the first
12 months). If you are outside the Free Tier, each lab costs approximately **$0.30/day**
if left running.

**Delete each stack promptly when you are done.**

---

## Cleanup

Any services created outside of Cloud Formation MUST be deleted manually before deleting the Cloud Formation stack.

After completing a lab, delete the CloudFormation stack to avoid ongoing charges:

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation)
2. Select your stack and click **Delete**
