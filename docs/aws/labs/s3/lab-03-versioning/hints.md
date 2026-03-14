---
layout: default
---

# Hints — S3 Content Recovery - Lab 03

Open each hint only after you've spent time investigating on your own.

---

<details markdown="1">
<summary>Hint 1 — Where to look</summary>

The bucket and policy are fine — the problem is with `index.html` itself.

Open the [S3 console](https://console.aws.amazon.com/s3), navigate to your
bucket, and click on `index.html`. What information is available about that
object?
</details>

---

<details markdown="1">
<summary>Hint 2 — Look at the object's history</summary>

S3 can keep a record of every change made to an object over time. Look at the
tabs available when you select `index.html` in the S3 console. Is there a tab
that shows more than just the current state of the file?

</details>

---

<details markdown="1">
<summary>Hint 3 — What you are looking for</summary>

You should see more than one entry in the object's history. One of them is the
current (broken) file. Another is the original correct file.

Can you delete the current version to reveal the one underneath it?

</details>

---

<details markdown="1">
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The bucket has versioning enabled. When the teammate uploaded
the new file, S3 stored it as a new version of `index.html` — but that version
contained the wrong content ("Coming Soon"). The original correct file still
exists as a previous version.

---

**To restore the correct content:**

1. Open the [S3 console](https://console.aws.amazon.com/s3) and navigate to your bucket
2. Click on `index.html`
3. Open the **Versions** tab
4. You will see two versions — the current one (wrong content) and an older one (correct content)
5. Check the box next to the **latest** version and click **Delete**
6. Type `permanently delete` to confirm, then click **Delete**
7. The previous version is now the active file — open the `BucketURL` and it should display the AWS Broken Labs welcome page

</details>
