---
layout: default
title: "Lab 01: Website Access I"
lab_level: associate
lab_service: s3
lab_number: "01"
redirect_from: /docs/aws/labs/s3/lab-01-s3-access/
---

# Lab 01 - Website Access I

> **Difficulty**: Beginner
> **Service**: Amazon S3

## Scenario

Your team deployed an S3 bucket to host a static web page publicly. The engineer who
set it up confirmed the stack deployed successfully. However, users are reporting they
cannot access any files — every request returns an error.

## What Was Deployed

| Resource                 | Purpose                                                       |
|--------------------------|---------------------------------------------------------------|
| `AWS::S3::Bucket`        | S3 bucket intended to serve static assets publicly            |
| `AWS::S3::BucketPolicy`  | Bucket policy granting public `s3:GetObject` on all objects   |

The stack deployed without errors.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-01-website-access-i.yaml" download>lab-01-website-access-i.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-lab-01`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE**
6. Find the bucket name in the stack **Outputs** tab

## The Problem

Files in the bucket cannot be accessed publicly. Reproduce the issue:

1. Open the `BucketURL` from the stack **Outputs** tab in your browser.

**Expected**: the page loads.
**Actual**: the browser displays an XML error:

```xml
<Error>
  <Code>AccessDenied</Code>
  <Message>Access Denied</Message>
  <RequestId>N4GCJYNRJ03SDAZY</RequestId>
  <HostId>Bm9aWhnp6LJaPKfqnOG88qiannFpigzAJABImgqWnogu7C91kw7V1fLOTfq3OTb0j8TDy6ILdcE=</HostId>
</Error>
```

The `RequestId` and `HostId` values will differ each time.

## Fix the Lab

Diagnose why the problem occurs and fix it so the result matches **Expected** above.

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)

