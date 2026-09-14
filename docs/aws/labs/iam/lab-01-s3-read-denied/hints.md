---
layout: default
---

# Hints — IAM Role Denied Read Access - Lab 01

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

The role has a permissions policy attached, and it isn't empty. Open the role in the
[IAM console](https://console.aws.amazon.com/iam/home#/roles) and review its
**Permissions** tab.

</details>

---

<details markdown="1">
<summary>Hint 2 — What to look for</summary>

Look at the `Action` list in the inline policy statement. Which S3 actions are listed?
Reading an object and listing a bucket's contents are two different API actions.

</details>

---

<details markdown="1">
<summary>Hint 3 — The gap</summary>

The policy grants `s3:ListBucket` (list the bucket's contents) but never grants
`s3:GetObject` (read an object's contents). Without an explicit `Allow` for
`s3:GetObject`, IAM's default is to deny it — this is called an **implicit deny**.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The role's inline policy only grants `s3:ListBucket`. It never grants
`s3:GetObject`, so any request to read an object is implicitly denied — IAM denies by
default unless a policy explicitly allows the action.

---

**To fix the policy:**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and select the role from the stack Outputs
2. Open the **Permissions** tab, find the inline policy, and click **Edit**
3. Add `s3:GetObject` to the `Action` list alongside `s3:ListBucket`
4. Click **Next** > **Save changes**
5. Re-run the simulation for `s3:GetObject` in the [IAM Policy Simulator](https://policysim.aws.amazon.com/) — it should now show **allowed**

If you're using the **Simulate** button inside the IAM console's policy editor, remember
to set an explicit **Resource** ARN for each action instead of leaving the default `*`
— testing against `*` shows **denied** regardless of the policy, since this policy only
grants access to a specific bucket:

- `s3:ListBucket` → the `ListBucketResourceArn` value from Outputs
- `s3:GetObject` → the `GetObjectResourceArn` value from Outputs

</details>
