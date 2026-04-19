# Lab 06 - EC2 User Data

> **Difficulty**: Intermediate
> **Service**: Amazon EC2

> **Cost**: This lab uses a t2.micro instance (Free Tier eligible). If left running outside the Free Tier,
> the cost is approximately **$0.30/day**. Delete the stack when you are done.

## Scenario

Your team deployed a web server on EC2 using a startup script in the instance user data.
The stack completed successfully, the instance passed all health checks, and the security
group allows traffic on port 80. But when you open the URL, the browser refuses the
connection. The instance looks healthy — so why isn't Apache running?

## What Was Deployed

| Resource | Purpose |
|----------|---------|
| `AWS::EC2::VPC` | Dedicated VPC for the lab |
| `AWS::EC2::Subnet` | Public subnet with internet access |
| `AWS::EC2::InternetGateway` | Internet gateway attached to the VPC |
| `AWS::EC2::RouteTable` | Route table with a default route to the internet |
| `AWS::EC2::SecurityGroup` | Allows inbound traffic on ports 80 and 22 |
| `AWS::IAM::Role` | Instance role with `AmazonSSMManagedInstanceCore` |
| `AWS::IAM::InstanceProfile` | Attaches the role to the instance |
| `AWS::EC2::Instance` | t2.micro running Amazon Linux 2023 |

The stack deployed without errors. The security group and networking are correct.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload `lab-06-user-data.yaml`
4. Enter a stack name (e.g., `brokenlabs-ec2-lab-06`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE** (takes 2–3 minutes)
6. Open the stack **Outputs** tab — you will see `InstanceId`, `InstancePublicIP`, and `WebPageURL`

## The Problem

Open the `WebPageURL` from the stack Outputs in your browser.

**Expected**: the AWS Broken Labs welcome page loads.
**Actual**: the browser displays:

```
This site can't be reached
ERR_CONNECTION_REFUSED
```

The instance is running and healthy. The security group allows port 80. The URL is correct.
Unlike a timeout (which means traffic is blocked), **connection refused** means the request
reached the instance but nothing is listening on port 80.

Apache was supposed to be installed and started by the user data script — but it never ran
successfully.

## Fix the Lab

Connect to the instance using **Session Manager** (EC2 console → select instance → Connect →
Session Manager → Connect) and investigate why Apache is not running.

Check the cloud-init log for clues:

```
cat /var/log/cloud-init-output.log
```

Once you identify the problem, fix it from the Session Manager terminal.

Need help? Open [hints.md](hints.md) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
2. Wait for the stack to reach **DELETE_COMPLETE** (or disappear from the list)
3. Verify in the [EC2 console](https://console.aws.amazon.com/ec2) that the instance no longer appears (or shows **Terminated**)
4. Verify in [IAM → Roles](https://console.aws.amazon.com/iam/home#/roles) that the lab role no longer appears

## Resources

- [Run commands at launch with EC2 user data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)
- [Troubleshoot EC2 user data](https://repost.aws/knowledge-center/execute-user-data-ec2)
- [Amazon Linux 2023 package management](https://docs.aws.amazon.com/linux/al2023/ug/package-management.html)
