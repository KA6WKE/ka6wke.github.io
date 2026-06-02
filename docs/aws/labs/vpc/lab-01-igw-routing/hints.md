---
layout: default
---

# Hints — VPC Internet Gateway Routing - Lab 01

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

The EC2 instance is running and the security group already allows HTTP on port 80. The
Internet Gateway exists and is attached to the VPC. So the problem is not in any of those
places.

In a VPC, having an Internet Gateway attached is not enough on its own. The VPC also needs
to know *when* to use it. What component tells a VPC where to send traffic destined for
addresses outside the VPC?

Navigate to the [VPC console](https://console.aws.amazon.com/vpc) and look at **Route Tables**.

</details>

---

<details markdown="1">
<summary>Hint 2 — Inspect the route table</summary>

In the VPC console, go to **Route Tables** and find the route table named
`brokenlabs-vpc-lab-01-rt`.

Click on it and open the **Routes** tab. Review the list of routes.

What destinations are listed? Is there a route for traffic going *outside* the VPC
(i.e., to the internet)?

</details>

---

<details markdown="1">
<summary>Hint 3 — What is missing</summary>

The route table only has one route:

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | local  |

The `local` route handles traffic *within* the VPC. But there is no route for anything
outside the VPC — no entry for `0.0.0.0/0` pointing to the Internet Gateway.

Without that route, the VPC has no path to send internet traffic through the IGW, even
though the IGW exists and is attached. Traffic from your browser reaches the IGW but the
VPC has no instruction for where to forward it next.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The route table associated with the public subnet has no default route to
the Internet Gateway. An Internet Gateway must be both *attached* to the VPC and *referenced
in a route table* before it can carry traffic. The missing `0.0.0.0/0 → igw` route means
the VPC silently drops all inbound and outbound internet traffic.

---

**To fix:**

1. Open the [VPC console](https://console.aws.amazon.com/vpc) and go to **Route Tables**
2. Select the route table named `brokenlabs-vpc-lab-01-rt`
3. Click the **Routes** tab, then click **Edit routes**
4. Click **Add route**
5. Set **Destination** to `0.0.0.0/0`
6. Set **Target** to **Internet Gateway**, then select `brokenlabs-vpc-lab-01-igw` from the list
7. Click **Save changes**
8. Reload the `WebPageURL` in your browser — the AWS Broken Labs page should appear

</details>
