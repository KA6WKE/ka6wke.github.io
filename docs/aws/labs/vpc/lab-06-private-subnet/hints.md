---
layout: default
---

# Hints — VPC NAT Gateway - Lab 06

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — What the instance can and cannot do</summary>

The instance is in a **private subnet** — it has no public IP address and is not directly
reachable from the internet.

Session Manager works because it uses **VPC interface endpoints** that allow the SSM agent
to communicate with AWS services without leaving the VPC. That traffic never touches the
internet.

But when the instance tries to reach an external host (like `checkip.amazonaws.com`), it
needs a route to the internet. Navigate to the [VPC console](https://console.aws.amazon.com/vpc)
and look at the **Route tables**. Find the private route table
(`brokenlabs-vpc-lab-06-private-rt`) and check its routes. What routes are listed?

> **Cleanup reminder**: Any services created outside of Cloud Formation MUST be deleted manually before deleting the Cloud Formation stack.

</details>

---

<details markdown="1">
<summary>Hint 2 — Why a direct IGW route won't work</summary>

The public subnet has a `0.0.0.0/0` route to the Internet Gateway — and that works because
instances in the public subnet have public IP addresses. The IGW performs the 1:1 NAT between
the public IP and the private IP.

A private subnet instance has **no public IP**. Even if you added a `0.0.0.0/0 → IGW` route
to the private route table, the traffic would be dropped — there is no public IP to translate
outbound packets to.

Private instances need a different component to reach the internet — one that sits in the
public subnet and performs outbound NAT on behalf of the private instances.

> **Cleanup reminder**: Any services created outside of Cloud Formation MUST be deleted manually before deleting the Cloud Formation stack.

</details>

---

<details markdown="1">
<summary>Hint 3 — The missing component</summary>

A **NAT Gateway** is the component that enables private instances to initiate outbound
connections to the internet while remaining unreachable from the internet.

- The NAT Gateway lives in the **public subnet** (where it has internet access via the IGW)
- It is assigned an **Elastic IP** (a static public IP address)
- The private route table gets a `0.0.0.0/0` route pointing to the NAT Gateway

To create one in the [VPC console](https://console.aws.amazon.com/vpc):

   1. Name: `brokenlabs-vpc-lab-06-nat`
   2. Connectivity type: **Public**
   3. Availability Mode: **Zonal**
   4. Subnet: select `brokenlabs-vpc-lab-06-public-subnet`
   5. Click **Allocate Elastic IP**, then click **Create NAT gateway**
   6. Wait for the status to become **Available**

> **Cleanup reminder**: The NAT Gateway you create to fix this lab is outside of CloudFormation — delete it manually before deleting the stack.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The private subnet's route table has no route to the internet. Private
instances cannot initiate outbound connections without a NAT Gateway in the public subnet.

---

**To fix:**

1. **Create a NAT Gateway** — Open the [VPC console](https://console.aws.amazon.com/vpc) > **NAT gateways** > **Create NAT gateway**
   - Name: `brokenlabs-vpc-lab-06-nat`
   - Connectivity type: **Public**
   - Availability Mode: **Zonal**
   - Subnet: select `brokenlabs-vpc-lab-06-public-subnet`
   - Click **Allocate Elastic IP**, then click **Create NAT gateway**
   - Wait for the status to become **Available**

2. **Add a route to the private route table** — Go to **Route tables** and select `brokenlabs-vpc-lab-06-private-rt`
   - Click the **Routes** tab > **Edit routes** > **Add route**
   - Destination: `0.0.0.0/0`
   - Target: select **NAT Gateway**, then select the NAT gateway you just created
   - Click **Save changes**

3. **Verify** — Reconnect to the instance via Session Manager and run:

   ```bash
   curl --max-time 5 https://checkip.amazonaws.com
   ```

   The public IP of the NAT Gateway is returned — the instance now has internet access

---

**Before deleting the stack**, see the Cleanup section below.

</details>

---

<details markdown="1">
<summary>Cleanup</summary>

The NAT Gateway you created is outside of CloudFormation and will not be removed when the
stack is deleted. Clean up in this order to avoid ongoing charges:

1. Open the [VPC console](https://console.aws.amazon.com/vpc) > **NAT gateways**
2. Select your NAT gateway and click **Actions** > **Delete NAT gateway** — wait for deletion
3. Go to **Elastic IPs**, select the EIP used by the NAT gateway, and click **Release Elastic IP address**
4. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
5. Wait for the stack to reach **DELETE_COMPLETE**

</details>
