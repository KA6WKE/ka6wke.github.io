---
layout: default
title: "Lab 06: Access Denied VI"
lab_level: associate
lab_service: iam
lab_number: "06"
redirect_from: /docs/aws/labs/iam/lab-06-lambda-role-denied/
---

# Lab 06 - Access Denied VI

> **Difficulty**: Intermediate
> **Service**: AWS IAM, AWS Lambda

> **Cost**: This lab deploys only IAM resources — no charges apply.

## Scenario

An automation role is used to create Lambda functions on behalf of the team. Each
function needs an execution role attached at creation time. The stack deployed without
errors and the automation role has a policy allowing it to create functions — but
function creation fails with a permissions error whenever an execution role is
specified.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::IAM::Role` (target) | Execution role intended to be attached to new Lambda functions |
| `AWS::IAM::Role` (actor) | Automation role that creates Lambda functions and must pass the execution role to them |

The stack deployed without errors. Both roles exist, and the automation role's policy
allows `lambda:CreateFunction`.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-06-access-denied-vi.yaml" download>lab-06-access-denied-vi.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-iam-lab-06`), click **Next** > **Next**
5. Check **I acknowledge that AWS CloudFormation might create IAM resources** and click **Submit**
6. Wait for the stack status to reach **CREATE_COMPLETE**
7. Open the stack **Outputs** tab — note `RoleName` and `TestResourceArn`

## The Problem

Open the [IAM Policy Simulator](https://policysim.aws.amazon.com/), select the role
named in the stack **Outputs** (`RoleName`), choose the `iam:PassRole` action, and set
the resource ARN to the `TestResourceArn` value from Outputs — this is the execution
role that needs to be passed to Lambda. Run the simulation.

**Expected**: the simulation result for `iam:PassRole` against this ARN shows
**allowed**.
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

