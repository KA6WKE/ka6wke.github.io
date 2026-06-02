---
layout: default
title: "Lab 09: VPC Endpoints"
lab_level: associate
lab_service: vpc
lab_number: "09"
---

# Lab 09 - VPC Endpoint

> **Difficulty**: Intermediate
> **Service**: Amazon VPC
>
> **Cost**: This lab uses a t2.micro instance plus three VPC interface endpoints.
> Estimated cost if left running: approximately **$1.00/day**. Delete the stack when you are done.

## Scenario

Your team deployed an EC2 instance in a private subnet. The instance needs to access Amazon
S3 — but rather than routing that traffic through the internet via a NAT Gateway, your team
created a VPC Gateway Endpoint for S3 to keep the traffic private and save cost.

The endpoint was created and shows as **Available**. But the instance still cannot reach S3.
Your job is to find out why.

## What Was Deployed

| Resource                     | Purpose                                                                 |
| ---------------------------- | ----------------------------------------------------------------------- |
| `AWS::EC2::VPC`              | Custom VPC for the lab (`10.0.0.0/16`)                                  |
| `AWS::EC2::Subnet` (x2)      | Public subnet (`10.0.1.0/24`) and private subnet (`10.0.2.0/24`)        |
| `AWS::EC2::InternetGateway`  | Internet Gateway — created and attached to the VPC                      |
| `AWS::EC2::RouteTable` (x2)  | Public route table (has `0.0.0.0/0 → IGW`) and private route table      |
| `AWS::EC2::VPCEndpoint` (x3) | Interface endpoints for SSM, SSMMessages, EC2Messages                   |
| `AWS::EC2::VPCEndpoint`      | Gateway endpoint for S3 — status: Available                             |
| `AWS::IAM::Role`             | IAM role with `AmazonSSMManagedInstanceCore` for Session Manager access |
| `AWS::EC2::Instance`         | t2.micro in the **private subnet** — no public IP                       |

The stack deployed without errors. The instance is running. Session Manager connectivity
works. The S3 gateway endpoint shows as **Available**.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-09-vpc-endpoint.yaml" download>lab-09-vpc-endpoint.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-09`) and click **Next** > **Next** > **Submit**

   > **IAM notice**: This template creates an IAM role. On the final confirmation page, check
   > the box acknowledging that CloudFormation will create IAM resources, then click **Submit**.

5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 3–5 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `PrivateIP`, and `S3TestURL`

## The Problem

Connect to the instance using Session Manager:

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) > **Instances**
2. Select the instance named `brokenlabs-vpc-lab-09`
3. Click **Connect** > **Session Manager** tab > **Connect**

Once connected, test connectivity to S3. Use the `S3TestURL` value from the stack Outputs tab:

```bash
curl --max-time 5 <S3TestURL>
```

**Expected**: The command completes immediately — S3 responds (the body may be empty or XML).
**Actual**: The connection times out after 5 seconds — the instance cannot reach S3.

The gateway endpoint exists and shows as Available. Something about its configuration is
preventing the private instance from using it.

## Fix the Lab

A VPC Gateway Endpoint works by injecting a route into the route tables it is associated
with. Investigate which route tables the endpoint is currently associated with, and whether
that is the correct configuration for an instance in the private subnet.

After applying the fix, reconnect via Session Manager and re-run the `curl` command. Any
response from S3 — including an XML error — confirms that traffic is now reaching S3
through the endpoint.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE**

## Resources

- [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html)
- [Gateway endpoints for Amazon S3](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html)
- [VPC route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Subnets for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html)
- [Internet gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- [Interface VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html)
- [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
- [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
