---
layout: post
title: IAM Roles Anywhere
subtitle: Connect to AWS with temporary credentials
permalink: /aws-iam-roles-anywhere/
leftmenu: left-menu/aws-iam.html
related: related-links/aws-iam.html
---

IAM Roles Anywhere is a powerful IAM capability that lets you provide third party applications and on-premises servers (non-AWS resources) access to you AWS accounts without using access keys. These permissions are temporary, can be easily changed or revoked if needed and it ties to the specific servers.

![IAM Roles Anywhere](https://shajam-s3.s3.amazonaws.com/images/aws/iam/iam-roles-anywhere-design.png)

 IAM Roles Anywhere uses X.509 certificates issued by your Certificate Authority (CA). When these non-AWS resources require access to AWS, these authenticate using certificates and  establish a trusted connection. 

### Configure IAM Roles Anywhere

To configure IAM Roles Anywhere, go to IAM Roles console page and scroll to the bottom of the page.

![Configure IAM Roles Anywhere](https://shajam-s3.s3.amazonaws.com/images/aws/iam/configure-iam-role-anywhere.png)

You then configure a trust anchor. The trust anchor contains the root certificate. You can setup a Certificate authority (CA) using AWS or use your own. In this example, I have setup a CA in my laptop and generated root certificate and client certificates.

![Configure trust anchor for IAM Roles Anywhere](https://shajam-s3.s3.amazonaws.com/images/aws/iam/trust-anchor-configuration.png)

Once you have the trust anchor setup, you can setup an IAM Role that will be used. You can provide any permissions needed. The important bit is the trust relationship. Here's an example.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Service": "rolesanywhere.amazonaws.com"
            },
            "Action": [
                "sts:AssumeRole",
                "sts:SetSourceIdentity",
                "sts:TagSession"
            ],
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalTag/x509Subject/CN": "local.example.com"
                },
                "ArnEquals": {
                    "aws:SourceArn": "arn:aws:rolesanywhere:ap-southeast-2:{AWS::AccountID}:trust-anchor/c3c2921d-****-????-b9d3-4240bbbb60ad"
                }
            }
        }
    ]
}
```

In the trust policy, note the service section. The **rolesanywhere.amazonaws.com**  allows using IAM Roles Anywhere. In the Condition section, note how I provided the server details. This the specific server’s name that is also available in the certificate (client certificate). This condition means only server with this name can use this IAM role. The source arn is the arn of the trust anchor you configured previously.

You also need to configure an IAM Role Anywhere Profile which allows you to pre-configure session policies which can scope down your role credentials to your target level of permissions. When profiles are set, only the permissions defined in the profiles are applied for that session. You can think of it as an additional permission boundary to the role.