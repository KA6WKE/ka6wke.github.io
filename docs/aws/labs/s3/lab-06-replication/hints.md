---
layout: default
---

# Hints — S3 Replication - Lab 06

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Check the replication rule</summary>

The replication rule is configured and enabled. Look more closely at the rule
itself — not just that it exists.

Open the [S3 console](https://console.aws.amazon.com/s3), navigate to the
**source bucket**, open the **Management** tab, and click into the replication
rule. What are the rule settings?

</details>

---

<details markdown="1">
<summary>Hint 2 — Replication rules can be scoped</summary>

Replication rules can be configured to replicate all objects, or only objects
that match a specific prefix or tag filter.

Look at the **Scope** section of the replication rule. What objects does this
rule apply to?

</details>

---

<details markdown="1">
<summary>Hint 3 — Compare the filter to the files in the bucket</summary>

The replication rule has a prefix filter. Only objects whose key starts with
that prefix will be replicated.

What is the prefix in the rule? Does `index.html` match it?

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The replication rule has a prefix filter set to `archive/`.
Only objects with keys starting with `archive/` are in scope for replication.
`index.html` is at the bucket root — it does not match the prefix, so it is
silently excluded and never replicated to the destination.

---

**To fix the replication rule:**

1. Open the [S3 console](https://console.aws.amazon.com/s3) and navigate to the source bucket
2. Open the **Management** tab and click the replication rule to edit it
3. Under **Scope**, change the scope to **Apply to all objects in the bucket**
4. Save the rule
5. Upload any file to the source bucket (e.g., upload a test `.txt` file via **Upload**)
6. Wait a few seconds, then open the destination bucket — the file should appear

</details>
