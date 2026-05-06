# AWS S3 Cross-Account Access

**ARN Request — Granting You Access to Our S3 Bucket**

---

## Overview

We host data files in an AWS S3 bucket and would like to grant your AWS account direct access to retrieve them. To set this up securely, we use AWS cross-account access, which requires your AWS Account ARN (Amazon Resource Name).

This document explains what an ARN is, how to find yours, and what to expect once you provide it to us.

## What We Need From You

Please provide us with the following:

- **Your AWS Account ARN** — this is the identity we will authorize in our bucket policy. It typically looks like: `arn:aws:iam::123456789012:root`
- **Preferred IAM principal (optional)** — if you would like us to grant access to a specific IAM role or user rather than the entire account, please provide that ARN instead (e.g., `arn:aws:iam::123456789012:role/YourRoleName`).
- **Contact confirmation** — once you send us the ARN, we will configure access and confirm when the bucket is ready for you.

## What Is an ARN?

An ARN (Amazon Resource Name) is a unique identifier for resources within AWS. For cross-account S3 access, we need the ARN of either your AWS account root or a specific IAM role/user. This allows us to add a policy to our S3 bucket that explicitly grants your identity permission to access the files.

Common ARN formats:

| Type | ARN Format |
| --- | --- |
| Account root | `arn:aws:iam::123456789012:root` |
| IAM role | `arn:aws:iam::123456789012:role/RoleName` |
| IAM user | `arn:aws:iam::123456789012:user/UserName` |

## How to Find Your ARN

### Option A: Find Your Account ARN

1. Sign in to the AWS Management Console.
2. Click on your account name in the top-right corner of the console.
3. Note your **Account ID** (a 12-digit number).
4. Your account ARN is: `arn:aws:iam::<your-12-digit-account-id>:root`

### Option B: Find a Specific IAM Role or User ARN

If your organization prefers to use a specific IAM role or user for this integration:

1. Sign in to the AWS Management Console.
2. Navigate to **IAM** (Identity and Access Management).
3. Select **Roles** or **Users** from the left sidebar.
4. Click on the role or user you want to use for this integration.
5. Copy the **ARN** shown in the summary section at the top of the page.

### Option C: Using the AWS CLI

If you have the AWS CLI configured, you can retrieve your identity ARN with:

```bash
aws sts get-caller-identity
```

This command returns your Account ID and ARN. Send us the value from the `Arn` field in the output.

## What Happens Next

Once you provide your ARN, here is what we will do on our side:

1. We will add your ARN to the bucket policy on our S3 bucket, granting you read access to the relevant files.
2. We will confirm the setup is complete and provide you with the bucket name, region, and any relevant path/prefix.
3. You will then be able to access the files using the AWS CLI, SDKs, or any S3-compatible tooling from your AWS account.

### Permissions We Will Grant

The following permissions will be applied to your ARN on our bucket:

| Permission | Purpose |
| --- | --- |
| `s3:GetObject` | Allows you to download/read files from the bucket. |
| `s3:ListBucket` | Allows you to list the contents of the bucket or a specific prefix. |
| `s3:GetBucketLocation` | Allows you to determine the bucket's AWS region. |

> **Note:** These are read-only permissions. You will not be able to modify, delete, or upload files to our bucket.

## Security Considerations

- Your ARN is not sensitive information — it is an identifier, not a credential. It cannot be used by others to access your AWS resources.
- The bucket policy we apply only grants access to the specific ARN you provide. No other AWS accounts will gain access through this policy.
- If you prefer, you can create a dedicated IAM role for this integration and provide that role's ARN. This allows you to control and audit access independently within your own AWS account.
- Access can be revoked at any time by either party — we can remove your ARN from our policy, or you can delete/disable the IAM principal on your end.

---

**Questions?**

If you have any questions about this process or need assistance locating your ARN, please don't hesitate to reach out. We're happy to schedule a call to walk through it together.
