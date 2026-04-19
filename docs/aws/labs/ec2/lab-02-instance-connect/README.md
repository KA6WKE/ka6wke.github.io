# Lab 02 - EC2 Instance Connect

> **Difficulty**: Beginner
> **Service**: Amazon EC2

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2. The website loads fine in the browser. But when you
try to open a terminal session on the instance using EC2 Instance Connect, the connection
fails. You need to get into the instance to investigate a separate issue — but you can't get in.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::EC2::VPC` | Dedicated VPC for the lab |
| `AWS::EC2::Subnet` | Public subnet with internet access |
| `AWS::EC2::InternetGateway` | Internet gateway attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a default route to the internet |
| `AWS::EC2::SecurityGroup` | Controls inbound and outbound traffic to the instance |
| `AWS::EC2::Instance` | t2.micro running Amazon Linux 2023 with Apache web server |

The stack deployed without errors. Apache is installed and the web page is accessible.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload `lab-02-instance-connect.yaml`
4. Enter a stack name (e.g., `brokenlabs-ec2-lab-02`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `InstancePublicIP`, and `WebPageURL`

## The Problem

First, confirm the web page loads — open `WebPageURL` from the Outputs. The AWS Broken Labs
page should display correctly. The web server is healthy.

Now try to connect to the instance:

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) and select your instance
2. Click **Connect** > **EC2 Instance Connect** > **Connect**

**Expected**: a browser-based terminal opens.
**Actual**:

```
Failed to connect to your instance
Error establishing SSH connection. Please try again.
```

The web server works. The instance is healthy. But no one can get in to manage it.

## Fix the Lab

Investigate what EC2 Instance Connect requires and determine what is missing from this
instance's configuration.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)

## Resources

- [Connect using EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-eic.html)
- [Amazon EC2 security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)
