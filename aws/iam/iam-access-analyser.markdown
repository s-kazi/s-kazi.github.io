---
layout: post
title: IAM Access Analyser
subtitle: Analyse your IAM roles, policies
permalink: /aws-iam-access-analyser/
leftmenu: left-menu/aws-iam.html
related: related-links/aws-iam.html
---

IAM Access Analyser helps in refining permissions to achieve least privileged access. With access analyser, you can centrally review the permissions, refine permissions, and therefore apply least privileged access. For example, when enabled you can review all the unused permissions and take actions (maybe delete the permission).

![Configure IAM access analyser](https://shajam-s3.s3.amazonaws.com/images/aws/iam/access-analyser.png)

### Benefits of IAM Access Analyzer
IAM Access Analyzer provides several benefits for organizations looking to manage and secure access to their AWS resources. Here are the key benefits:

#### Identify Unintended Access
Detects resources that are shared with external entities or publicly accessible, allowing organizations to address unintended access promptly.

#### Access Insights
Offers detailed analysis and visualization of access paths, making it easier to understand and manage complex permissions.

#### Policy Recommendations
Provides actionable recommendations for tightening permissions and reducing the risk of providing more permissions to roles and users.

#### Least Privilege Principle
Helps enforce the principle of [least privilege](/aws-iam-least-privilege/) by identifying and eliminating unnecessary permissions.

#### Cross-Account Access
Provides insights into cross-account access, ensuring that resources are not unintentionally exposed to other AWS accounts.

#### Policy Validation
Ensures that IAM policies are in compliance with organizational security policies and best practices.

#### Continuous Monitoring
Continuously monitors for changes in permissions and access configurations, providing real-time insights into potential security issues.

### IAM Access Analyser cannot scan resource policies
The IAM Access Analyser cannot scan your resource policies. For example, it does not scan your S3 bucket policies and cannot provide feedback. 

By leveraging IAM Access Analyzer, organizations can achieve better control over their AWS environments, ensuring that access to resources is secure, compliant, and efficiently managed.
