---
layout: default
---

# Hints — Network ACL - Lab 10

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Check the usual suspects</summary>

The page times out. Before looking at the NACL, verify the basics are in order:

- **Security group**: Open the [EC2 console](https://console.aws.amazon.com/ec2) >
  **Security Groups** > select `brokenlabs-vpc-lab-10-sg` > **Inbound rules** tab.
  Is port 80 allowed?
- **Route table**: Open the [VPC console](https://console.aws.amazon.com/vpc) >
  **Route tables** > select `brokenlabs-vpc-lab-10-rt` > **Routes** tab.
  Is there a `0.0.0.0/0 → igw` route?

Both should look correct. The problem is in the Network ACL.

</details>

---

<details markdown="1">
<summary>Hint 2 — The Network ACL</summary>

Open the [VPC console](https://console.aws.amazon.com/vpc) > **Network ACLs** > select
`brokenlabs-vpc-lab-10-nacl`.

Click the **Inbound rules** tab — there should be a rule allowing TCP port 80.

Now click the **Outbound rules** tab. What port range does the outbound rule allow?

Consider: when a client connects to port 80 on the server, where does the server send
its response? Is the outbound rule covering that traffic?

</details>

---

<details markdown="1">
<summary>Hint 3 — Stateless vs. stateful</summary>

**Security groups are stateful**: if an inbound rule allows traffic on port 80, the
return traffic is automatically permitted — no outbound rule needed.

**Network ACLs are stateless**: each direction is evaluated independently with no memory
of established connections. Both directions must be explicitly allowed.

When a client (browser) connects to port 80:
- The **inbound** NACL rule needs to allow traffic destined for port 80 ✓
- The **outbound** NACL rule needs to allow the response going back to the client

The response does NOT go back to port 80 on the client. It goes to the client's
**ephemeral source port** — the temporary high-numbered port the client randomly chose
when opening the connection (typically in the range `1024–65535`).

The current outbound rule only allows port 80. The response is dropped.

To fix, add an outbound rule in the [VPC console](https://console.aws.amazon.com/vpc) >
**Network ACLs** > select `brokenlabs-vpc-lab-10-nacl` > **Outbound rules** tab >
**Edit outbound rules** > **Add new rule**:

- Rule number: `200`
- Type: **Custom TCP**
- Port range: `1024-65535`
- Destination: `0.0.0.0/0`
- Allow/Deny: **Allow**

Click **Save changes**.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The NACL outbound rule only permits TCP port 80. HTTP response traffic
goes back to the client's ephemeral source port (1024–65535), not to port 80. NACLs are
stateless — return traffic must be explicitly allowed. The SYN-ACK is dropped before
the TCP handshake completes, causing `ERR_CONNECTION_TIMED_OUT`.

---

**To fix:**

1. **Add an outbound rule for ephemeral ports** — Open the [VPC console](https://console.aws.amazon.com/vpc) > **Network ACLs**
   - Select `brokenlabs-vpc-lab-10-nacl`
   - Click the **Outbound rules** tab > **Edit outbound rules** > **Add new rule**
   - Rule number: `200`
   - Type: **Custom TCP**
   - Port range: `1024-65535`
   - Destination: `0.0.0.0/0`
   - Allow/Deny: **Allow**
   - Click **Save changes**

2. **Verify** — Reload the `WebPageURL` in your browser.

   The Broken Labs success page loads — HTTP requests can now complete end-to-end.

> **Note**: In a production environment, the existing outbound Rule 100 (port 80) can also
> be removed. A web server receiving inbound connections does not initiate outbound HTTP
> requests — the only outbound traffic it generates is return traffic to clients, which
> uses ephemeral ports. Rule 200 (1024–65535) is the only outbound rule needed.

</details>
