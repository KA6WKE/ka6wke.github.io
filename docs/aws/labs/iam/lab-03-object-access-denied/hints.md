---
layout: default
---

# Hints — GetObject Denied Despite Allow Statement - Lab 03

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

Open the role in the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and
open its **Permissions** tab. Look closely at the `Resource` field in the policy
statement — not just the `Action` field.

</details>

---

<details markdown="1">
<summary>Hint 2 — What to look for</summary>

In S3, a bucket and the objects inside it have different ARNs:

- Bucket ARN: `arn:aws:s3:::bucket-name`
- Object ARN: `arn:aws:s3:::bucket-name/object-key` (or `bucket-name/*` for all objects)

Which form does the policy's `Resource` value use?

</details>

---

<details markdown="1">
<summary>Hint 3 — The mismatch</summary>

`s3:GetObject` is an object-level action — it always requires an object ARN
(`bucket-name/*` or a specific key), never the bucket ARN alone. If the policy's
`Resource` only lists the bucket ARN, it doesn't match any object, so the action is
implicitly denied no matter what the `Action` field says.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The policy's `Resource` value is the bucket ARN
(`arn:aws:s3:::bucket-name`) instead of an object ARN
(`arn:aws:s3:::bucket-name/*`). Because `s3:GetObject` operates on objects, not
buckets, the `Allow` statement never matches — the request falls through to an
implicit deny.

---

**To fix the policy:**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and select the role from the stack Outputs
2. Open the **Permissions** tab, find the inline policy, and click **Edit**
3. Change the `Resource` value from the bucket ARN to the bucket ARN plus `/*` (e.g. `arn:aws:s3:::brokenlabs-iam-lab-03-.../*`)
4. Click **Next** > **Save changes**
5. Re-run the simulation for `s3:GetObject` in the [IAM Policy Simulator](https://policysim.aws.amazon.com/) — it should now show **allowed**

If the resource field shows an error when you enter the ARN, check which resource
type is selected. `s3:GetObject` offers both `object` and `accesspointobject` in the
console's resource picker — this lab doesn't use an Access Point, so select
**`object`** and provide the bucket name and key.

</details>
