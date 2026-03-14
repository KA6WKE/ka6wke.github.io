---
layout: default
---

# Hints — S3 Bucket Policy - Lab 05

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

The bucket exists and the file is in it. The issue is in how access is being controlled.

Open the [S3 console](https://console.aws.amazon.com/s3), navigate to your bucket,
and open the **Permissions** tab. What does the bucket policy say?

</details>

---

<details markdown="1">
<summary>Hint 2 — Read the policy carefully</summary>

A bucket policy can contain more than one statement. Each statement has an `Effect`
of either `Allow` or `Deny`.

How many statements are in this policy? What does each one do?

</details>

---

<details markdown="1">
<summary>Hint 3 — How AWS evaluates policies</summary>

In AWS, an explicit `Deny` always wins — even if another statement in the same
policy grants `Allow` for the same action.

Is there a statement in this policy that could be overriding the `Allow`?

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The bucket policy contains two statements for `s3:GetObject`:
one `Allow` and one `Deny`, both applying to all principals (`*`). AWS always
evaluates an explicit `Deny` before any `Allow` — so the `Deny` wins, and every
request is blocked regardless of the `Allow` statement.

---

**To fix the policy:**

1. Open the [S3 console](https://console.aws.amazon.com/s3) and navigate to your bucket
2. Open the **Permissions** tab and scroll to **Bucket policy**
3. Click **Edit**
4. Find the statement with `"Effect": "Deny"` and delete it (leave the `Allow` statement in place)
5. Click **Save changes**
6. Open the `BucketURL` — the AWS Broken Labs welcome page should now load

</details>
