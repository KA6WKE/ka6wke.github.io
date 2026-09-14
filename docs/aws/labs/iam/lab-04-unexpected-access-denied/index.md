---
layout: default
title: "Lab 04: Read Access Denied Despite a Clear Allow"
lab_level: professional
lab_service: iam
lab_number: "04"
---

# Lab 04 - Read Access Denied Despite a Clear Allow

> **Difficulty**: Advanced
> **Service**: AWS IAM, Amazon S3

> **Cost**: This lab deploys only IAM resources — no charges apply.

## Scenario

An IAM role's permissions policy clearly grants read access to an S3 bucket — you've
double- and triple-checked it. The stack deployed without errors. But requests from
the role to read objects in the bucket are still denied.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::S3::Bucket` | Target bucket the role is meant to read from |
| `AWS::IAM::ManagedPolicy` | A separate policy attached to the role as a permissions boundary |
| `AWS::IAM::Role` | Role with an inline policy allowing bucket reads, plus the permissions boundary |

The stack deployed without errors. The role's own permissions policy allows
`s3:GetObject` and `s3:ListBucket` on the bucket.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-04-unexpected-access-denied.yaml" download>lab-04-unexpected-access-denied.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-iam-lab-04`), click **Next** > **Next**
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

**Expected**: the simulation result for `s3:GetObject` shows **allowed** — the role's
policy explicitly grants this action.
**Actual**: the simulation result shows **denied**, even though the role's own policy
looks correct.

## Fix the Lab

In the [IAM console](https://console.aws.amazon.com/iam/home#/roles), open the role and
look beyond its permissions policy — open the **Permissions** tab and look for the
**Permissions boundary** section (it may be collapsed near the bottom of the tab).

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)
- [Permissions boundaries for IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)
- [IAM policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
