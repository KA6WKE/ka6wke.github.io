---
layout: default
---

# Hints — Temporary Access Grant Not Working - Lab 05

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

Open the role in the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and
open its **Permissions** tab. View the full JSON of the inline policy statement — not
just the summarized visual editor view.

</details>

---

<details markdown="1">
<summary>Hint 2 — What to look for</summary>

The statement has a `Condition` block. Condition keys let a policy apply only when
certain context is true at request time — including time-based conditions like
`DateLessThan` or `DateGreaterThan` on the `aws:CurrentTime` key.

</details>

---

<details markdown="1">
<summary>Hint 3 — The expired grant</summary>

`DateLessThan` with `aws:CurrentTime` means "only allow this while the current time is
before the given date." If that date is in the past, the condition can never be true
again — the `Allow` statement will never apply, no matter what the `Action` and
`Resource` say.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The policy statement includes a `Condition` of
`DateLessThan: { "aws:CurrentTime": "2020-01-01T00:00:00Z" }`. Since that date has
already passed, the condition evaluates to false on every request, so the `Allow`
statement never takes effect — the access grant effectively expired.

---

**To fix the policy:**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and select the role from the stack Outputs
2. Open the **Permissions** tab, find the inline policy, and click **Edit**
3. Update the `DateLessThan` value under `aws:CurrentTime` to a future date (or remove the `Condition` block entirely if the grant should no longer expire)
4. Click **Next** > **Save changes**
5. Re-run the simulation for `s3:GetObject` in the [IAM Policy Simulator](https://policysim.aws.amazon.com/) — it should now show **allowed**

If the resource field shows an error when you enter the ARN, check which resource
type is selected. `s3:GetObject` offers both `object` and `accesspointobject` in the
console's resource picker — this lab doesn't use an Access Point, so select
**`object`** and provide the bucket name and key.

</details>
