---
layout: default
title: "Lab 02: Website Access II"
lab_level: associate
lab_service: s3
lab_number: "02"
redirect_from: /docs/aws/labs/s3/lab-02-endpoint/
---

# Lab 02 - Website Access II

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

The stack deployed without errors.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-02-website-access-ii.yaml" download>lab-02-website-access-ii.yaml</a>
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

Diagnose why the shared link fails and get the page loading so the result matches
**Expected** above.

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)

