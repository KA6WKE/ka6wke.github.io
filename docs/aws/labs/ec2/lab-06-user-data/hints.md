# Hints — EC2 User Data - Lab 06

Open each hint only after you've spent time investigating on your own.

---

<details>
<summary>Hint 1 — Connection refused vs timed out</summary>

The browser shows **ERR_CONNECTION_REFUSED**, not a timeout.

- **Timeout** means the request never reached the instance — a security group or routing issue.
- **Connection refused** means the request reached the instance but nothing is listening on port 80.

The networking is fine. Connect to the instance using
**Session Manager** (EC2 → select instance → Connect → Session Manager → Connect) and
check Apache's status:

```
systemctl status httpd
```

</details>

---

<details>
<summary>Hint 2 — Check the startup log</summary>

User data scripts run automatically when an instance first launches. If the script fails,
the instance still boots and passes health checks — the failure is silent from the outside.

Check the cloud-init output log to see what happened when the instance started:

```
sudo cat /var/log/cloud-init-output.log
```

Look for any errors near the package installation step.

</details>

---

<details>
<summary>Hint 3 — The package name</summary>

The log shows an error like:

```
No match for argument: apache2
Error: Unable to find a match: apache2
```

`apache2` is the Apache package name on Ubuntu and Debian systems. Amazon Linux 2023 uses
a different package name. What is the correct package name for Apache on Amazon Linux?

</details>

---

<details>
<summary><span style="color: red;">Spoiler Alert</span> — Full Solution</summary>

**Root cause**: The user data script installs `apache2`, which is the Ubuntu/Debian package
name for Apache. Amazon Linux 2023 uses `dnf` for package management and the correct package
name is `httpd`. The install command fails silently (no `set -e` in the script), so the instance
finishes booting and reaches a healthy state — but Apache was never installed. Nothing is
listening on port 80, so the browser gets a connection refused error.

---

**To fix:**

1. Open the [EC2 console](https://console.aws.amazon.com/ec2), select your instance
2. Click **Connect** > **Session Manager** > **Connect**
3. In the terminal, install and start Apache with the correct package name:

```
sudo dnf install -y httpd
sudo systemctl start httpd
```

4. Reload the `WebPageURL` in your browser — the words "It Works!" will be displayed.

</details>
