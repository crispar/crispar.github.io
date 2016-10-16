---
published: true
title: AWS ACHITECTURE FILE SYNCRONIZATION
layout: post
author: jungwook
category: aws
tags:
- aws
- architecture
---

## FLOW

1. The file synchronization service endpoint consists of an `ELB`(Elastic Load Balance) distributing incoming requests to a group of application servers hosted on `EC2`(Amazon Elastic Compute Cloud) instances. An `Auto Scailing` group automatically adjusts the number of `EC2` instances depending on the applicatino needs.

2. To upload a file, a client first needs to request the permission to the service and get a security token.

3. After checking the user\`s identify, application server get a temporary credential from `STS`(Amazon Security Token). This credential allows users to upload files.

> **What is STS?**
>
> The AWS Security Token Service is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management(IAM) users or for users that you authenticate (federated users)
>
> When requested for Access through an STS API call it would typically return Temporary Security credentials consisting of :
>
> + Security Token
> + An Access Key ID
> + A Secret Access keys
>
> The access Key ID & Secret Key generated with the token cannot be used without the token.
>
> There are No Limits on the nubmer of "Sets" that we can create.
>
> STS service is designed to have limited access on a couple of Services.

>**So who is a Federated User?**
>
> A non-AWS user whose identity can be authentificated.

4. User upload files into `Amazon Simple Storage Service`(Amazon S3), a highly durable storage infrastructure designed for mission-critical and primary data storage. **Amazon S2** makes it easy to store and retrieve any amount of data, at any time. Large files can be uploaded by the same client using multiple concurrent threads to maximize bandwith usage.

5. File metadata, version information, and unique identifiers are stored by the application server on and `Amazon DynamoDB` table. As the number ofo files to maintain in the application grows, `Amazon DynomoDB` tables can store and retrieve any amount of data, and serve any level of traffic.

6. File change notifications can be sent via email to users following the resource with `Amazon Simple Email Service` (Amazon SES), an easy-to-use, cost-effective email solution.

7. Other clients sharing the same files will query the service endpoint to check if newer versions are available. This query compares the list of local files checksums with the checksum listed in an `Amazon DynamoDB` table. If the query finds newer files, they can be retrieved from `Amazon S3` and sent to the client application.

- **Amazon Elastic Compute Cloud**
- **Auto Scaling**
- **Elastic Load Balancing**
- **Amazon Route 54**
- **Amazon DynamoDB**
- **Amazon Simple Storage Service**
- **AWS Security Token Service**
- **Amazon Simple Email Service**

