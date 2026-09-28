---
layout: default
title: "Lab 03: Web Access II"
lab_level: associate
lab_service: ec2
lab_number: "03"
redirect_from: /docs/aws/labs/ec2/lab-03-no-public-ip/
---

# Lab 03 - Web Access II

> **Difficulty**: Beginner
> **Service**: Amazon EC2

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible) and allocates one Elastic IP,
> which is billed at about **$0.005/hour (~$0.12/day)** and is not covered by the Free Tier.
> If left running outside the Free Tier, the total cost is at most about **$0.42/day**.
> Complete the lab and delete the stack promptly.

## Scenario

Your team deployed a web server on EC2. The stack completed successfully and the instance is
running — but the `WebPageURL` in the stack Outputs doesn't load.

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::EC2::VPC` | Dedicated VPC for the lab |
| `AWS::EC2::Subnet` | Subnet for the instance |
| `AWS::EC2::InternetGateway` | Internet gateway attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a default route to the internet |
| `AWS::EC2::SecurityGroup` | Allows inbound traffic on ports 80 and 22 |
| `AWS::EC2::EIP` | An Elastic IP address |
| `AWS::EC2::Instance` | t2.micro running Amazon Linux 2023 with a web server |

The stack deployed without errors. Apache is running on the instance.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-03-web-access-ii.yaml" download>lab-03-web-access-ii.yaml</a>
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

The instance is running and shows a healthy status in the EC2 console.

## Fix the Lab

Diagnose why the problem occurs and fix it so the result matches **Expected** above.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

> **Note**: If you created any resources outside the stack while fixing the lab, or attached
> anything to the stack's resources by hand, remove those first. The full solution in
> [hints.md](hints.md) lists any extra cleanup steps.

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)
4. Verify under **Elastic IPs** that no address from this lab remains

## Resources

- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)

