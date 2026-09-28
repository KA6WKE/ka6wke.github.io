---
layout: default
title: "Lab 01: Access Denied I"
lab_level: associate
lab_service: iam
lab_number: "01"
redirect_from: /docs/aws/labs/iam/lab-01-s3-read-denied/
---

# Lab 01 - Access Denied I

> **Difficulty**: Beginner
> **Service**: AWS IAM, Amazon S3

> **Cost**: This lab deploys only IAM resources — no charges apply.

## Scenario

An application team was granted an IAM role so their service can read objects from an
S3 bucket. The stack deployed without errors and the role has a policy attached — but
the team reports that read requests are still being denied.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::S3::Bucket` | Target bucket the role is meant to read from |
| `AWS::IAM::Role` | Role the application uses to read from the bucket |

The stack deployed without errors. The role exists and has a policy attached.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-01-access-denied-i.yaml" download>lab-01-access-denied-i.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-iam-lab-01`), click **Next** > **Next**
5. Check **I acknowledge that AWS CloudFormation might create IAM resources** and click **Submit**
6. Wait for the stack status to reach **CREATE_COMPLETE**
7. Open the stack **Outputs** tab — note `RoleName`, `BucketResourceArn`, and
   `ObjectResourceArn`

## The Problem

Open the [IAM Policy Simulator](https://policysim.aws.amazon.com/), select the role
named in the stack **Outputs** (`RoleName`), choose the `s3:GetObject` action, and set
the resource ARN to the `ObjectResourceArn` value from Outputs. Run the simulation.

> **Note**: If you're using the **Simulate** button inside the IAM console's policy
> editor instead of the standalone Policy Simulator, each action you add gets its own
> **Resource** field that defaults to `*`. Enter the ARN from the stack Outputs instead of
> leaving the default `*`, which can show **denied** regardless of the policy.

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

