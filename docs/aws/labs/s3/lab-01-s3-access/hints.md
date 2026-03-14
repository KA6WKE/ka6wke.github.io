---
layout: default
---

# Hints — S3 Access - Lab 01

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

Open the S3 console, navigate to your bucket, and examine the **Permissions** tab.

</details>

---

<details markdown="1">
<summary>Hint 2 — What to look for</summary>

On the **Permissions** tab, look at two things side by side:

- The **Block public access (bucket settings)** section
- The **Bucket policy** section

What is in the Bucket policy section? What do the Block Public Access settings show?

</details>

---

<details markdown="1">
<summary>Hint 3 — The conflict</summary>

The **Bucket policy** section is empty — there is no policy granting public access.

One of the four Block Public Access flags is enabled: **Block public policy**
(`BlockPublicPolicy: true`). When this flag is on, S3 rejects any attempt to save a
bucket policy that grants public access. The policy was never able to be applied.

Without a public bucket policy, all unauthenticated requests return 403 Access Denied.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: `BlockPublicPolicy` is set to `true`, which prevented a public bucket
policy from being saved. Without the policy, there is nothing granting public read
access to objects in the bucket.

---

### Fix via AWS Console

#### Step 1 — Disable Block public policy

1. Open the [S3 console](https://console.aws.amazon.com/s3) and select your bucket
2. Click the **Permissions** tab
3. Under **Block public access (bucket settings)**, click **Edit**
4. Uncheck **Block public policy**
5. Click **Save changes**, type `confirm`, and click **Confirm**

#### Step 2 — Add the bucket policy

1. On the **Permissions** tab, scroll to **Bucket policy** and click **Edit**
2. Paste the following policy, replacing `BUCKET-NAME` with your bucket name:

<div style="position: relative; margin: 12px 0;"><button onclick="var b=this;navigator.clipboard.writeText(document.getElementById('lab01-bucket-policy').textContent).then(function(){b.textContent='Copied!';setTimeout(function(){b.textContent='Copy'},1500)})" style="position: absolute; top: 6px; right: 6px; padding: 3px 10px; font-size: 12px; cursor: pointer; border: 1px solid #aaa; border-radius: 3px; background: #f6f8fa;">Copy</button><pre><code id="lab01-bucket-policy">{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::BUCKET-NAME/*"
    }
  ]
}</code></pre></div>

1. Click **Save changes**
2. Retry the `BucketURL` from the stack Outputs — it should now load

</details>
