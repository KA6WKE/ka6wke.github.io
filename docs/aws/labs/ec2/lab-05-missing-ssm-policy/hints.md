# Hints — EC2 IAM / Session Manager - Lab 05

Open each hint only after you've spent time investigating on your own.

---

<details>
<summary>Hint 1 — What Session Manager needs</summary>

AWS Systems Manager Session Manager connects to an EC2 instance through the SSM agent —
software that runs on the instance and communicates with the SSM service. For this
communication to work, the instance needs permission to call SSM APIs.

Those permissions come from the **IAM role** attached to the instance. Navigate to
**IAM → Roles** and find the role named in the `RoleName` stack Output.

What policies are attached to it?

</details>

---

<details>
<summary>Hint 2 — The role has no policies</summary>

The IAM role exists and is attached to the instance — but it has no policies. A role with
no permissions cannot authorize any AWS API calls, including the SSM calls that the agent
needs to register the instance and establish sessions.

Session Manager requires a specific AWS managed policy. What is that policy?

</details>

---

<details>
<summary>Hint 3 — The required policy</summary>

Session Manager requires the `AmazonSSMManagedInstanceCore` AWS managed policy. This policy
grants the SSM agent on the instance the permissions it needs to:

- Register the instance with Systems Manager
- Receive session commands
- Send session output back to the console

Attach this policy to the lab role, wait about 1 minute, then retry Session Manager. If
the **Connect** button remains grayed out, reboot the instance (Actions → Instance State →
Reboot instance) and try again after 1–2 minutes.

</details>

---

<details>
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The IAM role attached to the instance has no policies. The SSM agent on the
instance cannot authenticate with the Systems Manager service because the role lacks the
permissions needed to make SSM API calls. The instance may appear in Fleet Manager briefly
but Session Manager cannot establish a connection.

---

**To fix:**

1. Open the [IAM console](https://console.aws.amazon.com/iam) and go to **Roles**
2. Find and select the role shown in the `RoleName` stack Output
3. Click **Add permissions** → **Attach policies**
4. Search for `AmazonSSMManagedInstanceCore` and select it
5. Click **Add permissions**
6. Wait approximately 1 minute for the SSM agent to pick up the new permissions
7. Go to EC2, select your instance, click **Connect** → **Session Manager** → **Connect**
8. A browser terminal should open
9. If the **Connect** button remains grayed out, reboot the instance (Actions → Instance State → Reboot instance) and wait 1–2 minutes before retrying

</details>
