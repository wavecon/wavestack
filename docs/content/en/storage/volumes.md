---
title: "Volumes"
linkTitle: "Volumes"
description: "Block Storage Volumes on Wavestack"
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

Similar to compute instances, volumes in Wavestack have specific
types. However, there are not as many different volume types as there
are instance flavours. Volume types you allow to create volumes with
different properties and abilities.

## Volume Types on Wavestack

Execute the following [openstackclient command][openstackclient]
to list all currently available volume types:

```cli
❯ openstack volume type list
```

Currently, these types are:

| Volume Type | Description                                      |
|------------|--------------------------------------------------|
| rbd\_fast  | rbd_fast is our storage class for full flash block storage, based on Ceph RBD. It is a generally usable volume that is specially designed for applications with high input/output requirements. Thanks to the full flash architecture, it enables extremely fast data processing and has very low latencies. For data security and reliability, the data is replicated three times, providing high data redundancy.         |
| LUKS       | Similar to rbd\_fast, but with automated encryption added on top for enhanced data security. |

### Local SSD/NVMe Storage
For quick and low-latency workloads, local SSD/NVMe backed storage is recommended. Choose an SCS-standard flavor with 'p' (NVMe) or 's' (SSD) at the end of its name. For example, SCS-2V-8-20**s** signifies 20GB local SSD storage. Be aware that local storage is ephemeral, meaning data gets lost if the instance is deleted. Information on the instances offered can be found here: [Instance types](/compute/instance_types)

### Storage Types / IOPS

Performance attributes differ between storage/disk types, which are
summarised in the table below:

| Volume Type | Average IOPS | Burst IOPS | Average Throughput | Burst Throughput |
|-------------|--------------|------------|--------------------|------------------|
| rdb\_fast   | 3000         | 10000      | 250 MB/s           | 500 MB/s         |
| ssd         | 5000         | 20000      | 500 MB/s           | 1000 MB/s        |

{{% alert color="success" %}}
Note: With sufficient retrieval size, we can provide more than 15,000 IOPS on volumes. Please contact our [support team](/support/) for more information.
{{% /alert %}}

## Managing volumes

OpenStack enables the user to manage their data volumes via the Cinder
block storage service. Wavestack utilizes one Ceph RBD Cluster in each
availability zone to provide decoupled and fast storage for instances.

#### Creating a volume

Volumes are created by running the following openstackclient command:

```cli
❯ openstack create volume --size <size-in-GB> <volume-name>
```

{{% alert color="info" %}}
Omitting the name will create a volume with the same name as its UUID.
{{% /alert %}}

{{% alert color="info" %}}
If you wish to prevent forensic recovery of your data you must use
the encrypted volume type (LUKS) for volume creation.
{{% /alert %}}

You can also add further parameters to the [cinder volume create
command][cinder-volume-creation]. The following ones are commonly used:

- `--image` specifies which machine image to replicate the volume from
- `--snapshot` specifies which volume snapshot to replicate the volume from
- `--source` specifies which machine image to replicate the volume from


#### Creating an encrypted volume

To secure user data, volumes can be created with LUKS encryption.
OpenStack Barbican makes it easy to automatically create encrypted
volumes by utilizing the [LUKS volume type][cinder-volume-encryption]:

```cli
❯ openstack volume create --size <size-in-GB> --type LUKS <volume-name>
```
All of the above options and parameters for volume creation still apply.


#### Deleting a volume

If a volume is no longer needed, simply run [openstack volume
delete][cinder-volume-deletion]:

```cli
❯ openstack delete volume <volume>
```

To delete the volume. You can also add the `--purge` parameter
to the command to delete all snapshots of the volume.

#### Extending a volume

Volumes can be easily extended by setting a higher image size:

```cli
❯ openstack volume set --size <new-size-in-GB>
```

## Managing volume data

It is quite essential to create snapshots and backups of your volumes
to be able to revert data to a previous state.

OpenStack Cinder addresses this by providing the user with the
possibility to [snapshot and/or backup
volumes][cinder-backup-snapshot-info]:

#### Creating/deleting volume snapshot

It is easy to create a quick snapshot of a volume on the command line.
These snapshots can be used to quickly restore a volume to a previous
state.

```cli
❯ openstack volume snapshot create --volume <volume> <snapshot-name>
```

A snapshot can even be created of a volume that is currently in use by
a running machine.

{{% alert color="info" %}}
In some cases creating a snapshot from an attached volume can result
in a corrupted snapshot!
{{% /alert %}}

Thus, if you want to create such a snapshot, it is required to provide
the `--force` flag to the command.

#### Creating/deleting volume backup

Volumes as well as volume snapshots can be backed up by utilizing the
capabilities of the Cinder-Backup Service. Cinder-Backup saves backups
to a pool inside another storage cluster which spans over all 3
availability zones. This way Wavestack ensures backup data high
availability.

To create a volume backup, simply run:

```cli
❯ openstack volume backup create --name --volume <volume>
```

Similar to snapshots, you have to provide the `--force` flag when
creating a backup from an in-use volume (i.e. one that is attached to
an instance).

For further information on backup options view the [OpenStack
Docs][cinder-volume-backup].


## Transferring a volume

It is also possible to [transfer your volumes][cinder-volume-transfer] between
projects, which allows the user to copy and share data between subprojects
within their own organization.

First, a volume transfer request is created:

```cli
❯ openstack volume transfer request create <volume-uuid>
```

This creates a transfer request with an auth-key (printed to your
stdout).

You can then get a list of currently pending transfer requests by
running:

```cli
❯ openstack volume transfer request list
```

and get the auth-key for a request by running:

```cli
❯ openstack volume transfer request show <transfer-id>
```

This transfer request needs to be accepted by a user authenticated
within the other project, providing the transfer request ID and the
auth-key:

```cli
❯ openstack volume transfer request accept --auth-key <key>  <transfer-request-id>
```

<!-- References -->

[cinder-backup-snapshot-info]: https://docs.openstack.org/cinder/latest/admin/volume-backups.html
[cinder-volume-backup]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/volume-backup.html
[cinder-volume-creation]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/volume.html#volume-create
[cinder-volume-deletion]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/volume.html#volume-delete
[cinder-volume-encryption]: https://docs.openstack.org/cinder/latest/configuration/block-storage/volume-encryption.html
[cinder-volume-extension]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/volume.html#volume-set
[cinder-volume-snapshot]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/volume-snapshot.html
[cinder-volume-transfer]: https://docs.openstack.org/python-openstackclient/latest/cli/command-objects/volume-transfer-request.html
[openstackclient]: https://docs.openstack.org/python-openstackclient/latest/cli/command-list.html
