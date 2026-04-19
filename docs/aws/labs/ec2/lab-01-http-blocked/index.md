---
layout: default
title: "Lab 01: EC2 Security Groups"
lab_level: associate
lab_service: ec2
lab_number: "01"
---

# Lab 01 - EC2 Security Groups

> **Difficulty**: Beginner
> **Service**: Amazon EC2

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2. The CloudFormation stack completed successfully and the
instance is running — but the page won't load in the browser. The server is up. Something is
blocking traffic before it even reaches the instance.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::EC2::VPC` | Dedicated VPC for the lab |
| `AWS::EC2::Subnet` | Public subnet with internet access |
| `AWS::EC2::InternetGateway` | Internet gateway attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a default route to the internet |
| `AWS::EC2::SecurityGroup` | Controls inbound and outbound traffic to the instance |
| `AWS::EC2::Instance` | t2.micro running Amazon Linux 2023 with Apache web server |

The stack deployed without errors. Apache is installed and running on the instance.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-01-ec2-security-groups.yaml" download>lab-01-ec2-security-groups.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-ec2-lab-01`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `InstancePublicIP`, and `WebPageURL`

## The Problem

Open the `WebPageURL` from the stack Outputs in your browser.

**Expected**: the AWS Broken Labs welcome page loads.
**Actual**: the browser displays:

```
This site can't be reached
ERR_CONNECTION_TIMED_OUT
```

The instance is running and shows a healthy status in the EC2 console. Apache started
successfully during launch. The page simply never arrives.

## Fix the Lab

Investigate what controls inbound traffic to an EC2 instance and determine what is missing.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)

## Resources

- [Amazon EC2 security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)
- [Add rules to a security group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-security-groups.html#adding-security-group-rule)
