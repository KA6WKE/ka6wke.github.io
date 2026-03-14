---
layout: default
title: "AWS Broken Labs - S3 Troubleshooting"
---

# S3 Labs

Hands-on troubleshooting labs for Amazon S3.

## Why These Labs

Amazon S3 is one of the most widely used AWS services, and S3 access control is one
of the most common sources of real-world incidents. Subtle misconfigurations are
responsible for a significant portion of "why can't I access this?" tickets.

These labs reproduce those exact scenarios. Each lab deploys a realistic but broken
environment using CloudFormation. Your job is to diagnose the problem using the AWS
Console, identify the root cause, and apply the fix — the same way you would
in a production environment.

---

## Labs

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 01 | [Lab 01 — Access](lab-01-s3-access/) | Associate | Beginner |
| 02 | [Lab 02 — Endpoint](lab-02-endpoint/) | Associate | Beginner |
| 03 | [Lab 03 — Recovery](lab-03-versioning/) | Associate | Beginner |
| 04 | [Lab 04 — Sharing](lab-04-presigned-url/) | Associate | Beginner |
| 05 | [Lab 05 — Permissions](lab-05-permissions/) | Associate | Beginner |
| 06 | [Lab 06 — Replication](lab-06-replication/) | Associate | Intermediate |

---

## Prerequisites

- An AWS account with access to the AWS Console
- Basic familiarity with the AWS Console and S3 concepts

---

## Cleanup

After completing a lab, delete the CloudFormation stack to avoid ongoing charges:

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation)
2. Select your stack and click **Delete**

---

**Questions or bugs?** [Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)
