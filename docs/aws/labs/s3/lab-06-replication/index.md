---
layout: default
title: "Lab 06: S3 Replication"
lab_level: associate
lab_service: s3
lab_number: "06"
---

# Lab 06 - S3 Replication

> **Difficulty**: Intermediate
> **Service**: Amazon S3

## Scenario

Your team set up S3 replication to automatically copy objects from a source bucket
to a destination bucket for backup purposes. The replication rule is in place and
the stack deployed without errors — but after uploading files to the source bucket,
nothing is showing up in the destination. Figure out why replication isn't working
and fix it.

## What Was Deployed

| Resource          | Purpose                                                       |
|-------------------|---------------------------------------------------------------|
| `AWS::S3::Bucket` | Source bucket — files uploaded here should be replicated      |
| `AWS::S3::Bucket` | Destination bucket — should receive replicated objects        |
| `AWS::IAM::Role`  | IAM role granting S3 permission to perform replication        |

The stack deployed without errors. The replication rule is configured on the source
bucket. The destination bucket exists and is ready to receive objects.

## Deploy the Lab

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)
2. Click **Create stack** > **With new resources (standard)**
3. Select **Upload a template file** and upload <a href="lab-06-replication.yaml" download>lab-06-replication.yaml</a>
4. Enter a stack name (e.g., `brokenlabs-lab-06`) and click **Next** > **Next** > **Submit**
5. Wait for the stack status to reach **CREATE_COMPLETE**
6. Open the stack **Outputs** tab — you will see `SourceBucketName` and `DestinationBucketName`

## The Problem

Open both buckets in the [S3 console](https://console.aws.amazon.com/s3).

**Source bucket**: contains `index.html`
**Destination bucket**: empty — `index.html` has not been replicated

The replication rule is configured and the IAM role exists. But objects are not being copied to the destination.

## Fix the Lab

Investigate the replication configuration and determine why objects are not being
replicated.

To verify the fix:

1. Upload any file to the source bucket
2. Wait a few seconds, then check the destination bucket
3. The file should appear in the destination

Need help? Open [hints](hints) for progressive hints.

## Cleanup

1. Open [CloudFormation](https://console.aws.amazon.com/cloudformation), select your stack, and click **Delete**

## Resources

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Setting up replication](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-how-setup.html)
- [Replication configuration overview](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-add-config.html)
- [Questions or bugs? Open a GitHub Issue](https://github.com/KA6WKE/ka6wke.github.io/issues)
