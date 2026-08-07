---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Amazon SQS – The Key to Asynchronous Architecture on AWS

During my internship and while exploring serverless services on AWS, I published a technical blog about Amazon Simple Queue Service (Amazon SQS) on the AWS Study Group community. The blog introduces the role of Amazon SQS in building Asynchronous Architecture, highlighting how it improves application scalability, reduces system workload, and enhances reliability.

The main topics covered in the blog include:

* An introduction to Amazon SQS and the fundamentals of message queues.
* An explanation of the asynchronous processing model by integrating Amazon API Gateway, Amazon SQS, and AWS Lambda.
* The key benefits of Amazon SQS, including Loose Coupling, Scalability, and built-in retry mechanisms for handling failures.
* Best practices for implementing Amazon SQS, such as using Dead Letter Queues (DLQ), configuring Visibility Timeout, and designing AWS Lambda functions following the Idempotent principle.
* An architecture diagram demonstrating an asynchronous processing workflow using Amazon API Gateway, Amazon SQS, AWS Lambda, Amazon S3, and Amazon DynamoDB.

Through this blog, I strengthened my understanding of Amazon SQS and gained deeper insight into designing scalable, reliable, and serverless applications on the AWS Cloud.

## Illustration

![overview](images/3-BlogsPosted/blog_1.png)

## Blog Link
[Read the article on AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2234076147357345&hoisted_section_header_type=recently_seen&__cft__[0]=AZbn73yW8KNKYQgwxHrpiV3agA4I1MOTX3p8eQ7Sj0V-jexe8G1iLBdySt-X0l7HN4mNckwnvLGcs2ttzAa0OoZ-Zr0OO51kfS2EaKSoOkRsbjxodd2ztOzZVpeqhruT95_EuG5pssrW3WjsCjhjKhlt6qNfKYial3JlxO6VesbUg8RUNPmOy3wq100eG2LrZjE&__tn__=%2CO%2CP-R)