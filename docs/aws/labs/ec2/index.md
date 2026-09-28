---
layout: default
title: "AWS Broken Labs - Amazon EC2"
---

# Amazon EC2 Labs

Hands-on troubleshooting labs for Amazon EC2.

## Why These Labs

Amazon EC2 is one of the most widely used AWS services. Real-world EC2 incidents are
often caused by subtle misconfigurations that are easy to overlook and harder to diagnose
without hands-on experience.

These labs give you that experience. Each lab deploys a realistic but broken environment
using CloudFormation. Your job is to diagnose the problem using the AWS Console, identify
the root cause, and apply the fix — the same way you would in a production environment.

---

## Labs

### Beginner

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 01 | [Lab 01 — Web Access I](lab-01-web-access-i/) | Associate | Beginner |
| 02 | [Lab 02 — Instance Access I](lab-02-instance-access-i/) | Associate | Beginner |
| 03 | [Lab 03 — Web Access II](lab-03-web-access-ii/) | Associate | Beginner |
| 04 | [Lab 04 — Instance Access II](lab-04-instance-access-ii/) | Associate | Beginner |

### Intermediate

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 05 | [Lab 05 — Instance Access III](lab-05-instance-access-iii/) | Associate | Intermediate |
| 06 | [Lab 06 — Web Access III](lab-06-web-access-iii/) | Associate | Intermediate |

### Expert

Expert labs coming soon.

---

## Prerequisites

- An AWS account with access to the AWS Console
- Basic familiarity with the AWS Console and EC2 concepts

---

## Cost

All labs use a **t2.micro** instance (Free Tier eligible — 750 hours/month for the first
12 months). If you are outside the Free Tier, each lab costs approximately **$0.30/day**
if left running. Lab 03 may incur an additional charge — see the lab page for details.

**Delete each stack promptly when you are done.**

---

## Cleanup

After completing a lab, delete the CloudFormation stack to avoid ongoing charges:

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation)
2. Select your stack and click **Delete**

Some labs require additional cleanup steps before deleting the stack. Check the
**Cleanup** section on each lab page for details.
