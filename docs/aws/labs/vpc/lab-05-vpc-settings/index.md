---
layout: default
title: "Lab 05: VPC Settings"
lab_level: associate
lab_service: vpc
lab_number: "05"
---

# Lab 05 - VPC Settings

> **Difficulty**: Intermediate
> **Service**: Amazon VPC

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2 in a custom VPC. The CloudFormation stack completed
successfully and the instance is running. But when you look at the stack Outputs, something
about the `WebPageURL` doesn't look right — and clicking it doesn't work.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::EC2::VPC` | Custom VPC for the lab (`10.0.0.0/16`) |
| `AWS::EC2::Subnet` | Subnet with auto-assign public IP enabled |
| `AWS::EC2::InternetGateway` | Internet Gateway — created and attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a `0.0.0.0/0` route to the Internet Gateway |
| `AWS::EC2::SecurityGroup` | Inbound rule allowing HTTP on port 80 |
| `AWS::EC2::Instance` | t2.micro running a web server |

The stack deployed without errors. The instance is running and the web server is active.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-05-vpc-settings.yaml" download>lab-05-vpc-settings.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-05`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `InstancePublicIP`, and `WebPageURL`

## The Problem

Look at the `WebPageURL` value in the stack Outputs tab.

**Expected**: a valid URL like `http://ec2-1-2-3-4.compute-1.amazonaws.com/`
**Actual**: the URL looks like this:

```
http:///
```

The hostname is missing entirely. Clicking the URL produces an error in the browser because
`http:///` is not a valid address.

The instance is running and healthy — the web server is active. Something about the VPC
configuration is preventing the instance from being assigned a proper address.

## Fix the Lab

The instance has a public IP address (see `InstancePublicIP` in the Outputs). The routing,
security group, and Internet Gateway are all correctly configured. Investigate what VPC-level
setting controls whether instances receive the type of address that is missing from the URL.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)

## Resources

- [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Subnets for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html)
- [Internet gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- [VPC route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [DNS attributes for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)
- [View and update DNS attributes for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-updating)
- [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
