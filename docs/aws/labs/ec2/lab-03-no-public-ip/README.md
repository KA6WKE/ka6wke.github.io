# Lab 03 - EC2 Elastic IP

> **Difficulty**: Beginner
> **Service**: Amazon EC2

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible) and allocates one Elastic IP.
> An **unassociated Elastic IP costs ~$0.005/hour (~$0.12/day)** and is not covered by the Free Tier.
> This charge applies from the moment the stack is deployed until the EIP is associated or the stack
> is deleted. Complete the lab and delete the stack promptly.
>
> If left running outside the Free Tier, the total cost is approximately **$0.42/day**
> (instance $0.28 + EBS $0.02 + unassociated EIP $0.12). Once you associate the EIP as part of
> the fix, the EIP charge stops and the cost drops to approximately **$0.30/day**.

## Scenario

Your team deployed a web server on EC2. The stack completed successfully, the instance is
running, and the security group allows traffic on ports 80 and 22. The route table points to
the internet gateway. Everything looks configured correctly — but the `WebPageURL` in the
stack Outputs doesn't load.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::EC2::VPC` | Dedicated VPC for the lab |
| `AWS::EC2::Subnet` | Subnet — note that auto-assign public IP is disabled |
| `AWS::EC2::InternetGateway` | Internet gateway attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a default route to the internet |
| `AWS::EC2::SecurityGroup` | Allows inbound traffic on ports 80 and 22 |
| `AWS::EC2::EIP` | An Elastic IP address |
| `AWS::EC2::Instance` | t2.micro running Amazon Linux 2023 with a web server |

The stack deployed without errors. Apache is running on the instance.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload `lab-03-no-public-ip.yaml`
4. Enter a stack name (e.g., `brokenlabs-ec2-lab-03`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `ElasticIPAddress`, and `WebPageURL`

## The Problem

Open the `WebPageURL` from the stack Outputs in your browser.

**Expected**: the AWS Broken Labs welcome page loads.
**Actual**: the browser displays:

```
This site can't be reached
ERR_CONNECTION_TIMED_OUT
```

The security group, routing, and Apache are all correct. Something else is preventing the
instance from being reachable from the internet.

## Fix the Lab

Investigate the instance's network configuration. Check whether the instance has a public
IP address. Look at the `ElasticIPAddress` Output — what is it associated with?

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

> **Important**: If you associated the Elastic IP as part of the fix, you must **disassociate
> it before deleting the stack**. CloudFormation cannot delete an EIP that has been manually
> associated outside the stack.
>
> To disassociate: EC2 → Elastic IPs → select the EIP → Actions → Disassociate Elastic IP
> address → Disassociate. Then delete the stack.
>
> If you did not associate the EIP, delete the stack directly.

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)
4. Verify under **Elastic IPs** that no address from this lab remains

## Resources

- [Elastic IP addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html)
- [Associate an Elastic IP address](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-instance-addressing-eips-associating)
- [EC2 instance IP addressing](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html)
