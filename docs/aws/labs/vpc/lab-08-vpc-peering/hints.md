---
layout: default
---

# Hints — VPC Peering - Lab 08

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Test the connection and check the peering status</summary>

Connect to `brokenlabs-vpc-lab-08-instance-b` via Session Manager and run:

```bash
curl --max-time 5 http://<InstanceAPrivateIP>/
```

The connection times out. Now open the [VPC console](https://console.aws.amazon.com/vpc)
and go to **Peering connections**.

What is the status of `brokenlabs-vpc-lab-08-peering`? A status of **Active** means the
connection between the two VPCs exists. But does a connection alone make traffic flow?

</details>

---

<details markdown="1">
<summary>Hint 2 — The route tables</summary>

A peering connection creates a link between two VPCs, but instances don't automatically
know how to route traffic across it. Each VPC needs a route that says: "to reach the other
VPC's CIDR, send traffic via the peering connection."

Open [VPC console](https://console.aws.amazon.com/vpc) > **Route tables**.

Select `brokenlabs-vpc-lab-08-rt-b` (VPC B's route table) and click the **Routes** tab.
Is there a route for VPC A's CIDR (`10.0.0.0/16`)?

Now check `brokenlabs-vpc-lab-08-rt-a` (VPC A's route table). Is there a route for VPC
B's CIDR (`10.1.0.0/16`)?

</details>

---

<details markdown="1">
<summary>Hint 3 — Routes are required in both directions</summary>

VPC peering routing is not automatic. Routes must be added to **both** VPCs:

- VPC B needs to know how to reach VPC A: `10.0.0.0/16 → peering connection`
- VPC A needs to know how to reach VPC B: `10.1.0.0/16 → peering connection`

If only one side has the route, traffic flows one way but responses can't return — the
connection still fails.

To add a route:

1. Open [VPC console](https://console.aws.amazon.com/vpc) > **Route tables**
2. Select `brokenlabs-vpc-lab-08-rt-b` > **Routes** tab > **Edit routes** > **Add route**
   - Destination: `10.0.0.0/16`
   - Target: **Peering Connection** → select `brokenlabs-vpc-lab-08-peering`
   - Click **Save changes**
3. Select `brokenlabs-vpc-lab-08-rt-a` > **Routes** tab > **Edit routes** > **Add route**
   - Destination: `10.1.0.0/16`
   - Target: **Peering Connection** → select `brokenlabs-vpc-lab-08-peering`
   - Click **Save changes**

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The VPC peering connection is Active, but neither route table has a route
for the peer VPC's CIDR. Traffic has no path to follow across the peering connection.

---

**To fix:**

1. **Add a route in VPC B's route table** — Open the [VPC console](https://console.aws.amazon.com/vpc) > **Route tables** > select `brokenlabs-vpc-lab-08-rt-b`
   - Click the **Routes** tab > **Edit routes** > **Add route**
   - Destination: `10.0.0.0/16`
   - Target: **Peering Connection** → select `brokenlabs-vpc-lab-08-peering`
   - Click **Save changes**

2. **Add a route in VPC A's route table** — Select `brokenlabs-vpc-lab-08-rt-a`
   - Click the **Routes** tab > **Edit routes** > **Add route**
   - Destination: `10.1.0.0/16`
   - Target: **Peering Connection** → select `brokenlabs-vpc-lab-08-peering`
   - Click **Save changes**

3. **Verify** — Reconnect to `brokenlabs-vpc-lab-08-instance-b` via Session Manager and run:

   ```bash
   curl --max-time 5 http://<InstanceAPrivateIP>/
   ```

   The web server response is returned — the client can now reach VPC A over the private network.

</details>
