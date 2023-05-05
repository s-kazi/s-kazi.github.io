---
layout: post
title: Security in AWS security Group
subtitle: Let's enable Security in Buckets and Objects
permalink: /aws-vpc-security-group-security/
---

AWS security group is a virtual firewall that controls inbound and outbound traffic for instances in AWS. It acts as a firewall between instances and the internet, allowing administrators to specify which traffic is allowed to reach instances and which traffic is not.

AWS security groups can be thought of as a set of firewall rules that control access to instances. Each security group has a set of inbound and outbound rules that filter traffic based on the protocol, port, and source or destination IP address. By default, all inbound traffic is blocked and all outbound traffic is allowed, but administrators can create custom rules to allow specific types of traffic.

Security groups can be used on EC2 instances, ECS services, lambda functions, application load balancers, RDS databases and most other compute related resources. Security groups are assigned to instances when they are launched, and instances can have multiple security groups assigned to them. They are region-specific and can be used to control traffic between instances in the same region, as well as traffic between different regions.

AWS security groups provide a powerful and flexible way to control network traffic in the cloud. They are a critical component of any AWS deployment and should be carefully configured to ensure that only authorized traffic is allowed to reach instances.

### How to set security group 
Let's say we have an EC2 instance and RDS database and we want to allow connectivity a security group. For EC2 instances, we want have the following rules.

<div>
<table class="table table-dark">
<tr><th>Protocol</th><th>Port</th><th>Source IP</th><th>Description</th></tr>
<tr><td>TCP</td><td>80</td><td>0.0.0.0/0</td><td>Allow inbound HTTP traffic</td></tr>
<tr><td>TCP</td><td>443</td><td>0.0.0.0/0</td><td>Allow inbound HTTPS traffic</td></tr>
<tr><td></td><td></td><td></td><td></td></tr>
<tr><td></td><td></td><td></td><td></td></tr>
<tr><td></td><td></td><td></td><td></td></tr>
</table>
</div>

So, this EC2 allows traffic from anywhere on port 80 and port 443. Now, we need to consider whether the application in this EC2 is for internal (internal or private to a company) or for external public. Also, do you need both port 80 and 443, maybe just 443 will suffice or maybe port 80 is also to perform  redirect to port 443. If this is an internal application, the security rules could be updated as following.

<div>
<table class="table table-dark">
<tr><th>Protocol</th><th>Port</th><th>Source IP</th><th>Description</th></tr>
<tr><td>TCP</td><td>80</td><td>10.0.0.0/8</td><td>Allow inbound HTTP traffic</td></tr>
<tr><td>TCP</td><td>443</td><td>10.0.0.0/8</td><td>Allow inbound HTTPS traffic</td></tr>
</table>
</div>

By default, all outbound connections are allowed in a security group as shown below.

<div>
<table class="table table-dark">
<tr><th>Protocol</th><th>Port</th><th>Destination IP</th><th>Description</th></tr>
<tr><td>All</td><td>All</td><td>0.0.0.0/0</td><td>Allow outbound traffic</td></tr>
</table>
</div>