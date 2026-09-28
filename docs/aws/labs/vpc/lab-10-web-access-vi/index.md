---
layout: default
title: "Lab 10: Web Access VI"
lab_level: associate
lab_service: vpc
lab_number: "10"
redirect_from: /docs/aws/labs/vpc/lab-10-nacl/
---

# Lab 10 - Web Access VI

> **Difficulty**: Intermediate
> **Service**: Amazon VPC
>
> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside
> the Free Tier, the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2 in a custom VPC. The CloudFormation stack completed
successfully and the instance is running. But the web page won't load.

## What Was Deployed

| Resource | Purpose |
|---|---|
| `AWS::EC2::VPC` | Custom VPC for the lab (`10.0.0.0/16`) |
| `AWS::EC2::Subnet` | Subnet with auto-assign public IP enabled |
| `AWS::EC2::InternetGateway` | Internet Gateway — created and attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a `0.0.0.0/0` route to the Internet Gateway |
| `AWS::EC2::NetworkAcl` | Network ACL associated with the subnet |
| `AWS::EC2::SecurityGroup` | Inbound rule allowing HTTP on port 80 |
| `AWS::EC2::Instance` | t2.micro running a web server |

The stack deployed without errors. The instance is running and the web server is active.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-10-web-access-vi.yaml" download>lab-10-web-access-vi.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-10`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `WebPageURL` and `InstancePublicIP`

## The Problem

Click the `WebPageURL` link from the stack Outputs tab.

**Expected**: The Broken Labs success page loads.
**Actual**: The browser times out — `ERR_CONNECTION_TIMED_OUT`.

The instance is running and shows a healthy status in the EC2 console.

## Fix the Lab

Diagnose why the page won't load and fix it so the result matches **Expected** above.
After applying your fix, reload the `WebPageURL` to confirm the page loads.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE**

## Resources

- [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)

