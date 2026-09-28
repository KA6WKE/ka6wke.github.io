---
layout: default
title: "Lab 05: Access Denied V"
lab_level: professional
lab_service: iam
lab_number: "05"
redirect_from: /docs/aws/labs/iam/lab-05-access-stopped-working/
---

# Lab 05 - Access Denied V

> **Difficulty**: Expert
> **Service**: AWS IAM, Amazon S3

> **Cost**: This lab deploys only IAM resources — no charges apply.

## Scenario

A contractor was granted temporary read access to an S3 bucket for the duration of a
project. The stack deployed without errors — but the contractor reports that access has
stopped working, even though the project is still active.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::S3::Bucket` | Target bucket the contractor's role is meant to read from |
| `AWS::IAM::Role` | Role the contractor uses to read from the bucket |

The stack deployed without errors. The role exists and has a policy attached.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-05-access-denied-v.yaml" download>lab-05-access-denied-v.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-iam-lab-05`), click **Next** > **Next**
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
> pick **`object`** and fill in the bucket name and key — using `accesspointobject`
> will error since there's no access point to reference.

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

