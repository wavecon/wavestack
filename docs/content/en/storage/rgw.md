---
title: "Swift/RGW S3 API"
linkTitle: "Swift/RGW S3 API"
description: "Create object storage buckets/containers using Swift/Ceph-RGW S3 API"
type: "docs"
weight: 1

cascade:
- _target:
    path: "/**"
    kind: "page"
  type: "docs"
- _target:
    path: "/**"
    kind: "section"
  type: "docs"
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Overview

This guide details how to use Ceph Object Gateway S3 API for managing
object storage on Wavestack.

Specifically, you will learn how to:

- Create a credential for authentication with the Object Gateway S3 API
- Create a bucket/container
- Upload data to the bucket/container
- Access data in the bucket/container
- Manage data in the bucket/container
- Delete a bucket/container

## Prerequisites

In order to follow this guide, the following tools have to be installed:

- [openstackclient][openstackclient-getting-started]

### Compatibility

The steps in this guide have been tested with the following versions:

|                                    | Version  |
|------------------------------------|----------|
| openstackclient                    | v6.2.0   |
| openstackclient                    | v6.6.0   |

## Storage Placements

We offer two different kind of storage placements, which represent different backend types:

{{% alert color="info" %}}

It is important, that you need to select the correct placement, if it should not be in default-placement, on bucket creation time.
It can not be changed afterwards.

s3cmd example:

s3cmd mb --bucket-location=":<PLACEMENT>" s3://my-test-bucket

openstack example:

openstack container create --storage-policy <PLACEMENT> my-test-bucket

{{% /alert %}}

| Placement                          | Backend Type    |  Cloud Points  | Information                                    | 
|------------------------------------|-----------------|----------------|------------------------------------------------|
| default-placement                  | MIXED HDD+FLASH | 1 per GB/Month | Should be best for most usecases               | 
| express-onezone-placement          | FLASH Only      | 4 per GB/Month | Experimental, high performance, limited access |

{{% alert color="warning" %}}

Please be aware, that "express-onezone-placement" is experimental for now and we may set a quota of 10TB on your bucket.

{{% /alert %}}

## Access the Wavestack dashboard

You will probably spend most of your time on the command line, but you
can also log into the [Wavestack dashboard][wvst-dashboard] with your
Wavestack account if you want to get an overview of the provisioned
resources.

### About the Wavestack object storage

The Wavestack object store is built using Ceph and distributed across
three availability zones, namely muc5-a, muc5-b and muc5-d. Object
data is thus redundantly replicated across all 3 AZs.
The endpoint for the MUC5 RGW is `rgw.muc5.wavestack.de`.

Your buckets (or containers) are bound to your OpenStack project via
[Keystone RGW Tenants][keystone-rgw-multitenancy].

You can access the Wavestack object store with either the `openstackclient`,
or an S3 API-compatible tool.

For more detailed information on the features of the S3-API,
please refer to [Ceph S3 Documentation](https://docs.ceph.com/en/latest/radosgw/s3/).
This includes instructions on how to use advanced features such as object lock, immutability, 
and WORM, which can be crucial for implementing compliance and data retention policies.

{{% alert color="info" %}} Some clients (like GitLab) and SDKs need the signature version of 
the S3-API hard-set to 2 on the client's side to ensure maximal compatibility.{{% /alert %}}

### EC2 Credentials

To access the S3 API with any other client than the Openstackclient like [AWS CLI][awscli]
it is necessary to generate an EC2 credential [as described here][keystone-rgw-auth].

For this we utilise the [openstackclient][openstackclient-ec2-credentials]:

```cli
❯  openstack ec2 credentials create
```

### Creation of Buckets/Containers

To [create a bucket/container][openstackclient-container] you run:

```cli
❯ openstack container create <containername>
```

### Handling Data in Buckets/Containers

You can [handle objects in your containers][openstackclient-object] by
utilising the openstackclient. For example you can upload one or many
objects with the following command:

```cli
❯ openstack object create <container> <file> [<file2> <file3>  ...]
```

In a similar vein you can list data in your container by running the
following command:

```cli
❯ openstack object list <container>
```

Download objects to your local machine like so:

```cli
❯ openstack object save <container> <object>
```

If you wish to delete one or multiple object from your container
you can delete objects with the following command:

```cli
❯ openstack object delete <container> <object> [<object> ...]
```

### Deletion of Buckets/Containers

Should you want to delete one or multiple containers you can utilise
the openstackclient for that task as well.

It should be noted that a container is required to be empty before it
can be deleted. Alternatively you can delete the container and content
at the same time by passing the `--recursive` flag.

```cli
❯ openstack container delete [--recursive] <container> [<container> ...]
```

<!-- References -->

[awscli]: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
[keystone-rgw-auth]: https://docs.ceph.com/en/latest/radosgw/s3/authentication/#authentication-against-openstack-keystone
[keystone-rgw-multitenancy]: https://docs.ceph.com/en/latest/radosgw/multitenancy/#swift-with-keystone
[openstackclient-container]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/container.html
[openstackclient-ec2-credentials]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/ec2-credentials-v3.html
[openstackclient-getting-started]: https://github.com/openstack/python-openstackclient#getting-started
[openstackclient-object]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/object.html
[wvst-dashboard]: https://dashboard.wavestack.de/
