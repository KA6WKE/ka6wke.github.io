---
layout: default
title: "Lab 03: Site Content"
lab_level: associate
lab_service: s3
lab_number: "03"
redirect_from: /docs/aws/labs/s3/lab-03-versioning/
---

# Lab 03 - Site Content

> **Difficulty**: Beginner
> **Service**: Amazon S3

## Scenario

Your team hosts a static website in S3. A teammate pushed a content update
and now the site is showing the wrong page. They're confident they uploaded
the right file — but something went wrong. Your job is to figure out what
happened and restore the correct content.

## What Was Deployed

| Resource                | Purpose                                                     |
|-------------------------|-------------------------------------------------------------|
| `AWS::S3::Bucket`       | S3 bucket configured for public static website hosting      |
| `AWS::S3::BucketPolicy` | Bucket policy granting public `s3:GetObject` on all objects |

The stack deployed without errors.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-03-site-content.yaml" download>lab-03-site-content.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-lab-03`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE**
6. Open the stack **Outputs** tab — you will see `BucketName` and `BucketURL`

## The Problem

Open the `BucketURL` from the stack Outputs in your browser.

**Expected**: the page displays the AWS Broken Labs welcome page.
**Actual**: the page displays "Coming Soon — This page is under construction."

## Fix the Lab

Diagnose why the wrong content is being served and restore the correct page.

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)

