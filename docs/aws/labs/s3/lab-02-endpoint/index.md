---
layout: default
title: "Lab 02: S3 Bucket URL"
lab_level: associate
lab_service: s3
lab_number: "02"
---

# Lab 02 - S3 Bucket Region

> **Difficulty**: Beginner
> **Service**: Amazon S3

## Scenario

A teammate deployed this stack to host a static web page and sent you the
`BucketURL` from the CloudFormation Outputs to verify it loads. When you open
the URL they shared, you get an error instead of the page.

## What Was Deployed

| Resource                | Purpose                                                     |
|-------------------------|-------------------------------------------------------------|
| `AWS::S3::Bucket`       | S3 bucket containing a static `index.html`                  |
| `AWS::S3::BucketPolicy` | Bucket policy granting public `s3:GetObject` on all objects |

The stack deployed without errors and the bucket is correctly configured.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-02-bucket-region.yaml" download>lab-02-bucket-region.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-lab-02`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE**
6. Open the stack **Outputs** tab — you will see `BucketName` and `BucketURL`

## The Problem

Your teammate sent you this link. Open it in your browser:

[Test the link your teammate shared](https://brokenlabs-lab-02-us-east-2-123456789012.s3.us-east-2.amazonaws.com/index.html)

**Expected**: the page loads.
**Actual**: the browser displays an XML error:

```xml
<Error>
  <Code>NoSuchBucket</Code>
  <Message>The specified bucket does not exist</Message>
  <BucketName>brokenlabs-lab-02-us-east-2-123456789012</BucketName>
  <RequestId>N4GCJYNRJ03SDAZY</RequestId>
  <HostId>Bm9aWhnp6LJaPKfqnOG88qiannFpigzAJABImgqWnogu7C91kw7V1fLOTfq3OTb0j8TDy6ILdcE=</HostId>
</Error>
```

## Fix the Lab

Investigate why the URL fails and determine the correct URL to use.

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/s3.html)
- [Accessing a bucket using S3 path-style and virtual-hosted-style URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-bucket-intro.html)
- [Bucket restrictions and limitations](https://docs.aws.amazon.com/AmazonS3/latest/userguide/BucketRestrictions.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)
