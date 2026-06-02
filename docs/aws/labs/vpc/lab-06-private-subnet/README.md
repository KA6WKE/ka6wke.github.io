# Lab 06 - VPC NAT Gateway

> **Difficulty**: Advanced
> **Service**: Amazon VPC

> **Cost**: This lab uses a t2.micro instance plus three VPC interface endpoints.
> Estimated cost if left running: approximately **$1.00/day**. Delete the stack when you are done.
>
> **Note**: Any services created outside of CloudFormation must be deleted manually before deleting the stack.

## Scenario

Your team deployed an EC2 instance in a private subnet for security. The instance is running
and you can connect to it using Session Manager. But the operations team reports that the
instance cannot reach the internet — outbound connections are timing out and package
installations are failing.

The VPC has an Internet Gateway and a correctly configured public subnet. Your job is to
diagnose why the private instance has no outbound internet connectivity and fix it.

## What Was Deployed

| Resource                     | Purpose                                                                 |
| ---------------------------- | ----------------------------------------------------------------------- |
| `AWS::EC2::VPC`              | Custom VPC for the lab (`10.0.0.0/16`)                                  |
| `AWS::EC2::Subnet` (x2)      | Public subnet (`10.0.1.0/24`) and private subnet (`10.0.2.0/24`)        |
| `AWS::EC2::InternetGateway`  | Internet Gateway — created and attached to the VPC                      |
| `AWS::EC2::RouteTable` (x2)  | Public route table (has `0.0.0.0/0 → IGW`) and private route table      |
| `AWS::EC2::VPCEndpoint` (x3) | Interface endpoints for SSM, SSMMessages, EC2Messages                   |
| `AWS::IAM::Role`             | IAM role with `AmazonSSMManagedInstanceCore` for Session Manager access |
| `AWS::EC2::Instance`         | t2.micro in the **private subnet** — no public IP                       |

The stack deployed without errors. The instance is running. Session Manager connectivity
works via the VPC endpoints.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload `lab-06-private-subnet.yaml`
4. Enter a stack name (e.g., `brokenlabs-vpc-lab-06`) and click **Next** > **Next** > **Submit**

   > **IAM notice**: This template creates an IAM role. On the final confirmation page, check
   > the box acknowledging that CloudFormation will create IAM resources, then click **Submit**.

5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 3–5 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId` and `PrivateIP`

## The Problem

Connect to the instance using Session Manager:

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) > **Instances**
2. Select the instance named `brokenlabs-vpc-lab-06`
3. Click **Connect** > **Session Manager** tab > **Connect**

Once connected, test outbound internet access:

```bash
curl --max-time 5 https://checkip.amazonaws.com
```

**Expected**: The public IP address of the instance is returned.
**Actual**: The connection times out — the instance cannot reach the internet.

The instance is healthy and Session Manager works. Only outbound internet access is failing.

## Fix the Lab

The private subnet has a route table but no route to the internet. The public subnet already
has internet access via the Internet Gateway. Think about what component sits between a
private subnet and the internet, and what route the private subnet needs to reach it.

After applying the fix, reconnect via Session Manager and re-run the `curl` command to
confirm the instance can now reach the internet.

Need help? Open [hints.md](hints.md) for progressive hints.

## Resources

- [VPC with public and private subnets](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-example-private-subnets-nat.html)
- [VPC route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Interface VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html)
- [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
- [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
