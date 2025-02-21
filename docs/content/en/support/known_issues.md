---
title: "Known Issues"
linkTitle: "Known Issues"
description: "Information on known issues in Wavestack services."
type: "docs"
weight: 2
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2024 Wavecon GmbH -->


## Object Storage AWS SDK Incompability 

Object Storage customers who use a current version (released on Jan. 15, 2025)
of the AWS CLI or an AWS SDK or use a tool that is based on such a version must
expect problems uploading files to Object Storage RGW S3 in all regions.

The reason for this is a change in the AWS libraries from January 15, 2025, 
which requires the use of “Data Integrity Protection for Amazon S3” [1] [2]

The recommended solution for now is to revert to an older version of the 
AWS CLI or AWS SDK, or use another tool compatible with Object Storage.

- [1 Data Integrity Protections for Amazon S3](https://docs.aws.amazon.com/sdkref/latest/guide/feature-dataintegrity.html)
- [2 Github Issue 9214](https://github.com/aws/aws-cli/issues/9214)


## If there are any other issues present

Our team is continuously monitoring and ensuring that everything 
functions smoothly. Should you encounter any problems or require assistance, 
please do not hesitate to [contact our support](/support/).


