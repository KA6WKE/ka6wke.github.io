---
layout: default
---

# Hints — Role Denied After Policy Update - Lab 02

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

Open the role in the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and
open its **Permissions** tab. Look at the inline policy attached to the role.

</details>

---

<details markdown="1">
<summary>Hint 2 — What to look for</summary>

A policy document can contain more than one statement. Each statement has its own
`Effect`, and each is evaluated independently against the request. How many statements
does this policy have for `s3:GetObject`?

</details>

---

<details markdown="1">
<summary>Hint 3 — The conflict</summary>

In AWS, an explicit `Deny` always wins over an `Allow` — even when both statements are
in the same policy and target the same action and resource. Is there a `Deny` statement
covering `s3:GetObject`?

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The role's policy has two statements for `s3:GetObject` on the same
resource: one `Allow` and one `Deny`. IAM always evaluates an explicit `Deny` before any
`Allow`, so the `Deny` statement overrides the `Allow` and every request is blocked.

---

**To fix the policy:**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and select the role from the stack Outputs
2. Open the **Permissions** tab, find the inline policy, and click **Edit**
3. Locate the statement with `"Effect": "Deny"` (`Sid: DenyGetObject`) and remove it, leaving the `Allow` statement in place
4. Click **Next** > **Save changes**
5. Re-run the simulation for `s3:GetObject` in the [IAM Policy Simulator](https://policysim.aws.amazon.com/) — it should now show **allowed**

If the resource field shows an error when you enter the bucket ARN, check which
resource type is selected. `s3:GetObject` offers both `object` and `accesspointobject`
in the console's resource picker — this lab doesn't use an Access Point, so select
**`object`** and provide the bucket name and key (`*` for any object).

</details>
