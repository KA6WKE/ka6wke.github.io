---
layout: default
title: "Lab 05: Temporary Access Grant Not Working"
lab_level: professional
lab_service: iam
lab_number: "05"
---

# Lab 05 - Temporary Access Grant Not Working

> **Difficulty**: Advanced
> **Service**: AWS IAM, Amazon S3

> **Cost**: This lab deploys only IAM resources — no charges apply.

## Scenario

A contractor was granted temporary read access to an S3 bucket for the duration of a
project. The stack deployed without errors and the policy clearly allows
`s3:GetObject` — but the contractor reports that access has stopped working, even
though the project is still active.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::S3::Bucket` | Target bucket the contractor's role is meant to read from |
| `AWS::IAM::Role` | Role with a time-limited inline policy granting read access |

The stack deployed without errors. The role's policy explicitly allows `s3:GetObject`
on the bucket.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-05-access-stopped-working.yaml" download>lab-05-access-stopped-working.yaml</a>
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

The policy's `Allow` statement has an `Action` and `Resource` that both look correct.
Something else attached to the statement is limiting when it applies.

## Fix the Lab

Open the role in the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and
look at the full policy statement, including any `Condition` block — not just the
`Action` and `Resource` fields.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)
- [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html)
- [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)
