---
layout: default
title: "AWS Broken Labs - Amazon VPC"
---

# Amazon VPC Labs

Hands-on troubleshooting labs for Amazon VPC.

## Why These Labs

Amazon VPC is the networking foundation for nearly every AWS workload. VPC misconfigurations
are among the most common — and most frustrating — issues in real AWS environments. A single
overlooked setting can silently block traffic in ways that are hard to diagnose without
hands-on experience.

These labs give you that experience. Each lab deploys a realistic but broken VPC environment
using CloudFormation. Your job is to diagnose the problem using the AWS Console, identify the
root cause, and apply the fix — the same way you would in a production environment.

---

## Labs

### Beginner

Beginner labs coming soon.

### Intermediate

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 01 | [Lab 01 — Web Access I](lab-01-web-access-i/) | Associate | Intermediate |
| 02 | [Lab 02 — Web Access II](lab-02-web-access-ii/) | Associate | Intermediate |
| 03 | [Lab 03 — Web Access III](lab-03-web-access-iii/) | Associate | Intermediate |
| 04 | [Lab 04 — Web Access IV](lab-04-web-access-iv/) | Associate | Intermediate |
| 05 | [Lab 05 — Web Access V](lab-05-web-access-v/) | Associate | Intermediate |
| 08 | [Lab 08 — Cross-VPC Access](lab-08-cross-vpc-access/) | Associate | Intermediate |
| 09 | [Lab 09 — Private S3 Access](lab-09-private-s3-access/) | Associate | Intermediate |
| 10 | [Lab 10 — Web Access VI](lab-10-web-access-vi/) | Associate | Intermediate |

### Expert

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 06 | [Lab 06 — Outbound Access I](lab-06-outbound-access-i/) | Professional | Expert |
| 07 | [Lab 07 — Outbound Access II](lab-07-outbound-access-ii/) | Professional | Expert |

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

Any services created outside of CloudFormation MUST be deleted manually before deleting the CloudFormation stack.

After completing a lab, delete the CloudFormation stack to avoid ongoing charges:

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation)
2. Select your stack and click **Delete**

---

**Questions or bugs?** [Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)
