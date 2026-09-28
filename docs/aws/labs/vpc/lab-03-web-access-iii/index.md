---
layout: default
title: "Lab 03: Web Access III"
lab_level: associate
lab_service: vpc
lab_number: "03"
redirect_from: /docs/aws/labs/vpc/lab-03-sg/
---

# Lab 03 - Web Access III

> **Difficulty**: Intermediate
> **Service**: Amazon VPC

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2 in a custom VPC. The CloudFormation stack completed
successfully and the instance is running. But when you open the URL, the page won't load.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::EC2::VPC` | Custom VPC for the lab (`10.0.0.0/16`) |
| `AWS::EC2::Subnet` | Subnet with auto-assign public IP enabled |
| `AWS::EC2::InternetGateway` | Internet Gateway — created and attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a `0.0.0.0/0` route to the Internet Gateway |
| `AWS::EC2::SecurityGroup` | Security group controlling inbound traffic to the instance |
| `AWS::EC2::Instance` | t2.micro running a web server |

The stack deployed without errors. The instance is running and the web server is active.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-03-web-access-iii.yaml" download>lab-03-web-access-iii.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-03`) and click **Next** > **Next** > **Submit**
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

The instance is running and shows a healthy status in the EC2 console.

## Fix the Lab

Diagnose why the page won't load and fix it so the result matches **Expected** above.
After applying your fix, reload the `WebPageURL` to confirm the page loads.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)

## Resources

- [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)

