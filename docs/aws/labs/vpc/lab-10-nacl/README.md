# Lab 10 - Network ACL

> **Difficulty**: Intermediate
> **Service**: Amazon VPC
>
> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside
> the Free Tier, the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2. The CloudFormation stack completed successfully,
the instance is running, the security group allows port 80, and the route table has a
working Internet Gateway route. But the web page won't load.

A network engineer recently added a custom Network ACL to the subnet for additional
security. Your job is to figure out what the ACL is doing to the traffic.

## What Was Deployed

| Resource | Purpose |
|---|---|
| `AWS::EC2::VPC` | Custom VPC for the lab (`10.0.0.0/16`) |
| `AWS::EC2::Subnet` | Subnet with auto-assign public IP enabled |
| `AWS::EC2::InternetGateway` | Internet Gateway — created and attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a `0.0.0.0/0` route to the Internet Gateway |
| `AWS::EC2::NetworkAcl` | Custom NACL attached to the subnet |
| `AWS::EC2::SecurityGroup` | Inbound rule allowing HTTP on port 80 |
| `AWS::EC2::Instance` | t2.micro running a web server |

The stack deployed without errors. The instance is running and the web server is active.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload `lab-10-nacl.yaml`
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-10`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `WebPageURL` and `InstancePublicIP`

## The Problem

Click the `WebPageURL` link from the stack Outputs tab.

**Expected**: The Broken Labs success page loads.
**Actual**: The browser times out — `ERR_CONNECTION_TIMED_OUT`.

The instance is running and the web server is active. The security group and route table
are correctly configured. The custom Network ACL is the place to investigate.

## Fix the Lab

Network ACLs are **stateless** — unlike security groups, they do not automatically allow
return traffic. Every direction of traffic must be explicitly permitted. Look closely at
both the inbound and outbound rules of the NACL and consider what traffic each direction
needs to allow for HTTP to work end-to-end.

After applying the fix, reload the `WebPageURL` in your browser to confirm the page loads.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE**

## Resources

- [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
- [Ephemeral ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports)
- [Compare security groups and network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html#vpc-security-groups-and-network-acls)
- [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Subnets for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html)
- [Internet gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- [VPC route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
