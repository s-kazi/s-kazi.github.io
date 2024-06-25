---
layout: post
title: IAM Roles
subtitle: Best practice using IAM roles
permalink: /aws-iam-role/
leftmenu: left-menu/aws-iam.html
---

IAM roles are identities configured on an AWS account. It is like IAM users. It is used to provide AWS resource access to other AWS services. For example, if write a Lambda function that will write data to a S3 bucket, you need to give access to Lambda to write to S3. This permission can be given by using IAM roles. 

AWS generates temporary credentials (security tokens) using AWS Security Token Service (STS) to the service using IAM role. These security tokens are only valid for the duration of the session (short time). Thus, it is better than IAM users access keys, which are long-lived. IAM roles can also be assigned to multiple resources that require the same access. For example, you might want all your EC2s to have permissions for SSM login and one IAM role will be sufficient to give this access.

### IAM Policy ###

IAM policies are JSON documents that define the permissions of a resource. AWS supports JSON for composing the policy document. You may write CloudFormation or CDK using different language that eventually translates into JSON.

By default, all permissions are denied and if you somehow set permissions at different levels, then the more restrictive permissions apply. Any DENY will be overwrite any ALLOW for the same resource or permission.

An IAM policy has the following structure with minimal elements.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "StatementID",
      "Action": [
		"list_of_permissions"
      ],
      "Effect": "Allow",
      "Resource": "ARN of the resource or * to specify all resource"
      }
    }
  ]
}
```

The **Condition** can be used to check for additional properties that limits the permission. For example, it can be used to check for tags or source IP, etc. It can check for string, numeric, datetime and boolean properties. Following is an example of a policy that specifies that objects in S3 can only be downloaded over TLS.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt987654",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::my-bucket/my_prefix/*",
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "false"
        }
      }
    }
  ]
}
```

### Attach IAM Policy to IAM Role ###
Attaching policies to IAM roles is similar but slightly different to attaching it for IAM users and user groups. When creating the role, you need to select the AWS service, account or federation that can use this role. This is defining the trust policy.

   ![Select trusted entity](https://shajam-s3.s3.amazonaws.com/images/aws/iam/iam-role-trust.png)

In this example, I selected Lambda as the trusted service. In the next section, you can select the permission/policies that will apply to the role. In the last section, you can enter the name and description for the role.

   ![Configure permissions IAM role](https://shajam-s3.s3.amazonaws.com/images/aws/iam/configure-role.png)

You can view the trusted policy and verify or edit the permissions. You can optionally add tags and configure the role.

The trust policy essentially defines which AWS service can assume the role. For example, the demo-role configured, we cannot use this role with an EC2 instance. It can only be used with Lambda.


### AWS Managed, Customer Managed and Inline Policies ###
AWS Managed policies are policies configured by default by AWS in an AWS account. These policies are administered by AWS, and it makes it easy to configure role, users, or groups with these policies. These policies cannot be modified by users.

*Customer managed* policies are the ones that you have configured. You can manage, edit, or delete these policies. Essentially, these policies are your responsibility. 

Both AWS managed and customer managed policies can be applied to several users, groups, or roles. Note that, these have different ARN formats. For example, AWS managed policy can have `arn arn:aws:iam::aws:policy/AmazonS3FullAccess`.
which will be same on all AWS accounts. Customer managed policies can have ARN like `arn:aws:iam::987654321012:policy/MyPolicy`.

Notice the account number in the policy. Thus, it is easier to use AWS managed policies when assigning these to IAM roles, etc.
Inline policies are policies defined inline within the user, group, or role. These policies have one-to-one relation with resource it is attached to. When you modify a role, you can attach the inline policy through the console.

*Inline policies* are policies defined inline within the user, group, or role. These policies have one-to-one relation with resource it is attached to. When you modify a role, you can attach the inline policy through the console.

![Setup Inline Policies for IAM Role](https://shajam-s3.s3.amazonaws.com/images/aws/iam/inline-policy-for-role.png)


### Service Linked IAM Roles ###
Service Linked IAM Roles are unique and are directly linked to an AWS service. These predefined roles help define the permissions that an AWS service will need to function. These roles are only setup when you start using the service. You can view but not edit the permissions of these roles. 

For example, when you use ECS (container service), a service linked role is configured with ARN like `arn:aws:iam::987654321012:role/aws-service-role/ecs.amazonaws.com/AWSServiceRoleForECS`.

### IAM Permission Boundary ###
Permission Boundary is essentially a guardrail that you can configure for the role and user permissions. This defines the maximum permissions that the IAM resource can have. For example, you can configure a permission boundary with only S3 and SQS permissions. Now if you configure a role with permissions to EC2 and attach the permission boundary, the role will still not be able to use the EC2 permission (*blocked by permission boundary*).

The permission boundary is useful when you for example let a team configure the IAM roles but want to make sure that they only play within the boundary.
