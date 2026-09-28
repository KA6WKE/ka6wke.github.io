---
layout: default
---

# Hints — Access Denied IV - Lab 04

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

The role's own permissions policy is not the problem — you've already confirmed it
grants `s3:GetObject`. Open the role in the
[IAM console](https://console.aws.amazon.com/iam/home#/roles), go to the **Permissions**
tab, and look for the **Permissions boundary** section — it may be collapsed and need
to be expanded.

</details>

---

<details markdown="1">
<summary>Hint 2 — What to look for</summary>

A permissions boundary is a separate managed policy that sets the *maximum* permissions
an identity can ever have — regardless of what its own policies allow. Open the
boundary policy attached to this role. What actions does it grant?

</details>

---

<details markdown="1">
<summary>Hint 3 — How the two combine</summary>

A role's effective permissions are the **intersection** of its identity policies and
its permissions boundary — an action must be allowed by both to actually work. If the
boundary doesn't include an action, it's blocked even if the identity policy allows it.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The role has a permissions boundary attached that only allows
`s3:ListBucket`. Even though the role's own inline policy grants `s3:GetObject`, the
boundary caps the role's effective permissions to its own list of allowed actions.
Since `s3:GetObject` isn't in the boundary, it's denied — the identity policy and the
boundary must **both** allow an action for it to succeed.

---

**To fix the boundary policy:**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home#/policies) and open the role, then open the policy listed under **Permissions boundary** on its **Permissions** tab
2. Open the **Permissions** tab and click **Edit**
3. Add `s3:GetObject` to the `Action` list alongside `s3:ListBucket`
4. Click **Next** > **Save changes**
5. Re-run the simulation for `s3:GetObject` in the [IAM Policy Simulator](https://policysim.aws.amazon.com/) — it should now show **allowed**

If the resource field shows an error when you enter the ARN, check which resource
type is selected. `s3:GetObject` offers both `object` and `accesspointobject` in the
console's resource picker — this lab doesn't use an Access Point, so select
**`object`** and provide the bucket name and key.

---

**References**

- [Permissions boundaries for IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)
- [IAM policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)

</details>
