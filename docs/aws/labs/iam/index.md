---
layout: default
title: "AWS Broken Labs - AWS Identity and Access Management (IAM)"
---

# AWS Identity and Access Management (IAM) Labs

Hands-on troubleshooting labs for AWS Identity and Access Management (IAM).

## Why These Labs

AWS IAM controls who can do what in your account, and IAM misconfigurations are
responsible for some of the most common — and most confusing — access issues in real
AWS environments. Many different mistakes produce the same symptom: **AccessDenied**,
with no obvious clue as to why.

These labs give you that troubleshooting experience without deploying any running
compute. Each lab deploys a realistic but broken IAM configuration using
CloudFormation. Instead of reproducing the problem against live infrastructure, you use
the **IAM Policy Simulator** to test the exact action and resource in question, see
the denial, diagnose the root cause, and confirm your fix — the same way you would
investigate an access issue in a production account.

---

## Labs

### Beginner

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 01 | [Lab 01 — Access Denied I](lab-01-access-denied-i/) | Associate | Beginner |
| 02 | [Lab 02 — Access Denied II](lab-02-access-denied-ii/) | Associate | Beginner |

### Intermediate

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 03 | [Lab 03 — Access Denied III](lab-03-access-denied-iii/) | Associate | Intermediate |
| 06 | [Lab 06 — Access Denied VI](lab-06-access-denied-vi/) | Associate | Intermediate |

### Expert

| # | Lab | Level | Difficulty |
| --- | --- | --- | --- |
| 04 | [Lab 04 — Access Denied IV](lab-04-access-denied-iv/) | Professional | Expert |
| 05 | [Lab 05 — Access Denied V](lab-05-access-denied-v/) | Professional | Expert |

---

## Prerequisites

- An AWS account with access to the AWS Console
- Basic familiarity with the AWS Console and IAM concepts
- Access to the [IAM Policy Simulator](https://policysim.aws.amazon.com/) (uses your
  signed-in console credentials — no additional setup required)

---

## Cleanup

After completing a lab, delete the CloudFormation stack to avoid leaving unused IAM
resources in your account:

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation)
2. Select your stack and click **Delete**
