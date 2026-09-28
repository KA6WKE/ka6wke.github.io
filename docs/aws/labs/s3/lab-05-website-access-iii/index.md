---
layout: default
title: "Lab 05: Website Access III"
lab_level: associate
lab_service: s3
lab_number: "05"
redirect_from: /docs/aws/labs/s3/lab-05-permissions/
---

# Lab 05 - Website Access III

> **Difficulty**: Beginner
> **Service**: Amazon S3

## Scenario

Your team hosts a static website in S3. A colleague set it up and the stack deployed
without errors — but every request to the site returns 403 Access Denied.

## What Was Deployed

| Resource                | Purpose                                                     |
|-------------------------|-------------------------------------------------------------|
| `AWS::S3::Bucket`       | S3 bucket that hosts the site                               |
| `AWS::S3::BucketPolicy` | Bucket policy for the site                                  |

The stack deployed without errors.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-05-website-access-iii.yaml" download>lab-05-website-access-iii.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-lab-05`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE**
6. Open the stack **Outputs** tab — you will see `BucketName` and `BucketURL`

## The Problem

Open the `BucketURL` from the stack Outputs in your browser.

**Expected**: the page displays the AWS Broken Labs welcome page.
**Actual**: the browser returns an XML error:

```xml
<Error>
  <Code>AccessDenied</Code>
  <Message>Access Denied</Message>
  <RequestId>EEN63HXBJD24G08R</RequestId>
  <HostId>
    ZW2Wpr64vuzJ+qbQAAwHhKHVzSzDp39z6q8u4wfzfNjxJPkse0Q2bSH9AiYQPYtmw2cIosmLDTGkY+41XAPNZ10UvZfoOeNl
  </HostId>
</Error>
```

## Fix the Lab

Diagnose why the problem occurs and fix it so the result matches **Expected** above.

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)

