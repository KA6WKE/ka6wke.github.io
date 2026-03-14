---
layout: default
---

# Hints — Presigned URL - Lab 04

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Read the error</summary>

Look closely at the error response in your browser. It will be an XML document. What does the error say?

</details>

---

<details markdown="1">
<summary>Hint 2 — Inspect the URL</summary>

Presigned URLs contain query parameters that control their behavior. Look at the URL itself.

</details>

---

<details markdown="1">
<summary>Hint 3 — How to fix it</summary>

The original presigned URL is expired and cannot be extended. You need to generate a new one with a longer expiration.

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The presigned URL was generated with an expiration of 5 seconds. By the time you copied it from the CloudFormation Outputs and opened it, it had already expired. Expired presigned URLs return `AccessDenied`.

---

**To generate a working presigned URL:**

1. Open the [S3 console](https://console.aws.amazon.com/s3) and navigate to your bucket
2. Check the box next to `index.html`
3. Click **Object actions** > **Share with a presigned URL**
4. Set a time interval (e.g., 1 hour) and click **Create presigned URL**
5. The URL is copied to your clipboard — open it in your browser
6. The AWS Broken Labs welcome page should load

</details>
