# AWS S3 Cross-Account Bucket Access

**Setup Instructions for Data Delivery**

---

## Overview

We would like to deliver data files directly to your AWS S3 bucket. To enable this, you will need to add a bucket policy that grants our AWS account cross-account access to write files to your bucket.

This document explains exactly what permissions are needed and provides a ready-to-use policy template.

## What We Need From You

Please provide us with the following information so we can confirm the integration:

- **Your S3 bucket name** (e.g., my-company-data-bucket)
- **Your S3 bucket region** (e.g., us-east-1)
- **A preferred folder/prefix** within the bucket where files should be delivered (optional)
- **Confirmation** that the bucket policy below has been applied

## Permissions Required

The following S3 permissions are needed for us to deliver data into your bucket:

| Permission | Purpose |
| --- | --- |
| `s3:GetBucketLocation` | Required to determine the bucket's AWS region. |
| `s3:ListBucket` | Required to verify that we are not overwriting an existing object when uploading data. |
| `s3:GetObject` | Required together with `s3:ListBucket` to verify that a file does not already exist in the target destination. |
| `s3:PutObject` | Required for uploading data files to your bucket. |

## Setup Steps

### Step 1: Open Your S3 Bucket Policy

1. Sign in to the AWS Management Console.
2. Navigate to S3 and select the bucket where you would like to receive data.
3. Go to the **Permissions** tab.
4. Scroll down to **Bucket policy** and click **Edit**.

### Step 2: Apply the Bucket Policy

Copy the JSON policy below and paste it into your bucket policy editor. Replace `<your-s3-bucket>` with the name of your S3 bucket.

> **Important:** You must also replace `<our-aws-account-arn>` with the ARN we provide to you separately.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CrossAccountS3Access",
      "Effect": "Allow",
      "Principal": {
        "AWS": "<our-aws-account-arn>"
      },
      "Action": [
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject",
        "s3:GetBucketLocation"
      ],
      "Resource": [
        "arn:aws:s3:::<your-s3-bucket>",
        "arn:aws:s3:::<your-s3-bucket>/*"
      ]
    }
  ]
}
```

### Step 3: Save and Confirm

1. Click **Save changes** to apply the bucket policy.
2. Send us a confirmation along with your bucket name and region so we can begin delivering data.

## Important Notes

- This policy grants the minimum permissions necessary for us to upload data. We cannot delete objects, modify bucket settings, or change permissions on your bucket.
- If you already have an existing bucket policy, you will need to merge the statement above into your existing policy's `Statement` array rather than replacing the entire policy.
- If you would like to restrict access to a specific folder/prefix within the bucket, replace `<your-s3-bucket>/*` with `<your-s3-bucket>/your-folder/*`.
- If you have any questions or need our AWS account ARN, please contact us and we will provide it.

---

**Questions?**

If you have any questions about this setup or encounter any issues, please don't hesitate to reach out to us. We're happy to schedule a call to walk through the process together.
