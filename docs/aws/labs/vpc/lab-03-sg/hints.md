---
layout: default
---

# Hints — VPC Security Group - Lab 03

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Start with the basics</summary>

The Internet Gateway is attached to the VPC. The route table has a `0.0.0.0/0` route pointing
to that gateway. No custom Network ACL is in use — the subnet uses the default NACL, which
allows all traffic in both directions.

The instance is running and the web server is active. The routing path from the internet to
the subnet is clear.

What controls inbound traffic at the instance level?

</details>

---

<details markdown="1">
<summary>Hint 2 — The instance-level firewall</summary>

In AWS, every EC2 instance has a **security group** attached to it. Security groups act as a
stateful firewall — they control what traffic is allowed in and out of the instance.

Navigate to the [EC2 console](https://console.aws.amazon.com/ec2) and go to **Security Groups**
in the left navigation.

Find the security group named `brokenlabs-vpc-lab-03-sg`. Click on it and open the
**Inbound rules** tab.

What port is listed in the inbound rule?

</details>

---

<details markdown="1">
<summary>Hint 3 — The port number</summary>

The inbound rule in the security group allows TCP traffic on a specific port. HTTP traffic —
the kind a browser sends when you type `http://` — uses **port 80** by default.

Look at the port number in the inbound rule. Is it port 80?

If the port in the rule does not match the port the web server listens on, traffic will never
reach the server.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The security group inbound rule allows TCP traffic on port **8080**, but the
web server listens on port **80**. Port 80 is never opened — inbound HTTP requests are silently
dropped at the security group before they reach the instance.

---

**To fix:**

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) and go to **Security Groups**
2. Select the security group named `brokenlabs-vpc-lab-03-sg`
3. Click the **Inbound rules** tab, then click **Edit inbound rules**
4. Find the rule for port **8080** and change the port range to **80**
5. Click **Save rules**
6. Reload the `WebPageURL` in your browser — the AWS Broken Labs page should appear

</details>
