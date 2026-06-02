---
layout: default
title: "Lab 04: VPC Route Table Association"
lab_level: associate
lab_service: vpc
lab_number: "04"
---

# Lab 04 - VPC Route Table

> **Difficulty**: Intermediate
> **Service**: Amazon VPC

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2 in a custom VPC. The CloudFormation stack completed
successfully. The instance is running, the Internet Gateway is attached, and there is a route
table that routes internet traffic through the gateway. But the page still won't load.

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
3. Select **Upload a template file** and upload <a href="lab-04-rt-assoc.yaml" download>lab-04-rt-assoc.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-04`) and click **Next** > **Next** > **Submit**
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

The instance is running and healthy. The Internet Gateway is attached. A route table exists
with a working route to the Internet Gateway. The security group allows HTTP. The page still
never arrives.

## Fix the Lab

The route table and the Internet Gateway are both in place. Dig into how route tables actually
apply to subnets — the relationship between them may not be what it appears.

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
- [Subnet route table associations](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithRouteTables.html#AssociateSubnet)
- [Main route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#main-route-table)
- [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
