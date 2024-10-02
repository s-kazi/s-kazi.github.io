---
layout: post
title: Design Network in AWS 
subtitle: using AWS VPC
permalink: /aws-vpc/
leftmenu: left-menu/aws-vpc.html
related: related-links/aws-vpc.html
---

### AWS VPC (Amazon Virtual Private Cloud)

**Amazon Virtual Private Cloud (VPC)** is a service that lets you provision a logically isolated section of the Amazon Web Services (AWS) cloud where you can launch AWS resources, such as EC2 instances, in a virtual network that you define. With VPC, you have complete control over your virtual networking environment, including IP address ranges, subnets, route tables, internet gateways, and network access control lists (ACLs).

### Key Components of AWS VPC

1. **Subnets:**
   - Subnets are subdivisions within a VPC that let you segment your resources. Each subnet maps to a specific **Availability Zone**.
   - You can have **public subnets** (accessible from the internet) and **private subnets** (isolated from the internet).

2. **CIDR Blocks:**
   - When you create a VPC, you define an IP address range using **Classless Inter-Domain Routing (CIDR)** notation (e.g., `10.0.0.0/16`).
   - This defines the size of the network and how many IP addresses can be assigned.

3. **Route Tables:**
   - Each subnet in a VPC is associated with a route table that defines how traffic is routed within the VPC or to external networks.
   - **Public subnets** have route tables that direct traffic to the internet through an **Internet Gateway**, while **private subnets** do not.

4. **Internet Gateway (IGW):**
   - An Internet Gateway allows resources in your VPC to access the internet. It connects the VPC to the outside world.
   - Resources in public subnets can have public IPs and communicate with the internet via this gateway.

5. **NAT Gateway (Network Address Translation):**
   - A NAT Gateway enables resources in private subnets to access the internet for outbound traffic (e.g., updates or downloading software) without exposing them to inbound internet traffic.

6. **Elastic IP Addresses (EIPs):**
   - These are static IP addresses associated with your AWS account. You can assign an Elastic IP to EC2 instances, load balancers, or NAT gateways in your VPC for consistent public IP addresses.

7. **Security Groups:**
   - These act as virtual firewalls for your instances, controlling inbound and outbound traffic at the instance level. You can create rules to permit or deny traffic.

8. **Network ACLs (Access Control Lists):**
   - NACLs act as a firewall at the **subnet** level, providing another layer of security. You can control inbound and outbound traffic by allowing or denying specific IP ranges.

9. **VPC Peering:**
   - VPC Peering allows you to connect two VPCs privately using AWS's network. Instances in the peered VPCs can communicate as if they were within the same network.

10. **VPN Gateway (Virtual Private Gateway):**
    - This enables secure communication between your on-premises network and your AWS VPC via an IPsec VPN connection.

11. **Direct Connect:**
    - AWS Direct Connect allows you to establish a dedicated, private network connection between your on-premises data center and AWS VPC. This provides a more consistent network experience compared to internet-based connections.

12. **Endpoints:**
    - **VPC Endpoints** allow you to connect your VPC to supported AWS services, like S3 or DynamoDB, without routing traffic over the public internet.

### Use Cases of VPC

- **Isolated Environment for Applications:**
  - Deploy applications that require secure, isolated environments. You can have both public-facing and private components in the same VPC.
  
- **Hybrid Cloud:**
  - Use VPN Gateway or Direct Connect to integrate your on-premises infrastructure with AWS for a hybrid cloud setup.

- **Security & Compliance:**
  - VPC enables granular control over your network configuration, helping meet security and compliance requirements for industries like finance, healthcare, etc.

- **Multi-Tier Architecture:**
  - You can host a multi-tier web application by deploying the web servers in a public subnet and database servers in a private subnet.

### Benefits of AWS VPC

1. **Full Control Over Network:**
   - You control IP addressing, subnets, routing tables, and gateways, providing you with a secure, configurable virtual network.

2. **Isolation and Security:**
   - Resources in a VPC can be fully isolated from the public internet or configured to limit exposure, offering strong security.

3. **Customization:**
   - VPC offers flexibility to create different network configurations like public and private subnets, security groups, and peering connections.

4. **Cost-Efficiency:**
   - By using private IP addresses and efficient subnets, you can limit your public-facing resources and avoid unnecessary costs.

AWS VPC provides a scalable and flexible foundation for building secure, isolated, and highly available cloud applications.