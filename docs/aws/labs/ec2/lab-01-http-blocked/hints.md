---
layout: default
---

# Hints — EC2 Security Groups - Lab 01

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

The instance is running and Apache is installed. The browser request never even reaches the
server — it times out completely.

In EC2, all inbound traffic is blocked by default unless explicitly allowed. What controls
inbound traffic to an EC2 instance?

Navigate to the EC2 console, select your instance, and look at the **Security** tab.

</details>

---

<details markdown="1">
<summary>Hint 2 — Read the security group rules</summary>

Click on the security group attached to your instance. Open the **Inbound rules** tab.

What ports are currently allowed? Is port 80 (HTTP) listed?

</details>

---

<details markdown="1">
<summary>Hint 3 — What needs to be added</summary>

Security groups are deny-by-default. Traffic on port 80 (HTTP) must be explicitly allowed
before web browsers can reach your server.

You need to add an inbound rule that allows TCP traffic on port 80.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The security group attached to the instance has no inbound rule for port 80 (HTTP).
EC2 security groups block all inbound traffic by default — only ports with explicit allow rules
can receive traffic. The browser request times out because it is silently dropped at the security
group before reaching Apache.

---

**To fix:**

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) and go to **Security Groups**
2. Select the security group named `brokenlabs-ec2-lab-01-sg`
3. Click **Edit inbound rules**
4. Click **Add rule**
5. Set **Type** to `HTTP` (port 80 fills in automatically), **Source** to `0.0.0.0/0`
6. Click **Save rules**
7. Reload the `WebPageURL` in your browser — the AWS Broken Labs page should appear

</details>
