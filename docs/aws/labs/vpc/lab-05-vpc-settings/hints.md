---
layout: default
---

# Hints — VPC Settings - Lab 05

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Look at the Outputs</summary>

Open the **Outputs** tab for your CloudFormation stack. Look at the `WebPageURL` value.

A normal URL looks like:
```
http://ec2-1-2-3-4.compute-1.amazonaws.com/
```

What does your `WebPageURL` show instead? What part of the URL is missing?

</details>

---

<details markdown="1">
<summary>Hint 2 — The instance itself</summary>

The `InstancePublicIP` Output contains the instance's public IP address.

Try opening that address directly in your browser: `http://<InstancePublicIP>/`

Does the page load? If yes, the web server is running correctly. The issue is not with the
instance or the web server — something is preventing the instance from being assigned the
type of address that should appear in `WebPageURL`.

</details>

---

<details markdown="1">
<summary>Hint 3 — VPC-level settings</summary>

EC2 instances receive different types of addresses depending on how the VPC is configured.
A public IP address is assigned when the subnet has auto-assign enabled. Another type of
address — the one missing from the URL — is controlled separately at the VPC level.

Navigate to the [VPC console](https://console.aws.amazon.com/vpc) > **Your VPCs**. Select
`brokenlabs-vpc-lab-05-vpc` and look at the **Details** tab.

Find the settings that relate to how instances in this VPC are addressed. Is everything
enabled?

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The VPC has **DNS hostnames** disabled. When this setting is off, EC2
instances in the VPC are not assigned public DNS hostnames — only a public IP address.
The `WebPageURL` Output used the DNS hostname, which was empty at stack creation time,
producing the broken `http:///` URL.

---

**To fix:**

1. Open the [VPC console](https://console.aws.amazon.com/vpc) and go to **Your VPCs**
2. Select the VPC named `brokenlabs-vpc-lab-05-vpc`
3. Click **Actions** > **Edit VPC settings**
4. Check the box for **Enable DNS hostnames**
5. Click **Save**
6. Navigate to the [EC2 console](https://console.aws.amazon.com/ec2) > **Instances**
7. Select `brokenlabs-vpc-lab-05` and find the **Public IPv4 DNS** field
8. Open `http://<public-ipv4-dns>/` in your browser — the AWS Broken Labs page should appear

> **Note**: The `WebPageURL` in CloudFormation Outputs was captured at stack creation and
> will still show `http:///`. Use the DNS name from the EC2 console directly.

</details>
