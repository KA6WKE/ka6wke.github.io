---
layout: default
title: "Lab 04: S3 Shared Link"
lab_level: associate
lab_service: s3
lab_number: "04"
---

# Lab 04 - Presigned URL

> **Difficulty**: Beginner
> **Service**: Amazon S3

## Scenario

A teammate shared a link to a file stored in a private S3 bucket. The link worked
when they sent it, but now it's returning an error. The file is still in the bucket —
the link just doesn't work anymore. Figure out why and get the file accessible again.

## What Was Deployed

| Resource          | Purpose                                          |
| ----------------- | ------------------------------------------------ |
| `AWS::S3::Bucket` | Private S3 bucket with all public access blocked |

The stack deployed without errors. The bucket exists and `index.html` is in it.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-04-presigned-url.yaml" download>lab-04-presigned-url.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-lab-04`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE**
6. Open the stack **Outputs** tab — you will see `BucketName` and `PresignedUrl`

## The Problem

Copy the `PresignedUrl` from the stack Outputs and open it in your browser.

**Expected**: the page should display the AWS Broken Labs welcome page.
**Actual**: the browser returns an XML error:

```xml
<Error>
  <Code>AccessDenied</Code>
  <Message>Request has expired</Message>
  <X-Amz-Expires>5</X-Amz-Expires>
  <Expires>2026-03-12T01:27:21Z</Expires>
  <ServerTime>2026-03-12T01:27:33Z</ServerTime>
  <RequestId>10MPBS1FQARZQ477</RequestId>
  <HostId>
    bVlkslAfTxXWggkKExw5n+pdBs7Ts1/VmN34WFKrhbn4dQOt4YqMx0s1t2LspnS0PdACjHjvrqj64RL90n9XofRi6uasYVoi
  </HostId>
</Error>
```

The file exists in the bucket and has the correct permissions.

## Fix the Lab

Investigate why the presigned URL no longer works.

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Sharing objects with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)
