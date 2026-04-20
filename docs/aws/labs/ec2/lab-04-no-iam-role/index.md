---
layout: default
title: "Lab 04: EC2 SSM Session Manager"
lab_level: associate
lab_service: ec2
lab_number: "04"
---

# Lab 04 - EC2 SSM Session Manager

> **Difficulty**: Beginner
> **Service**: Amazon EC2, AWS IAM, AWS Systems Manager

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2. The website loads fine. But when you try to connect
to the instance using Session Manager, the option is unavailable.

## What Was Deployed

| Resource                    | Purpose                                                   |
| --------------------------- | --------------------------------------------------------- |
| `AWS::EC2::VPC`             | Dedicated VPC for the lab                                 |
| `AWS::EC2::Subnet`          | Public subnet with internet access                        |
| `AWS::EC2::InternetGateway` | Internet gateway attached to the VPC                      |
| `AWS::EC2::RouteTable`      | Route table with a default route to the internet          |
| `AWS::EC2::SecurityGroup`   | Allows inbound traffic on ports 80 and 22                 |
| `AWS::EC2::Instance`        | t2.micro running Amazon Linux 2023 with Apache web server |

The stack deployed without errors. The web page loads. The instance has no IAM role.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-04-ec2-ssm-session-manager.yaml" download>lab-04-ec2-ssm-session-manager.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-ec2-lab-04`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `InstancePublicIP`, and `WebPageURL`

## The Problem

First, confirm the web page loads — open `WebPageURL` from the Outputs. The AWS Broken Labs
page displays correctly.

Now try to connect using Session Manager:

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) and select your instance
2. Click **Connect** > **Session Manager**

**Expected**: you can click **Connect** to open a browser terminal.
**Actual**: the **Connect** button is grayed out with the message:

```
The instance does not have the required prerequisites to use Session Manager.
```

The web server works. The security group is correct. But you have no way to get a terminal
on this instance.

## Fix the Lab

Session Manager requires an IAM role with the right permissions to be attached to the
instance. Create a role with the required policy and attach it.

> **Note**: After attaching the role, the instance may need to be rebooted before Session
> Manager becomes available. Reboot from the EC2 console: select the instance →
> **Actions** → **Instance State** → **Reboot instance**.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

> **Important**: The IAM role you create as part of the fix is **not managed by CloudFormation**
> and will not be deleted when you delete the stack.
>
> Before deleting the stack, detach the role:
> EC2 → select instance → Actions → Security → Modify IAM role → select **No IAM role** → Update.
>
> Then delete the stack. Finally, delete the IAM role you created:
> IAM → Roles → select the role → Delete.

1. Detach the IAM role from the instance (see note above)
2. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
3. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
4. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)
5. Delete the IAM role you created from the IAM console
6. Verify in [IAM → Roles](https://console.aws.amazon.com/iam/home#/roles) that the role no longer appears

## Resources

- [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
- [Configure instance permissions for Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-instance-profile.html)
- [Attach an IAM role to an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role)
