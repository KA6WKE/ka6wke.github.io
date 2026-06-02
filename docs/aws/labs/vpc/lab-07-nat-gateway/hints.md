---
layout: default
---

# Hints — VPC NAT Gateway - Lab 07

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Start with the route table</summary>

Connect to the instance via Session Manager and run:

```bash
curl --max-time 5 https://checkip.amazonaws.com
```

The connection times out. Now open the [VPC console](https://console.aws.amazon.com/vpc)
and go to **Route tables**. Find `brokenlabs-vpc-lab-07-private-rt` and click the
**Routes** tab.

What is listed for `0.0.0.0/0`? Is there a route? Does it point to something?

> **Cleanup reminder**: Any services created outside of CloudFormation MUST be deleted manually before deleting the CloudFormation stack.

</details>

---

<details markdown="1">
<summary>Hint 2 — The NAT Gateway</summary>

The private route table has a `0.0.0.0/0` route pointing to a NAT Gateway. The NAT
Gateway exists and its status shows **Available**.

Open VPC > **NAT gateways** and select the NAT Gateway named `brokenlabs-vpc-lab-07-nat`.

Look at the **Details** tab. Which **subnet** is it in? Is that what you would expect
for a NAT Gateway?

> **Cleanup reminder**: Any services created outside of CloudFormation MUST be deleted manually before deleting the CloudFormation stack.

</details>

---

<details markdown="1">
<summary>Hint 3 — Where NAT Gateways belong</summary>

A NAT Gateway must be placed in a **public subnet** — a subnet whose route table has a
`0.0.0.0/0 → Internet Gateway` route. That route is what gives the NAT Gateway its own
path to the internet.

A NAT Gateway in a **private subnet** has no internet access itself. When the private
instance sends traffic to it, the NAT Gateway cannot forward that traffic anywhere — it
is a dead end.

To fix this:

1. Create a new NAT Gateway in the **public** subnet:
   - Open [VPC console](https://console.aws.amazon.com/vpc) > **NAT gateways** > **Create NAT gateway**
   - Name: `brokenlabs-vpc-lab-07-nat-fix`
   - Connectivity type: **Public**
   - Availability Mode: **Zonal**
   - Subnet: select `brokenlabs-vpc-lab-07-public-subnet`
   - Click **Allocate Elastic IP**, then click **Create NAT gateway**
   - Wait for the status to become **Available**

   - **Save changes**

> **Cleanup reminder**: The NAT Gateway you create to fix this lab is outside of CloudFormation — delete it manually before deleting the stack.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The NAT Gateway is in the **private subnet**. A NAT Gateway placed in a
private subnet has no route to the Internet Gateway, so it cannot forward outbound traffic.
The private instance's default route points to the NAT Gateway, but the NAT Gateway itself
is a dead end.

---

**To fix:**

1. **Create a new NAT Gateway in the public subnet** — Open the [VPC console](https://console.aws.amazon.com/vpc) > **NAT gateways** > **Create NAT gateway**
   - Name: `brokenlabs-vpc-lab-07-nat-fix`
   - Connectivity type: **Public**
   - Availability Mode: **Zonal**
   - Subnet: select `brokenlabs-vpc-lab-07-public-subnet`
   - Click **Allocate Elastic IP**, then click **Create NAT gateway**
   - Wait for the status to become **Available**

2. **Update the private route table** — Go to **Route tables** and select `brokenlabs-vpc-lab-07-private-rt`
   - Click the **Routes** tab > **Edit routes**
   - Change the `0.0.0.0/0` target from the old NAT Gateway to the new one
   - Click **Save changes**

   > **Note**: The original NAT Gateway (`brokenlabs-vpc-lab-07-nat`) was created by CloudFormation — do **not** delete it manually. Deleting a CloudFormation-managed resource outside of CloudFormation will cause the stack deletion to fail. It will be removed automatically when the stack is deleted.

3. **Verify** — Reconnect to the instance via Session Manager and run:

   ```bash
   curl --max-time 5 https://checkip.amazonaws.com
   ```

   The public IP of the new NAT Gateway is returned — the instance now has internet access.

> **Note**: Only one NAT Gateway is needed. In a real production environment you would fix
> the CloudFormation template itself — changing the NAT Gateway's subnet from private to
> public — and redeploy. CloudFormation would replace the misplaced NAT Gateway with a
> correctly placed one, leaving a single working NAT Gateway. In this lab the console-only
> constraint means creating a second one manually; the original is removed when the stack
> is deleted.

---

**Before deleting the stack**, see the Cleanup section below.

</details>

---

<details markdown="1">
<summary>Cleanup</summary>

Two NAT Gateways are running after the fix: the original (`brokenlabs-vpc-lab-07-nat`,
created by CloudFormation) and the one you created (`brokenlabs-vpc-lab-07-nat-fix`).
Only the fix one needs manual cleanup — the original is deleted with the stack.

Clean up in this order to avoid ongoing charges:

1. Open the [VPC console](https://console.aws.amazon.com/vpc) > **NAT gateways**
2. Select `brokenlabs-vpc-lab-07-nat-fix` and click **Actions** > **Delete NAT gateway** — wait for deletion
3. Go to **Elastic IPs**, select the EIP you allocated for the fix NAT gateway, and click **Release Elastic IP address**
4. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**
5. Wait for the stack to reach **DELETE_COMPLETE** — this removes the original NAT Gateway, its EIP, and all other lab resources

</details>
