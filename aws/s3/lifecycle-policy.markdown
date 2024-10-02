---
layout: post
title: Lifecycle Policy in S3
subtitle: to move objects between storage tiers
permalink: /aws-s3-lifecycle-policy/
leftmenu: left-menu/aws-s3.html
related: related-links/aws-s3.html
---

To write a **Lifecycle Policy** in AWS S3, you define rules for transitioning objects to different storage classes or automatically deleting them after a specified time. Lifecycle policies help optimize storage costs by moving infrequently accessed data to cheaper storage classes or by expiring (deleting) them when they are no longer needed.

You can set up a lifecycle policy using either the AWS Management Console or by directly writing a JSON configuration.

Here's how you can write a lifecycle policy in JSON format.

### Example Lifecycle Policy in JSON:

This example demonstrates how to:
1. **Move objects to S3 Standard-IA** (Infrequent Access) after 30 days.
2. **Move objects to S3 Deep Archive** after 1 year.
3. **Delete objects** after 7 years.

```json
{
  "Rules": [
    {
      "ID": "Move to Standard-IA after 30 days",
      "Filter": {
        "Prefix": "" 
      },
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "STANDARD_IA"
        }
      ],
      "NoncurrentVersionTransitions": [],
      "NoncurrentVersionExpiration": {},
      "AbortIncompleteMultipartUpload": {}
    },
    {
      "ID": "Move to Deep Archive after 365 days",
      "Filter": {
        "Prefix": ""
      },
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 365,
          "StorageClass": "DEEP_ARCHIVE"
        }
      ],
      "NoncurrentVersionTransitions": [],
      "NoncurrentVersionExpiration": {},
      "AbortIncompleteMultipartUpload": {}
    },
    {
      "ID": "Expire objects after 7 years",
      "Filter": {
        "Prefix": ""
      },
      "Status": "Enabled",
      "Expiration": {
        "Days": 2555
      },
      "NoncurrentVersionTransitions": [],
      "NoncurrentVersionExpiration": {},
      "AbortIncompleteMultipartUpload": {}
    }
  ]
}
```

### Explanation of the Policy:
- **ID**: A unique identifier for each rule (optional but recommended).
- **Filter**: Determines which objects this rule applies to. In this example, no prefix is specified (`"Prefix": ""`), meaning the rule applies to all objects in the bucket. You can set a prefix to apply rules to objects in specific folders or with specific prefixes.
- **Status**: Set to `"Enabled"` to activate the rule.
- **Transitions**: Defines when the object should be moved to a different storage class. The `"Days"` field specifies the number of days after the object's creation when the transition should occur.
  - In this example, objects are moved to **S3 Standard-IA** after 30 days and **S3 Deep Archive** after 365 days.
- **Expiration**: Automatically deletes objects after a specified number of days (2555 days in this case).
- **NoncurrentVersionTransitions** and **NoncurrentVersionExpiration**: These fields apply to versioned objects, defining when older versions of objects should transition to different storage classes or be deleted. In this example, they are not used.
- **AbortIncompleteMultipartUpload**: This option is used to clean up incomplete multipart uploads after a specific time, but it's left empty in this example.

### How to Apply the Lifecycle Policy

1. **Using the AWS Management Console**:
   - Go to the **S3 Console**.
   - Choose your bucket, then click on the **"Management"** tab.
   - Scroll to **"Lifecycle Rules"** and click **"Create lifecycle rule"**.
   - Follow the steps to create rules for transition or expiration.
   - You can either use predefined options in the UI or enter a JSON lifecycle policy similar to the one above.

2. **Using AWS CLI**:
   You can apply this policy using the AWS CLI by uploading the JSON file and applying it to the S3 bucket:

   Save the JSON policy in a file (e.g., `lifecycle.json`), and then use the following CLI command:

   ```bash
   aws s3api put-bucket-lifecycle-configuration --bucket <your-bucket-name> --lifecycle-configuration file://lifecycle.json
   ```

### Storage Classes You Can Use in Transitions:
- **STANDARD_IA**: Infrequent Access, suitable for data that's accessed less frequently.
- **GLACIER**: Very low-cost storage for archival data.
- **GLACIER_IR**: Glacier Instant Retrieval, for data that is rarely accessed but needs to be retrieved quickly.
- **DEEP_ARCHIVE**: The lowest-cost storage, but with slower retrieval times.

### Best Practices:
- **Use transitions wisely**: Move infrequently accessed data to lower-cost storage classes like **Standard-IA** or **Glacier**.
- **Set expiration for cleanup**: Expire or delete objects that are no longer needed to save storage costs.

With a lifecycle policy, you can automatically manage your S3 storage to optimize costs based on your data access patterns.

### Transitioning Objects Smaller than 128KB
By default, you cannot transition objects smaller than 128KB. This is done to avoid the unnecessary costs of transition. Check [S3 costs](/aws-s3-true-cost/) to get more details. To transition smaller objects, you need to allow a filter for `ObjectSizeGreaterThan` as shown below.


```json
{
  "Rules": [
    {
      "ID": "Move to Deep Archive after 365 days",
      "Filter": {
        "Prefix": "",
        "ObjectSizeGreaterThan": 1
      },
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 365,
          "StorageClass": "DEEP_ARCHIVE"
        }
      ],
      "NoncurrentVersionTransitions": [],
      "NoncurrentVersionExpiration": {},
      "AbortIncompleteMultipartUpload": {}
    }
  ]
}
```
