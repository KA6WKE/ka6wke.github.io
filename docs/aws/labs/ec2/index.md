# EC2 Labs

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

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 01 | [EC2 Lab 01](lab-01-http-blocked/) | Associate | Beginner |
| 02 | [EC2 Lab 02](lab-02-instance-connect/) | Associate | Beginner |
| 03 | [EC2 Lab 03](lab-03-no-public-ip/) | Associate | Beginner |
| 04 | [EC2 Lab 04](lab-04-no-iam-role/) | Associate | Beginner |
| 05 | [EC2 Lab 05](lab-05-missing-ssm-policy/) | Associate | Intermediate |
| 06 | [EC2 Lab 06](lab-06-user-data/) | Associate | Intermediate |

---

## Prerequisites

- An AWS account with access to the AWS Console
- Basic familiarity with the AWS Console and EC2 concepts

---

## Cost

All labs use a **t2.micro** instance (Free Tier eligible — 750 hours/month for the first
12 months). If you are outside the Free Tier, each lab costs approximately **$0.30/day**
if left running. Lab 03 may incur an additional charge — see the lab README for details.

**Delete each stack promptly when you are done.**

---

## Cleanup

After completing a lab, delete the CloudFormation stack to avoid ongoing charges:

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation)
2. Select your stack and click **Delete**

Some labs require additional cleanup steps before deleting the stack. Check the
**Cleanup** section in each lab's README for details.
