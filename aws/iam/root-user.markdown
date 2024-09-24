---
layout: post
title: IAM Root User
subtitle: How to protect Root User
permalink: /aws-iam-root-user/
leftmenu: left-menu/aws-iam.html
related: related-links/aws-iam.html
---

IAM is a core AWS service and that helps with access control to various AWS resources. Resources can be the entities like S3 bucket, RDS database, etc. IAM performs both authentication and authorisation for resources. AWS supports fine-grained access controls and IAM should be appropriately used to set the security permissions to achieve least privileged access.

### Root User
The root user is the first user that is automatically configured when you open an AWS account. The root user will exist until you close the account. The root user can be used for both console and CLI access (by using access keys). Note, it is not recommended to use root user for day-to-day operation. You should enable MFA for root user and disable or delete any access keys.

You need to use the root user for account setup related activities, billing modification, support upgrades. These cannot be performed by other IAM users.

### Best Practice to Protect Root User
The root user is important. If it is leaked, essentially, access to your entire AWS account is leaked. The root user can make any modification, delete resources and even close the account. Protecting the root user is super important.

- **Enable Multi-Factor Authentication (MFA)**: Like protecting any user account, you must protect root user with an MFA. This is critical and will stop most your console access related concerns.

- **Avoid Using Root User**: Not using the root user unless you have no other choice is ideal. Certain tasks can only be done by root user; otherwise do not use it.

- **Use IAM Users**: Use IAM users instead of root users. Using IAM users is also not ideal but if you don't have other choice, setup an IAM user with limited access and use it instead of root user.

- **Avoid Access Keys for Root User**: This is an absolute no. Do not ever setup access keys for root user. Setting up access keys also means to save it somewhere which can lead to leaked keys, for example, accidental check in to your source control like GitHub.

- **Use Strong Password**: For any password, always use strong passwords. Require number, upper/lower case letters, special characters and have minimum length to 8 character. Save the password in password manager or your company vault.  

- **Use SCP to restrict Root User**: You can use service control policies to restrict the use of root password. It is best to do so to limit the blast radius.

- **Check changes using AWS Config**: AWS Config checks and alerts for changes to your resources. Use Config rules to check for any changes to your root user and also IAM policies.

- **IAM Policies and Permissions**: You should conduct regular audits for your IAM policies. You should remove unnecessary permissions when not required. You can take advantage of IAM Access Analyser to identity unnecessary permissions.
