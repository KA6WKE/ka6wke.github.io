---
layout: default
---

# Hints — VPC Network ACL - Lab 02

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to start</summary>

The EC2 instance is running. The security group allows HTTP on port 80. The route table
has a `0.0.0.0/0` route to the Internet Gateway. All of those things check out.

In a VPC, security groups are not the only layer that can filter traffic. There is a
second, separate filtering mechanism that operates at the subnet level rather than the
instance level.

Navigate to the [VPC console](https://console.aws.amazon.com/vpc) and look beyond route
tables and security groups.

</details>

---

<details markdown="1">
<summary>Hint 2 — A second layer</summary>

In the [VPC console](https://console.aws.amazon.com/vpc), look at **Network ACLs** in the
left navigation.

Find the Network ACL associated with the subnet named `brokenlabs-vpc-lab-02-subnet`.
Click on it and open the **Inbound rules** tab.

How many rules are listed for port 80? What are their rule numbers and actions?

</details>

---

<details markdown="1">
<summary>Hint 3 — Examine the rules</summary>

The inbound rules contain two entries for port 80:

| Rule # | Type | Port | Source | Allow/Deny |
|--------|------|------|--------|------------|
| 90 | HTTP | 80 | 0.0.0.0/0 | DENY |
| 100 | HTTP | 80 | 0.0.0.0/0 | ALLOW |

Unlike security groups, Network ACL rules are evaluated in **numerical order** — lowest
number first. When a packet matches a rule, evaluation stops immediately and that rule's
action is applied.

Given that, which rule actually applies to inbound HTTP traffic?

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The Network ACL has a DENY rule at number 90 for TCP port 80, which fires
before the ALLOW rule at number 100. Network ACLs process rules in ascending numerical
order and stop at the first match — so rule 90 (DENY) is always reached before rule 100
(ALLOW). HTTP traffic is silently dropped at the subnet boundary before it ever reaches
the security group or the instance.

---

**To fix:**

1. Open the [VPC console](https://console.aws.amazon.com/vpc) and go to **Network ACLs**
2. Select the ACL named `brokenlabs-vpc-lab-02-nacl`
3. Click the **Inbound rules** tab, then click **Edit inbound rules**
4. Find rule **90** (DENY, port 80) and click **Remove**
5. Click **Save changes**
6. Reload the `WebPageURL` in your browser — the AWS Broken Labs page should appear

</details>
