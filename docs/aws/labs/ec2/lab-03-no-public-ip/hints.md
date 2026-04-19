---
layout: default
---

# Hints — EC2 Elastic IP - Lab 03

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

The error is `ERR_CONNECTION_TIMED_OUT` — the same symptom as a blocked security group.
But the security group in this lab allows port 80. Look at a more fundamental requirement:
for an EC2 instance to be reachable from the internet, it must have a **public IP address**.

In the EC2 console, select your instance and check the **Details** tab. What does
**Public IPv4 address** show?

</details>

---

<details markdown="1">
<summary>Hint 2 — The Elastic IP</summary>

The instance has no public IP. The stack Outputs include an `ElasticIPAddress` — an Elastic IP
was allocated as part of this lab, but it was never associated with the instance.

Navigate to **EC2 → Elastic IPs**. Find the EIP from the Outputs. What does the
**Associated instance** column show?

</details>

---

<details markdown="1">
<summary>Hint 3 — How to associate it</summary>

An Elastic IP must be explicitly associated with an EC2 instance before it can route traffic
to that instance. Allocating the EIP alone is not enough.

In the EC2 console, go to **Elastic IPs**, select the unassociated EIP, and look at the
**Actions** menu.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The subnet has auto-assign public IP disabled, so the instance launched with
only a private IP address. An Elastic IP was allocated by the stack but never associated with
the instance. Without a public IP, the instance is unreachable from the internet — even with
a correctly configured security group and internet gateway.

---

**To fix:**

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) and go to **Elastic IPs**
2. Select the EIP with the address shown in the `ElasticIPAddress` stack Output
3. Click **Actions** → **Associate Elastic IP address**
4. Under **Instance**, select your lab instance
5. Click **Associate**
6. Reload the `WebPageURL` in your browser — the AWS Broken Labs page should appear

---

**Before deleting the stack:**

CloudFormation cannot delete an EIP that was manually associated outside the stack template.
Before deleting the stack, go to **Elastic IPs**, select the EIP, and click **Actions →
Disassociate Elastic IP address**. Then delete the stack.

</details>
