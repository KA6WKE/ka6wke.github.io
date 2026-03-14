---
layout: default
title: "Lab 05: S3 Bucket Policy"
lab_level: associate
lab_service: s3
lab_number: "05"
---

# Lab 05 - S3 Bucket Policy

> **Difficulty**: Beginner
> **Service**: Amazon S3

## Scenario

Your team hosts a static website in S3. The bucket and policy were set up by
a colleague and the stack deployed without errors — but every request to the
site returns 403 Access Denied. The file is in the bucket. Something in the
configuration is wrong.

## What Was Deployed

| Resource                | Purpose                                                     |
|-------------------------|-------------------------------------------------------------|
| `AWS::S3::Bucket`       | S3 bucket with public access block partially disabled       |
| `AWS::S3::BucketPolicy` | Bucket policy controlling public access to `index.html`     |

The stack deployed without errors. The bucket exists, the file is uploaded,
and a bucket policy is in place.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-05-explicit-deny.yaml" download>lab-05-explicit-deny.yaml</a>
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

The bucket exists and the file is in it. A bucket policy is in place.

## Fix the Lab

Investigate the bucket policy and determine why access is being denied.

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Bucket policies and user policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-iam-policies.html)
- [IAM policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)
