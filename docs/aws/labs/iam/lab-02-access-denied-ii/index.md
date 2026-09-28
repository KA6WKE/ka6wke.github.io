---
layout: default
title: "Lab 02: Access Denied II"
lab_level: associate
lab_service: iam
lab_number: "02"
redirect_from: /docs/aws/labs/iam/lab-02-role-access-denied/
---

# Lab 02 - Access Denied II

> **Difficulty**: Beginner
> **Service**: AWS IAM, Amazon S3

> **Cost**: This lab deploys only IAM resources — no charges apply.

## Scenario

A colleague recently updated an IAM role's permissions policy for an S3 bucket. The stack
deployed without errors — but now every read request from the role is denied, including
requests that used to succeed before the update.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::S3::Bucket` | Target bucket the role is meant to read from |
| `AWS::IAM::Role` | Role that reads from the bucket |

The stack deployed without errors. The role exists and has a policy attached.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-02-access-denied-ii.yaml" download>lab-02-access-denied-ii.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-iam-lab-02`), click **Next** > **Next**
5. Check **I acknowledge that AWS CloudFormation might create IAM resources** and click **Submit**
6. Wait for the stack status to reach **CREATE_COMPLETE**
7. Open the stack **Outputs** tab — note `RoleName` and `TestResourceArn`

## The Problem

Open the [IAM Policy Simulator](https://policysim.aws.amazon.com/), select the role
named in the stack **Outputs** (`RoleName`), choose the `s3:GetObject` action, and set
the resource ARN to the `TestResourceArn` value from Outputs. Run the simulation.

> **Note**: `s3:GetObject` supports two resource types in the console's resource
> picker — `object` (`arn:aws:s3:::<bucket>/<key>`) and `accesspointobject` (for
> requests routed through an S3 Access Point). This lab doesn't use an Access Point, so
> pick **`object`** and fill in the bucket name and key (`*` for any object) — using
> `accesspointobject` will error since there's no access point to reference.

**Expected**: the simulation result for `s3:GetObject` shows **allowed**.
**Actual**: the simulation result shows **denied**.

## Fix the Lab

Diagnose why the simulation is denied and fix the role's permissions so the result matches
**Expected** above. Re-run the simulation to confirm your fix.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)
- [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

