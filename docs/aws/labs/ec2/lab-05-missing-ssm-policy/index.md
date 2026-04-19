---
layout: default
title: "Lab 05: SSM Session Manager"
lab_level: associate
lab_service: ec2
lab_number: "05"
---

# Lab 05 - SSM Session Manager

> **Difficulty**: Intermediate
> **Service**: Amazon EC2, AWS IAM, AWS Systems Manager

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2. The website loads fine. The instance has an IAM
role attached. But when you try to open a Session Manager terminal to investigate the
instance, the connection fails. The role exists — so what is it missing?

## What Was Deployed

| Resource                    | Purpose                                                   |
| --------------------------- | --------------------------------------------------------- |
| `AWS::EC2::VPC`             | Dedicated VPC for the lab                                 |
| `AWS::EC2::Subnet`          | Public subnet with internet access                        |
| `AWS::EC2::InternetGateway` | Internet gateway attached to the VPC                      |
| `AWS::EC2::RouteTable`      | Route table with a default route to the internet          |
| `AWS::EC2::SecurityGroup`   | Allows inbound traffic on ports 80 and 22                 |
| `AWS::IAM::Role`            | IAM role attached to the instance — but with no policies  |
| `AWS::IAM::InstanceProfile` | Attaches the role to the instance                         |
| `AWS::EC2::Instance`        | t2.micro running Amazon Linux 2023 with Apache web server |

The stack deployed without errors. The web page loads. The instance has an IAM role.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-05-ssm-session-manager.yaml" download>lab-05-ssm-session-manager.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-ec2-lab-05`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `InstancePublicIP`, `WebPageURL`, and `RoleName`

## The Problem

First, confirm the web page loads — open `WebPageURL` from the Outputs. The AWS Broken Labs
page displays correctly. Apache is healthy.

Now try to open a Session Manager terminal:

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) and select your instance
2. Click **Connect** > **Session Manager** > **Connect**

**Expected**: a browser-based terminal opens.
**Actual**: the button is grayed out, or the connection fails with an error about the instance
not being available in Session Manager.

The instance has an IAM role attached — but Session Manager still won't connect.

## Fix the Lab

Investigate the IAM role attached to the instance. Session Manager requires specific
permissions to be present in the instance's role.

The role name is shown in the `RoleName` stack Output.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)
4. Verify in [IAM → Roles](https://console.aws.amazon.com/iam/home#/roles) that the lab role no longer appears

## Resources

- [Configure instance permissions for Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-instance-profile.html)
- [AmazonSSMManagedInstanceCore policy](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonSSMManagedInstanceCore.html)
- [Troubleshoot Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-troubleshooting.html)
