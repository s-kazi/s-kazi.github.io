---
layout: post
title: sqs
permalink: /aws-sqs/
---

AWS SQS (Simple Queue Service) is a fully managed message queuing service provided by Amazon Web Services (AWS). It enables the decoupling and scaling of microservices, serverless applications, and distributed systems by allowing messages to be exchanged between independent components.

SQS is designed to provide a reliable and scalable way to transmit messages between different parts of a distributed application, without requiring each component to be available or online at the same time. It allows components to communicate asynchronously with each other, which can improve application reliability and performance.

SQS works by storing messages in a distributed queue, which can be accessed by applications using the AWS SDK or API. Messages can be sent to the queue by producers, and then retrieved and processed by consumers. This allows components to operate independently and at their own pace, without needing to wait for other components to complete their tasks.

AWS SQS supports two types of message queues: standard and FIFO. Standard queues provide high throughput, while FIFO queues ensure that messages are processed in the order they are received. Both types of queues can be used for a variety of use cases, such as batch processing, event-driven architectures, and message-based systems.
