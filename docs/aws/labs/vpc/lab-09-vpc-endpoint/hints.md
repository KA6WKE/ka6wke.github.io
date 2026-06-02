---
layout: default
---

# Hints — VPC Endpoint - Lab 09

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Test S3 connectivity and check the endpoint</summary>

Connect to the instance via Session Manager. Use the `S3TestURL` value from the stack Outputs tab and run:

```bash
curl --max-time 5 <S3TestURL>
```

The connection times out. Now open the [VPC console](https://console.aws.amazon.com/vpc)
and go to **Endpoints**.

Find the endpoint named `brokenlabs-vpc-lab-09-s3-endpoint`. What is its status? What
service does it connect to? What type is it — Interface or Gateway?

</details>

---

<details markdown="1">
<summary>Hint 2 — How Gateway Endpoints work</summary>

A **Gateway Endpoint** does not use a network interface or a private IP address. Instead,
it works by injecting a route into the route tables it is associated with. That route
directs traffic destined for S3 IP addresses through the endpoint instead of the internet.

Select `brokenlabs-vpc-lab-09-s3-endpoint` in the Endpoints list and click the
**Route tables** tab.

Which route table is listed? Now go to **Route tables** and compare:
- What route table is the private subnet using?
- Does that match what the endpoint is associated with?

</details>

---

<details markdown="1">
<summary>Hint 3 — Fix the route table association</summary>

The endpoint is associated with the public route table, not the private one. The private
subnet's route table never received the S3 prefix-list route, so the instance has no path
to S3.

To fix:

1. Select `brokenlabs-vpc-lab-09-s3-endpoint` in the [VPC console](https://console.aws.amazon.com/vpc) > **Endpoints**
2. Click **Actions** > **Manage route tables**
3. Uncheck `brokenlabs-vpc-lab-09-public-rt`
4. Check `brokenlabs-vpc-lab-09-private-rt`
5. Click **Modify route tables**

After saving, verify the routes moved correctly. It may take several console refreshes for the changes to appear — use the browser refresh button if the routes don't update immediately.

- Go to **Route tables** > select `brokenlabs-vpc-lab-09-public-rt` > **Routes** tab — the prefix list entry (`pl-xxxxxxxx`) should be **gone**
- Select `brokenlabs-vpc-lab-09-private-rt` > **Routes** tab — the prefix list entry should now **appear** here

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The S3 Gateway Endpoint is associated with the public route table instead
of the private route table. Gateway Endpoints inject routes only into their associated
route tables — the private subnet received no S3 route, so traffic to S3 has no path and
times out.

---

**To fix:**

1. **Update the endpoint's route table association** — Open the [VPC console](https://console.aws.amazon.com/vpc) > **Endpoints**
   - Select `brokenlabs-vpc-lab-09-s3-endpoint`
   - Click **Actions** > **Manage route tables**
   - Uncheck `brokenlabs-vpc-lab-09-public-rt`
   - Check `brokenlabs-vpc-lab-09-private-rt`
   - Click **Modify route tables**

2. **Verify the routes moved** — Go to **Route tables** (refresh the browser several times if changes are not immediately visible)
   - Select `brokenlabs-vpc-lab-09-public-rt` > **Routes** tab — the prefix list entry (`pl-xxxxxxxx`) should be **gone**
   - Select `brokenlabs-vpc-lab-09-private-rt` > **Routes** tab — the prefix list entry should now **appear** here

3. **Verify connectivity** — Reconnect to the instance via Session Manager and run:

   ```bash
   curl --max-time 5 <S3TestURL>
   ```

   Use the `S3TestURL` value from the stack Outputs tab.

   The command completes immediately — S3 responded. The body may be empty or XML; either
   is fine. If the command **times out** (waits the full 5 seconds), the endpoint is still
   not routing correctly. The lab goal is endpoint connectivity, not S3 authentication.

</details>
