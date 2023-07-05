---
layout: post
title: Enable security using 
subtitle: AWS Security Group
permalink: /aws-vpc-security-group-security/
---

AWS security group is a virtual firewall that controls inbound and outbound traffic for instances in AWS. It acts as a firewall between instances and the internet, allowing administrators to specify which traffic is allowed to reach instances and which traffic is not.

AWS security group is a virtual firewall that control access to instances and outbound connections. Each security group has a set of inbound and outbound rules that filter traffic based on the protocol, port, and source or destination IP address. By default, all inbound traffic is blocked and all outbound traffic is allowed, but administrators can create custom rules to allow specific traffic.

Security groups can be used on EC2 instances, ECS services, lambda functions, application load balancers, RDS databases and most other compute related resources. Security groups are assigned to instances when they are launched, and instances can have multiple security groups assigned to them. They are region-specific and can be used to control traffic between instances in the same region, as well as traffic between different regions.

AWS security groups provide a powerful and flexible way to control network traffic in the cloud. They are a critical component of any AWS deployment and should be carefully configured to ensure that only authorized traffic is allowed to reach instances.

### How Security Group works
Security groups are stateful. That is, if a traffic is allowed inbound, then the reply outbound traffic is also allowed. The same applies for outbound traffic - if a traffic is allowed outbound, then the reply traffic is also allowed. This is achieved using connection tracking. Connection tracking is commonly found in network security solutions and firewalls. Security group connection tracking allows these security groups to keep track of the state of network connections, such as TCP and UDP sessions, and apply rules accordingly.

With connection tracking enabled, the security group can dynamically allow or deny traffic based on the established connections. It maintains information about ongoing connections, including source and destination IP addresses, ports, and protocol information. This information is used to verify if incoming traffic is part of an established connection or a new connection request.

The benefit of connection tracking is that it helps improve the security and efficiency of network traffic by allowing only legitimate connections and preventing unauthorized access. It provides granular control over network traffic by allowing or denying packets based on their relationship to existing connections. For example, if a packet is part of an established connection, it may be permitted, whereas a packet attempting to initiate a new connection that doesn't meet the defined rules may be blocked.

Connection tracking is bypassed if inbound or outbound traffic all traffic. For example, if all outbound traffic to 0.0.0.0/0 is allowed, then the inbound connection is not tracked. However, if the outbound rule is changed to specific ports or protocols or destination, then connection tracking will begin immediately.

Note that, there is a maximum limit on the number of connections in connection tracking. This stat is not visible in CloudWatch. It can be viewed from within the instance using *ethtool -S* command 

### How to set security group 
Let's say we have an EC2 instance and RDS database and we want to allow connectivity a security group. For EC2 instances, we want have the following rules.

<div>
<table class="table table-light table-striped">
<tr><th>Protocol</th><th>Port</th><th>Source IP</th><th>Description</th></tr>
<tr><td>TCP</td><td>80</td><td>0.0.0.0/0</td><td>Allow inbound HTTP traffic</td></tr>
<tr><td>TCP</td><td>443</td><td>0.0.0.0/0</td><td>Allow inbound HTTPS traffic</td></tr>
</table>
</div>

So, this EC2 allows traffic from anywhere on port 80 and port 443. Now, we need to consider whether the application in this EC2 is for internal (internal or private to a company) or for external public. Also, do you need both port 80 and 443, maybe just 443 will suffice or maybe port 80 is also to perform redirect to port 443. If this is an internal application, the security rules could be updated as following.

<div>
<table class="table table-light table-striped">
<tr><th>Protocol</th><th>Port</th><th>Source IP</th><th>Description</th></tr>
<tr><td>TCP</td><td>80</td><td>10.0.0.0/8</td><td>Allow inbound HTTP traffic</td></tr>
<tr><td>TCP</td><td>443</td><td>10.0.0.0/8</td><td>Allow inbound HTTPS traffic</td></tr>
</table>
</div>

Note that, this rule allows all traffic from 10.0.0.0/8 network. If you want to only traffic within the VPC, then it should be different since VPCs can have a maximum CIDR or /16.

By default, all outbound connections are allowed in a security group as shown below.

<div>
<table class="table table-light table-striped">
<tr><th>Protocol</th><th>Port</th><th>Destination IP</th><th>Description</th></tr>
<tr><td>All</td><td>All</td><td>0.0.0.0/0</td><td>Allow outbound traffic</td></tr>
</table>
</div>

This rule is the default, however, I wish this was made a conscious choice and not a default choice as you may not necessarily want to allow all outbound traffic. To avoid  this default outbound rule, you could do few things.

- Write a lambda function that removes the default outbound rule. The lambda can run at a schedule or can be invoked by AWS Config. I don't like this method, because, you might get a false impression that your deployment works only to find out that the rule has been removed and thus breaking an application. 
- Review security groups using Security Hub and let the responsible developer know to update the rule.
- Setup a default outbound rule that the developer must use. That will stop the default rule from being created. You can also update your CI CD process to enable this.

You can setup a minimum default outbound rule as per below.

<div>
<table class="table table-light table-striped">
<tr><th>Protocol</th><th>Port</th><th>Destination IP</th><th>Description</th></tr>
<tr><td>All</td><td>All</td><td>127.0.0.1/32</td><td>Minimum traffic to localhost</td></tr>
</table>
</div>