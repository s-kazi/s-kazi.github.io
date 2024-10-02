---
layout: post
title: Security AWS S3 Buckets
subtitle: using S3 Buckets Policy
permalink: /aws-s3-security-bucket-policy/
leftmenu: left-menu/aws-s3.html
related: related-links/aws-s3.html
---

You can secure AWS S3 buckets using a **bucket policy** involves defining a set of permissions that explicitly control access to the bucket and its contents. Bucket policies are JSON documents that define which actions are allowed or denied for specific principals (users, roles, services) under certain conditions.

Here are the key steps and examples to secure your AWS S3 bucket using a bucket policy:

### 1. Access Over SSL only
To ensure that your bucket is only accessible over TLS, you can deny all other traffic by configuring the bucket policy.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::YOUR_BUCKET_NAME",
        "arn:aws:s3:::YOUR_BUCKET_NAME/*"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "false"
        }
      }
    }
  ]
}
```
**Explanation**: 
- This policy denies all access (`s3:*`) from any principal (`*`) if they are not using **HTTPS** (`aws:SecureTransport: false`). Replace `YOUR_BUCKET_NAME` with the name of your bucket.
- The resource name is specified using ARN but with two values - `arn:aws:s3:::YOUR_BUCKET_NAME/*` and `arn:aws:s3:::YOUR_BUCKET_NAME`. The first refers to the objects (in this case, all objects) in the bucket and the second one refers to the bucket itself. 

### 2. Allow Access Only from Specific IPs
To restrict access to a bucket based on IP address ranges, use the `IpAddress` condition.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "192.0.2.0/24"
        }
      }
    }
  ]
}
```
**Explanation**:
- This policy allows `s3:GetObject` access (read-only) to objects in the bucket for requests that originate from IPs in the range `192.0.2.0/24`.
- This is a public IP address. You cannot specify private addresses like `10.0.0.0/8` as S3 will not see your private IP.

### 3. Allow Access to Specific IAM Roles or Users
To give access only to specific AWS IAM users or roles, you can specify their ARN (Amazon Resource Name) in the `Principal` field.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ACCOUNT_ID:role/ROLE_NAME"
      },
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```
**Explanation**:
- This policy allows the IAM role `ROLE_NAME` in AWS account `ACCOUNT_ID` to read and write (`s3:GetObject`, `s3:PutObject`) to the objects in the bucket.

### 4. Require Multi-Factor Authentication (MFA) for Sensitive Actions
To enforce MFA for specific actions, such as deleting objects, use the `aws:MultiFactorAuthPresent` condition.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```
**Explanation**:
- This policy denies object deletions (`s3:DeleteObject`) unless MFA is enabled for the requesting user or role.

### 5. Enforce Encrypted Uploads
To require that all objects uploaded to the S3 bucket are encrypted with server-side encryption (SSE), you can use a condition on the `s3:x-amz-server-side-encryption` key.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": "AES256"
        }
      }
    }
  ]
}
```
**Explanation**:
- This policy denies any `PutObject` request if the upload does not include server-side encryption with the `AES256` algorithm.

### 6. Allow Read-Only Access to Specific AWS Account
To grant read-only access to another AWS account, use the `Principal` field to specify the AWS account's ID.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ANOTHER_ACCOUNT_ID:root"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```
**Explanation**:
- This policy allows the root user of `ANOTHER_ACCOUNT_ID` to read objects (`s3:GetObject`) in your bucket.

### 7. Restricting Access to VPC Endpoint
If your S3 bucket needs to be accessed only through a VPC (Virtual Private Cloud) endpoint, use the `aws:SourceVpc` condition.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::YOUR_BUCKET_NAME",
        "arn:aws:s3:::YOUR_BUCKET_NAME/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "aws:SourceVpc": "vpc-987654321"
        }
      }
    }
  ]
}
```
**Explanation**:
- This policy denies access to the bucket unless the requests originate from the VPC with ID `vpc-987654321`.

### 8. Granting Access to CloudFront for Website Hosting
If you are using CloudFront to serve content from your S3 bucket, you can grant CloudFront access using the OAI (Origin Access Identity).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "CanonicalUser": "CLOUDFRONT_OAI_ID"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```
**Explanation**:
- This policy allows CloudFront (via the OAI identified by `CLOUDFRONT_OAI_ID`) to retrieve objects (`s3:GetObject`) from your bucket for serving through the CDN.


### 9. Bucket Policy for Access Points
Access Points provide access to your S3 bucket using a different endpoint. You can restrict permissions at the access point level and need not worry about accidental policy changes. To use access point, you need both an access point policy and a bucket policy.

Here's the access point policy.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ACCOUNT_ID:role/RoleName"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:ap-southeast-2:ACCOUNT_ID:accesspoint/your-access-point/object/*"
    }
  ]
}
```
**Explanation**:
- This policy allows the RoleName from the specified ACCOUNT_ID to retrieve objects via the Access Point.

Here's the bucket policy.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
      "Condition": {
        "StringEquals": {
          "s3:DataAccessPointAccount": "ACCOUNT_ID"
        }
      }
    }
  ]
}
```
**Explanation**:
- This bucket policy allows access to objects in the YOUR_BUCKET_NAME, but only if the request comes from an Access Point in the specified ACCOUNT_ID.


---

### Additional Consideration
- Always follow the [principle of least privilege](/aws-iam-least-privilege/), ensuring users and roles only get the minimum permissions they need.
- Regularly review and update your bucket policies.
- Enable **AWS CloudTrail** to log bucket activity for auditing purposes.
