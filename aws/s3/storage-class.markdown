---
layout: post
title: S3 Storage Classes
subtitle: store data based on your needs
permalink: /aws-s3-storage-class/
leftmenu: left-menu/aws-s3.html
related: related-links/aws-s3.html
---

Amazon S3 provides a variety of storage classes tailored to different use cases. Each class comes with specific features like minimum object size, storage duration, pricing model, lifecycle management, and retrieval times. With eight distinct storage classes available, you can select the one that best meets your needs based on factors such as data access frequency, regulatory compliance, and other business-specific requirements. By choosing the appropriate storage class, you can optimize both storage costs and the performance of your applications.

Amazon S3 ensures exceptional durability by replicating object copies across multiple Availability Zones (AZs) within a single AWS Region. It is designed to provide **11 nines** (99.999999999%) of durability, protecting data in all storage classes from site-level failures, errors, and threats.

Data in Amazon S3 is distributed across at least three AZs within the same region, with each AZ located between 1 km and 100 km apart. This geographic separation safeguards your data against localized natural disasters. Amazon S3 guarantees fault tolerance, resiliency, and low latency, making it ideal for high-performance, latency-sensitive workloads.

### Storage classes

Amazon S3 provides various storage classes tailored to different data use cases. Each object is assigned a specific storage class, and a single bucket can contain objects across multiple classes. As your data needs or access patterns evolve, you can easily change the storage class of objects. Some classes come with minimum object size requirements or mandated storage durations. The screenshot below from the AWS Management Console shows the available storage classes. Switching between them is simple—just select the desired option and save.

It is important to analyse your data before choosing your storage tier. For example, you should consider the following questions. 

- **Service Levels**: What are the service-level expectations for data retrieval? Can users tolerate a five-hour delay, or do they need faster access?
- **Compliance and Storage**: Are there any compliance requirements or long-term data storage needs for your business or the data itself?
- **Performance Requirements**: What are the application's data performance needs? Do you require low-latency or millisecond-level response times?
- **Access Patterns**: Is data access predictable or unpredictable? How often is the data accessed—every minute, hour, daily, or just occasionally, such as once a year or during specific projects?
- **Data Access Patterns**: What are the general patterns of data access for the application?


