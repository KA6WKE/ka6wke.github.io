---
layout: default
title: "AWS Broken Labs - IAM Troubleshooting"
---

# IAM Labs

Hands-on troubleshooting labs for AWS Identity and Access Management (IAM).

## Why These Labs

AWS IAM controls who can do what in your account, and IAM misconfigurations are
responsible for some of the most common — and most confusing — access issues in real
AWS environments. A missing action, a stray `Deny` statement, a mismatched resource
ARN, or a permissions boundary that's out of sync with a role's policy can all produce
the same symptom: **AccessDenied**, with no obvious clue as to why.

These labs give you that troubleshooting experience without deploying any running
compute. Each lab deploys a realistic but broken IAM configuration using
CloudFormation. Instead of reproducing the break against live infrastructure, you use
the **IAM Policy Simulator** to test the exact action and resource in question, see
the denial, diagnose the root cause, and confirm your fix — the same way you would
investigate an access issue in a production account.

---

## Labs

| # | Lab | Topic | Level | Difficulty |
| --- | --- | --- | --- | --- |
| 01 | [IAM Lab 01](lab-01-s3-read-denied/) | Missing action in an identity policy | Associate | Beginner |
| 02 | [IAM Lab 02](lab-02-role-access-denied/) | Explicit Deny vs. Allow | Associate | Beginner |
| 03 | [IAM Lab 03](lab-03-object-access-denied/) | Resource ARN specificity | Associate | Intermediate |
| 04 | [IAM Lab 04](lab-04-unexpected-access-denied/) | Permissions boundaries | Professional | Advanced |
| 05 | [IAM Lab 05](lab-05-access-stopped-working/) | IAM policy conditions | Professional | Advanced |
| 06 | [IAM Lab 06](lab-06-lambda-role-denied/) | `iam:PassRole` | Associate | Intermediate |

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
