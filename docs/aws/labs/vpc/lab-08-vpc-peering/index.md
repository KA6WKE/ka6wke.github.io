---
layout: default
title: "Lab 08: VPC Peering"
lab_level: associate
lab_service: vpc
lab_number: "08"
---

# Lab 08 - VPC Peering

> **Difficulty**: Intermediate
> **Service**: Amazon VPC

> **Cost**: This lab uses two t2.micro instances (Free Tier eligible — 750 hours/month for the
> first 12 months). If left running outside the Free Tier, the cost is approximately
> **$0.60/day**. Delete the stack when you are done.

## Scenario

Your team has two VPCs: VPC A hosts a web server that other internal systems need to reach,
and VPC B hosts a client instance that needs to communicate with it. A VPC peering connection
was created between the two VPCs and the connection shows as **Active**.

But the team reports that the client instance in VPC B still cannot reach the web server in
VPC A over the private network. Your job is to find out why and fix it.

## What Was Deployed

| Resource                          | Purpose                                                          |
| --------------------------------- | ---------------------------------------------------------------- |
| `AWS::EC2::VPC` (x2)              | VPC A (`10.0.0.0/16`) and VPC B (`10.1.0.0/16`)                 |
| `AWS::EC2::Subnet` (x2)           | One public subnet in each VPC                                    |
| `AWS::EC2::InternetGateway` (x2)  | One Internet Gateway per VPC — both attached                     |
| `AWS::EC2::RouteTable` (x2)       | One route table per VPC — each has a `0.0.0.0/0 → IGW` route    |
| `AWS::EC2::SecurityGroup` (x2)    | VPC A allows HTTP port 80 from `10.1.0.0/16`; VPC B unrestricted |
| `AWS::EC2::VPCPeeringConnection`  | Peering connection between VPC A and VPC B — status: Active     |
| `AWS::IAM::Role` (x2)             | IAM roles with `AmazonSSMManagedInstanceCore` for Session Manager |
| `AWS::EC2::Instance` (x2)         | Web server in VPC A; client instance in VPC B                    |

The stack deployed without errors. The peering connection is Active. The web server in VPC A
is running and reachable from the internet. The client instance in VPC B is running.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-08-vpc-peering.yaml" download>lab-08-vpc-peering.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-08`) and click **Next** > **Next** > **Submit**

   > **IAM notice**: This template creates IAM roles. On the final confirmation page, check
   > the box acknowledging that CloudFormation will create IAM resources, then click **Submit**.

5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceAPrivateIP` and `InstanceBId`

## The Problem

Connect to the client instance in VPC B using Session Manager:

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) > **Instances**
2. Select the instance named `brokenlabs-vpc-lab-08-instance-b`
3. Click **Connect** > **Session Manager** tab > **Connect**

Once connected, test connectivity to the web server using its **private IP**:

```bash
curl --max-time 5 http://<InstanceAPrivateIP>/
```

Replace `<InstanceAPrivateIP>` with the value from the stack Outputs tab.

**Expected**: The web server response page is returned.
**Actual**: The connection times out — the client cannot reach the web server over the private network.

The peering connection shows as **Active** in the VPC console. The web server is running.
Something is still preventing traffic from flowing between the two VPCs.

## Fix the Lab

A VPC peering connection establishes the link between two VPCs, but traffic will not flow
until both VPCs know how to route packets to each other. Investigate what is missing from
each VPC's route table.

After applying the fix, reconnect via Session Manager and re-run the `curl` command to
confirm the client can reach the web server using its private IP.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE**

## Resources

- [VPC peering overview](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)
- [Update your route tables for a VPC peering connection](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-routing.html)
- [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Subnets for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html)
- [Internet gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- [VPC route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
- [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
