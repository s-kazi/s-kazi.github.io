---
layout: post
title: True Cost Of AWS S3
subtitle: to store 1 million files and 1TB of data
permalink: /aws-s3-true-cost/
leftmenu: left-menu/aws-s3.html
related: related-links/aws-s3.html
---

S3 is one of the fundamental services in AWS as you can safely store millions of files with unlimited storage. However, understanding the cost of S3 could be a pain as you pay for various API calls like `PUT`, `GET` and storage fees. In this page, we will discuss these costs. For the purpose of this exercise, let's assume we have 1 million objects and 1TB of storage, so, on average, a file is 1MB. 


### Choosing your AWS Region
You can choose an AWS region for price comparison but in reality you usually cannot as your application needs to be in the same region as your business is operating. Some region can be expensive, for example, `eu-west-1` and `us-west-2` has half the price than `ap-southeast-2` for Deep Archive. For calculating the costs, let's assume `us-west-2` region.


### Storage Cost

Storage cost is predictable. Standard storage is $0.023 per GB per month. So 1 TB of storage is $23.

```
    $0.023 * 1000 = $23 / month
```

### PUT Requests Pricing

PUT requests is the charge for ingesting files to S3. Essentially, are you charged for every file you send to S3. For 10 million files, the charge is $5.


```
    $0.005 per 1000 requests * 1 million = $5
```

### GET and HEAD Requests Pricing

GET pricing is the request fee for each file you request from S3. This fee is there regardless of whether it is requested from within AWS (like from EC2) or outside. So, if you have an application that requests each of the files once per month, you will be charged $4.

```
    $0.0004 per 1000 requests * 1 million = $0.4
```

If you need to perform other operations like HEAD requests, then the cost is similar to GET request.


### Moving to Deep Archive Cost

You can move the data to Deep Archive to reduce your storage cost. In us-west-2, your storage cost will reduce from $23 per TB to $1 per TB. That's a great saving if you do not need to access the files. You can move the files to Deep Archive using lifecycle policies. However, to move the files to Deep Archive, there is a fee per file. For 1 million files, it will be $500.

```
    $0.05 per 1000 requests * 1 million = $50  ## transition fee
```

### Other costs
There are other costs as well depending on what you want to do. For example, you can use intelligent tiering for automatically moving files between storage tiers, and there is associated costs for it. Also, you need to keep the files in Standard tier for a longer period of time. You can also lists the objects in a bucket and are charged like GET requests.

### Summary
In summary, S3 is an amazing service for storing files and the storage pricing is reasonable. However, the associated API calls are quite expensive. In our example, storing   TB data in S3 standard is $23 per month but the associated API costs for processing the files and moving to deep archive will cost close to $80.
