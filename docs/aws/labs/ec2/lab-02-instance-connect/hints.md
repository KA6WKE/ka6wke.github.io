---
layout: default
---

# Hints — EC2 Instance Connect - Lab 02

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — What Instance Connect needs</summary>

EC2 Instance Connect works by sending a temporary SSH public key to the instance over the
EC2 API, then opening an SSH connection from your browser to the instance.

That SSH connection travels over the network — specifically over **TCP port 22**. Something
is preventing it from reaching the instance.

Check the security group attached to your instance.

</details>

---

<details markdown="1">
<summary>Hint 2 — Read the security group rules</summary>

Navigate to the EC2 console, select your instance, and open the **Security** tab.
Click on the security group and check the **Inbound rules**.

What ports are currently allowed? Is port 22 (SSH) listed?

Note that port 80 is open (which is why the web page loads) — but web traffic and
SSH are handled on separate ports.

</details>

---

<details markdown="1">
<summary>Hint 3 — What needs to be added</summary>

Instance Connect requires inbound TCP port 22 to be open in the security group.
Even though the web server is reachable on port 80, SSH on port 22 is completely
separate and must be explicitly allowed.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The security group has an inbound rule for port 80 (HTTP) but no rule for
port 22 (SSH). EC2 Instance Connect connects over SSH on port 22 — without that rule,
the connection is silently dropped at the security group. The web server on port 80 is
unaffected because that rule exists.

---

**To fix:**

1. Open the [EC2 console](https://console.aws.amazon.com/ec2) and go to **Security Groups**
2. Select the security group named `brokenlabs-ec2-lab-02-sg`
3. Click **Edit inbound rules**
4. Click **Add rule**
5. Set **Type** to `SSH` (port 22 fills in automatically), **Source** to `0.0.0.0/0`
6. Click **Save rules**
7. Go back to your instance, click **Connect** > **EC2 Instance Connect** > **Connect**
8. A browser terminal should open

</details>
