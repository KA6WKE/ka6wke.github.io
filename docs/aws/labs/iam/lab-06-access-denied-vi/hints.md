---
layout: default
---

# Hints — Access Denied VI - Lab 06

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

Open the automation role in the [IAM console](https://console.aws.amazon.com/iam/home#/roles)
and open its **Permissions** tab. Find the statement for `iam:PassRole`.

</details>

---

<details markdown="1">
<summary>Hint 2 — What to look for</summary>

`iam:PassRole` is an unusual permission: it isn't a permission on the actor itself, but
a permission to hand a *specific role* to a service. The `Resource` in the
`iam:PassRole` statement must match the ARN of the role actually being passed.

Compare that `Resource` value to the target execution role's real ARN
(`TestResourceArn` from the stack Outputs). Are they the same role?

</details>

---

<details markdown="1">
<summary>Hint 3 — The wrong role</summary>

The `iam:PassRole` statement grants permission to pass a role, but it names a
*different* role than the one the automation actually needs to attach to the Lambda
function. Scoping `iam:PassRole` to the wrong ARN is the same, from IAM's perspective,
as not granting it at all for the role you actually need.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The automation role's `iam:PassRole` statement is scoped to a
`Resource` ARN for a role that isn't the target execution role — it points at an
unrelated role name. Because `iam:PassRole` is evaluated against the specific resource
ARN being passed, the statement never matches the real target role, and every attempt
to pass it is denied.

---

**To fix the policy:**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home#/roles) and select the automation (actor) role from the stack Outputs
2. Open the **Permissions** tab, find the inline policy, and click **Edit**
3. Update the `Resource` value on the `iam:PassRole` statement to the target role's actual ARN (`TestResourceArn` from Outputs)
4. Click **Next** > **Save changes**
5. Re-run the simulation for `iam:PassRole` in the [IAM Policy Simulator](https://policysim.aws.amazon.com/) — it should now show **allowed**

---

**References**

- [Granting a user permissions to pass a role to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html)
- [AWS Lambda permissions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html)

</details>
