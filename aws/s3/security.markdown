---
layout: post
title: S3 Security
subtitle: Review Security in S3
permalink: /aws-s3-security/
---

Amazon S3 (Simple Storage Service) is a highly scalable and durable object storage service provided by Amazon Web Services (AWS). S3 offers several security features to protect the data stored in it. Here are some of the security measures you can take to secure your data in AWS S3

### Encryption

S3 supports server-side encryption of objects using AWS-managed keys, AWS KMS (Key Management Service) customer master keys, or customer-provided encryption keys. You can also use client-side encryption to encrypt data before uploading it to S3.

### Access Control

Access control is a fundamental security feature in AWS S3. You can define who can access your data in S3 and what actions they can perform. You can use AWS Identity and Access Management (IAM) to create users and groups, and assign permissions to them.

### Bucket Policies

S3 bucket policies are an essential way to control access to your S3 buckets. You can use bucket policies to define who can access your buckets and what actions they can perform.

### Logging and Monitoring 

AWS S3 provides logging and monitoring features to help you detect and respond to security incidents. You can enable access logging for your S3 buckets to record all requests made to them. You can also use Amazon CloudWatch to monitor S3 metrics and set alarms for anomalous activity.

### Versioning

S3 versioning is a feature that allows you to store multiple versions of an object in the same bucket. This feature can help protect against accidental deletion or modification of objects.

### Secure Transfer

Secure transfer in AWS S3 refers to the process of securely transferring data between your client and S3 over a network connection. S3 provides several secure transfer options to protect data in transit, including:

- ***SSL/TLS encryption***: S3 uses SSL/TLS (Secure Sockets Layer/Transport Layer Security) encryption to encrypt data in transit between your client and S3. SSL/TLS is a widely used encryption protocol that provides a secure and encrypted connection.

- ***AWS Transfer Family***: AWS Transfer Family is a fully managed service that enables you to transfer files over Secure File Transfer Protocol (SFTP), File Transfer Protocol over SSL/TLS (FTPS), and Secure Shell (SSH) File Transfer Protocol (SFTP) directly into and out of Amazon S3. AWS Transfer Family automatically scales to meet your file transfer needs and provides a highly available architecture that can be integrated with your existing authentication systems.

- ***Amazon S3 Transfer Acceleration***: Amazon S3 Transfer Acceleration uses Amazon CloudFront's globally distributed edge locations to accelerate transfers over the public internet. When you enable S3 Transfer Acceleration, your data is routed through Amazon's network of edge locations, which can result in faster data transfer speeds and improved reliability.

### Block Public Access

Public Access can be disabled for existing and new buckets in S3. This makes sure that there is no public access. Recently, AWS has enabled this option by default and all new S3 buckets will have public access disabled. This option is powerful and overrides other permissions that allow public access to the bucket.

### Object Lock

S3 Object Lock feature allows storing objects in write-once-read-many (WORM) model. This prevents accidental deletion or modification of objects and ensures that objects remain immutable for a specified retention period.

S3 supports two different modes for Object Lock: Governance mode or Compliance mode.

- ***Governance mode*** enforces retention periods on objects, but it also enables authorized users to override or delete locked objects before their retention period expires.

- ***Compliance mode*** is designed for regulatory requirements and is a stricter mode than Governance mode. In Compliance mode, objects cannot be deleted or overwritten until their retention period has expired, even by authorized users.

S3 Object Lock works by applying a retention period to objects stored in S3. During the retention period, the objects cannot be deleted or overwritten. Once the retention period expires, the objects can be deleted or modified as usual.

S3 Object Lock can be used in many scenarios where data immutability is required, such as financial records, legal documents, medical records, and other sensitive data. By using S3 Object Lock, you can be confident that your data is secure and cannot be accidentally or maliciously modified or deleted.
