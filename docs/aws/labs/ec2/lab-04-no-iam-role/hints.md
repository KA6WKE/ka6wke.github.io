# Hints — EC2 SSM Session Manager - Lab 04

Open each hint only after you've spent time investigating on your own.

---

<details>
<summary>Hint 1 — What Session Manager needs</summary>

Session Manager connects to EC2 instances through the SSM agent running on the instance.
The agent communicates with the AWS Systems Manager service — and to do that, the instance
needs AWS credentials.

EC2 instances get their AWS credentials through an **IAM instance profile**. An instance
profile is a container for an IAM role that EC2 can attach to an instance at launch (or
afterward).

In the EC2 console, select your instance and check the **Security** tab. What does the
**IAM Role** field show?

</details>

---

<details>
<summary>Hint 2 — Create a role for EC2</summary>

The instance has no IAM role. You need to:

1. Create an IAM role that **EC2 can assume** (trusted entity: EC2)
2. Attach the `AmazonSSMManagedInstanceCore` managed policy
3. Attach the role to the running instance

In the IAM console, go to **Roles → Create role**. Under **Trusted entity type**, select
**AWS service**, then choose **EC2** from the use case list.

</details>

---

<details>
<summary>Hint 3 — Attach the role to the running instance</summary>

You can attach an IAM role to an already-running EC2 instance — you do not need to stop it
or redeploy.

After creating the role in IAM, go to:
EC2 → Instances → select your instance → **Actions** → **Security** → **Modify IAM role**

Select the role you just created and click **Update IAM role**. Wait about 1 minute, then
retry Session Manager. If the **Connect** button remains grayed out, reboot the instance
(Actions → Instance State → Reboot instance) and try again after 1–2 minutes.

</details>

---

<details>
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The instance was deployed without an IAM instance profile. Without an attached
role, the SSM agent on the instance cannot authenticate with AWS Systems Manager. The agent
has no credentials to make the API calls required for Session Manager to function, so the
instance never registers as a managed instance.

---

**To fix:**

1. Open the [IAM console](https://console.aws.amazon.com/iam) and go to **Roles → Create role**
2. Under **Trusted entity type**, select **AWS service**
3. Under **Use case**, select **EC2**, then click **Next**
4. Search for `AmazonSSMManagedInstanceCore`, select it, and click **Next**
5. Give the role a name (e.g., `brokenlabs-ec2-lab-04-fix`) and click **Create role**
6. Open the [EC2 console](https://console.aws.amazon.com/ec2), select your instance
7. Click **Actions** → **Security** → **Modify IAM role**
8. Select the role you just created and click **Update IAM role**
9. Wait approximately 1 minute, then click **Connect** → **Session Manager** → **Connect**
10. If the **Connect** button remains grayed out, reboot the instance (Actions → Instance State → Reboot instance) and wait 1–2 minutes before retrying

---

**Before deleting the stack:**

The role you created is not managed by CloudFormation. Before deleting the stack:
1. Go to EC2 → Actions → Security → Modify IAM role → select **No IAM role** → Update
2. Delete the stack
3. Go to IAM → Roles → delete the role you created

</details>
