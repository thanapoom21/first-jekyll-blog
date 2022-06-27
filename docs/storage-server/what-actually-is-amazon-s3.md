---
layout: default
title: "Amazon S3?"
parent: Storage & Server
nav_order: 1
description: "Amazon Web Services"
---

We might have heard the terms "AWS" or "Amazon S3" but not really sure what they are capable of doing.

In this article, I'll talk about one of the Amazon Web Services - AWS or Amazon S3.

Imaging if you can store all your stuffs on the cloud. Literally, I mean the cloud in the sky. It's huge and it can move to anywhere in the world. However, it turns out that we can only store our data in the cloud. There's a ton of data out there that we can use and store. Amazon S3 gives us a chance to do that and it's packed with a lot of features and plenty of options to choose from.

You can choose between Standard, Standard - Infrequent Access Storage, or Reduced Redundancy Storage.

However, [On-Demand Storage][on-demand-storage] is another way you can store your data on your own budget. It lets you pay for storage by GB with no long-term commitments freeing you from the cost and effort of planning, estimating and purchasing storage capacity ahead of time.

|-----------------+------------+-----------------+----------------|
|  | **Standard Storage** | **Standard - Infrequent Access Storage†** | **Glacier Storage**  |
|-----------------|:-----------|:---------------:|---------------:|
| First 50 TB / month | $0.0390 per GB | $0.0200 per GB | $0.006 per GB |
| Next 450 TB / month | $0.037 per GB | $0.0200 per GB | $0.006 per GB |
| Over 500 TB / month | $0.0355 per GB | $0.0200 per GB | $0.006 per GB |
|-----------------+------------+-----------------+----------------|

† Standard – Infrequent Access storage has a minimum object size of 128KB. Smaller objects will be charged for 128KB of storage.

[amazons3-faqs]:https://aws.amazon.com/s3/faqs/
[on-demand-storage]:https://aws.amazon.com/govcloud-us/pricing/s3/
