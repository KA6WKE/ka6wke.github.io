---
layout: default
---

# Hints — VPC Route Table - Lab 04

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Check the usual suspects</summary>

Work through the layers in order:

- **Network ACL**: No custom NACL is in use. The subnet uses the default NACL, which allows
  all traffic in both directions.
- **Security group**: The inbound rules allow TCP port 80 from anywhere. That's correct.
- **Internet Gateway**: The IGW is created and attached to the VPC.
- **Route table**: Navigate to [VPC console](https://console.aws.amazon.com/vpc) > **Route tables**.
  Find the route table named `brokenlabs-vpc-lab-04-rt`. Does it have a route to the Internet
  Gateway?

</details>

---

<details markdown="1">
<summary>Hint 2 — The route table</summary>

The custom route table has a `0.0.0.0/0` route pointing to the Internet Gateway. That looks
correct.

But a route table only controls traffic for the subnets that are associated with it. If a
subnet is not explicitly associated with a route table, it uses the **VPC main route table**
by default — and that table may not have an internet route.

In VPC > **Route tables**, click on `brokenlabs-vpc-lab-04-rt` and open the **Subnet
associations** tab. What subnets are listed under **Explicit subnet associations**?

</details>

---

<details markdown="1">
<summary>Hint 3 — Associations</summary>

The **Subnet associations** tab on the custom route table shows no explicit associations —
the lab subnet is not linked to it.

Because the subnet has no explicit association, it falls back to the VPC's **main route
table**. Click on the route table marked **(main)** in the route tables list and check its
routes. Does it have a `0.0.0.0/0` route?

The main route table has only the local VPC route. There is no internet route — so traffic
from the internet cannot reach the subnet.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The custom route table (`brokenlabs-vpc-lab-04-rt`) has a `0.0.0.0/0` route
to the Internet Gateway, but it is not associated with the lab subnet. The subnet falls back
to the VPC main route table, which has only the local VPC route and no internet route.
Inbound traffic from the internet is dropped before it reaches the subnet.

---

**To fix:**

1. Open the [VPC console](https://console.aws.amazon.com/vpc) and go to **Route tables**
2. Select the route table named `brokenlabs-vpc-lab-04-rt`
3. Click the **Subnet associations** tab, then click **Edit subnet associations**
4. Check the box next to `brokenlabs-vpc-lab-04-subnet`
5. Click **Save associations**
6. Reload the `WebPageURL` in your browser — the AWS Broken Labs page should appear

</details>
