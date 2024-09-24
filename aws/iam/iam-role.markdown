---
layout: post
title: IAM Roles
subtitle: Best practice using IAM roles
permalink: /aws-iam-role/
leftmenu: left-menu/aws-iam.html
related: related-links/aws-iam.html
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

#### Key Features of Service-Linked Roles

**Predefined Permissions**:
   - Service-linked roles come with predefined policies that grant the necessary permissions for the service to function correctly.

**Service Management**:
   - AWS services automatically configure and manage these roles. This ensures that the correct permissions are applied and simplifies role management for users.

**Secure and Isolated**:
   - These roles are used only by the linked service, ensuring that permissions are not misused or accessed by other services or users.

### Configuration

**Automatic Configuration**:
   - AWS services automatically configure service-linked roles when you enable the service or a specific feature that requires the role. For example, enabling AWS Config might automatically configure a service-linked role with permissions to record and manage resource configurations.

**Manual Configuration**:
   - You can also manually configure a service-linked role. AWS provides specific instructions for each service on how to do this. This lets control which service linked roles are configured in your account. Using service catalog, you can deploy these roles simultaneously instead of configuring in individual accounts.

**Deletion**:
   - These roles can only be deleted if the service that uses them no longer requires them. AWS prevents the deletion of service-linked roles that are still in use to avoid disrupting service functionality.

### Examples of Service-Linked Roles

**Amazon EC2**:
   - The `AWSServiceRoleForEC2Spot` role allows Amazon EC2 Spot instances to communicate with other AWS services on your behalf.

**AWS Config**:
   - The `AWSServiceRoleForConfig` role enables AWS Config to record and manage configurations of supported resources.

### Benefits of Service-Linked Roles

**Simplified Permissions Management**:
   - By automatically creating and managing these roles, AWS reduces the complexity and potential errors in manually assigning permissions.

**Enhanced Security**:
   - Since the roles are managed and used only by the specific AWS service, the risk of improper access or privilege escalation is minimized.

**Audit and Compliance**:
   - These roles are designed to meet AWS security best practices and compliance requirements, helping organizations maintain a secure and compliant environment.

### How to View and Manage Service-Linked Roles

**Viewing Roles**:
   - You can view service-linked roles in the IAM console by navigating to the "Roles" section. Service-linked roles are marked with a specific service identifier.

**Role Policies**:
   - The policies attached to service-linked roles can be viewed but are managed by the AWS service. You can review these policies to understand what permissions are granted.

**Troubleshooting**:
   - If a service-linked role is not working correctly, ensure that the AWS service is enabled and configured properly. AWS documentation provides troubleshooting steps specific to each service.


### IAM Permission Boundary ###
Permission Boundary is essentially a [guardrail that you can configure for the role and user permissions](/aws-iam-least-privilege/). This defines the maximum permissions that the IAM resource can have. For example, you can configure a permission boundary with only S3 and SQS permissions. Now if you configure a role with permissions to EC2 and attach the permission boundary, the role will still not be able to use the EC2 permission (*blocked by permission boundary*).

The permission boundary is useful when you for example let a team configure the IAM roles but want to make sure that they only play within the boundary.
