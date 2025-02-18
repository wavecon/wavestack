---
title: "Instance types"
linkTitle: "Instance types"
description: "Instance types on Wavestack"
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

Every compute instance running in Wavestack has a specific type, or
*flavour* in OpenStack parlance. The type defines various combinations
of instance attributes such as number of virtual CPU cores, amount of
memory, network or storage capacity.

You can choose between a wide variety of instance types on Wavestack,
which allows you to tailor your selection to your specific needs.

Execute the following command to list all currently available instance
types/flavours:

```cli
❯ openstack flavor list
```

We follow the Sovereign Cloud Stack naming standard. If you want to
learn more about it and understand the logic behind these names, you
can read more about it in:

- [SCS - Flavor Naming Standard v3][gh-scs-standards-v3-flavor-naming].

## General Purpose

General purpose instances provide a balanced ratio of virtual CPU
cores and memory. They are a good choice for a wide variety of use
cases unless your workloads have more specific requirements.

Typical workloads:

- Web servers
- Business intelligence applications
- Containerised services
- Development and test environments

|                  | vCPU | Memory | Disk | Disk Type | Average Bandwidth | Peak Bandwidth |
| ---------------- | ---- | ------ | ---- | --------- | ----------------- | -------------- |
| SCS-2V-8         | 2    | 8      | 0    | rbd\_fast | 0.512             | 1              |
| SCS-2V-8-20      | 2    | 8      | 20   | rbd\_fast | 0.512             | 1              |
| SCS-4V-16        | 4    | 16     | 0    | rbd\_fast | 1.25              | 2.5            |
| SCS-4V-16-50     | 4    | 16     | 50   | rbd\_fast | 1.25              | 2.5            |
| SCS-8V-32        | 8    | 32     | 0    | rbd\_fast | 1.25              | 2.5            |
| SCS-8V-32-100    | 8    | 32     | 100  | rbd\_fast | 1.25              | 2.5            |
| SCS-16V-64       | 16   | 64     | 0    | rbd\_fast | 2.5               | 5              |
| SCS-16V-64-200   | 16   | 64     | 200  | rbd\_fast | 2.5               | 5              |
| SCS-32V-128      | 32   | 128    | 0    | rbd\_fast | 2.5               | 5              |
| SCS-32V-128-500  | 32   | 128    | 500  | rbd\_fast | 2.5               | 5              |
| SCS-64V-256      | 64   | 256    | 0    | rbd\_fast | 5                 | 10             |
| SCS-64V-256-1000 | 64   | 256    | 1000 | rbd\_fast | 5                 | 10             |

## Compute Optimised

Compute optimised instances are particularly well suited for compute
bound workloads. We recommend to pick these if you your workloads are
processing heavy, but do not require that much memory at the same
time.

Typical workloads:

- High-performance web servers
- Scientific computation
- Batch processing
- Image or video processing
- High-performance computing (HPC)

|                 | vCPU | Memory | Disk | Disk Type | Average Bandwidth | Peak Bandwidth |
| --------------- | ---- | ------ | ---- | --------- | ----------------- | -------------- |
| SCS-8V-16-50    | 8    | 16     | 50   | rbd\_fast | 1.25              | 2.5            |
| SCS-16V-32      | 8    | 16     | 0    | rbd\_fast | 2.5               | 5              |
| SCS-16V-32-100  | 16   | 32     | 100  | rbd\_fast | 2.5               | 5              |
| SCS-32V-64      | 32   | 64     | 0    | rbd\_fast | 2.5               | 5              |
| SCS-32V-64-200  | 32   | 64     | 200  | rbd\_fast | 2.5               | 5              |
| SCS-64V-128     | 64   | 128    | 0    | rbd\_fast | 5                 | 10             |
| SCS-64V-128-500 | 64   | 128    | 500  | rbd\_fast | 5                 | 10             |

## Memory Optimised

Memory optimised instances are particularly suitable for memory bound
workloads that benefit from fast access to large data sets.

Typical workloads:

- Database servers
- In-memory data stores (e.g. redis)

|                  | vCPU | Memory | Disk | Disk Type | Average Bandwidth | Peak Bandwidth |
| ---------------- | ---- | ------ | ---- | --------- | ----------------- | -------------- |
| SCS-1V-8         | 1    | 8      | 0    | rbd\_fast | 0.512             | 1              |
| SCS-1V-8-20      | 1    | 8      | 20   | rbd\_fast | 0.512             | 1              |
| SCS-2V-16        | 2    | 16     | 0    | rbd\_fast | 0.512             | 1              |
| SCS-2V-16-50     | 2    | 16     | 50   | rbd\_fast | 0.512             | 1              |
| SCS-4V-32        | 4    | 32     | 0    | rbd\_fast | 1.25              | 2.5            |
| SCS-4V-32-100    | 4    | 32     | 100  | rbd\_fast | 1.25              | 2.5            |
| SCS-8V-64        | 8    | 64     | 0    | rbd\_fast | 2.5               | 5              |
| SCS-8V-64-200    | 8    | 64     | 200  | rbd\_fast | 2.5               | 5              |
| SCS-16V-128      | 16   | 128    | 0    | rbd\_fast | 2.5               | 5              |
| SCS-16V-128-500  | 16   | 128    | 500  | rbd\_fast | 2.5               | 5              |
| SCS-32V-256      | 32   | 256    | 0    | rbd\_fast | 5                 | 10             |
| SCS-32V-256-1000 | 32   | 256    | 1000 | rbd\_fast | 5                 | 10             |
| SCS-1V-16        | 1    | 16     | 0    | rbd\_fast | 1.25              | 2.5            |
| SCS-1V-16-50     | 1    | 16     | 50   | rbd\_fast | 1.25              | 2.5            |
| SCS-2V-32        | 2    | 32     | 0    | rbd\_fast | 1.25              | 2.5            |
| SCS-2V-32-100    | 2    | 32     | 100  | rbd\_fast | 1.25              | 2.5            |
| SCS-4V-64        | 4    | 64     | 0    | rbd\_fast | 2.5               | 5              |
| SCS-4V-64-200    | 4    | 64     | 200  | rbd\_fast | 2.5               | 5              |

## Disk I/O Optimised

Disk I/O optimised instance are equipped with fast local storage and
are well suited for workloads that require frequent disk access.

Typical workloads:

- Key-value stores (e.g. etcd)
- Kubernetes controlplane nodes

{{% alert color="danger" %}}

Please note that local storage is ephemeral, which means that data
stored on these volumes is lost if the instance is destroyed.

Data will also not be migrated or evacuated when maintenance tasks are
performed.

Please ensure that your workloads can tolerate this behaviour when
utilising instances with local storage or opt for a different instance
type.

{{% /alert %}}

|                   | vCPU | Memory | Disk | Disk Type | Average Bandwidth | Peak Bandwidth |
| ----------------- | ---- | ------ | ---- | --------- | ----------------- | -------------- |
| SCS-2V-8-20s      | 2    | 8      | 20   | ssd       | 1.25              | 2.5            |
| SCS-4V-16-50s     | 4    | 16     | 50   | ssd       | 1.25              | 2.5            |
| SCS-8V-32-100s    | 8    | 32     | 100  | ssd       | 2.5               | 5              |
| SCS-16V-64-200s   | 16   | 64     | 200  | ssd       | 2.5               | 5              |
| SCS-32V-128-500s  | 32   | 128    | 500  | ssd       | 5                 | 10             |
| SCS-64V-256-1000s | 64   | 256    | 1000 | ssd       | 5                 | 10             |
| SCS-1V-8-20s      | 1    | 8      | 20   | ssd       | 512               | 1              |
| SCS-2V-16-50s     | 2    | 16     | 50   | ssd       | 1.25              | 2.5            |
| SCS-8V-64-200s    | 8    | 64     | 200  | ssd       | 2.5               | 5              |
| SCS-4V-32-100s    | 4    | 32     | 100  | ssd       | 2.5               | 5              |
| SCS-16V-128-500s  | 16   | 128    | 500  | ssd       | 2.5               | 5              |
| SCS-32V-256-1000s | 32   | 256    | 1000 | ssd       | 5                 | 10             |

## AI/ML Optimised

AI/ML optimised instances give you access to GPU resources and are
well suited for high-performance computing workloads.

Instance types with NVIDIA A30 Tensor Core GPUs are currently only offered in the muc-a availability zone.

|                         | vCPU | Memory | Disk | Disk Type | Average Bandwidth | Peak Bandwidth | VRam |
|-------------------------|------|--------|------|-----------|-------------------|----------------|------|
| SCS-64V-256-500s_GNa-24h| 64   | 256    | 500  | ssd       | 2.5               | 10             | 24   |

{{% alert color="success" %}}

Please [get in touch with our support](../../support) if you
require differently sized GPU based instances!

{{% /alert %}}

## Budget Optimised

These instances are offered at a lower cost and are a good choice for
non-critical workloads.

Typical workloads:

- Non-critical processes
- Long-running batch processes
- Development or test environments

|          | vCPU | Memory | Disk | Disk Type | Average Bandwidth | Peak Bandwidth |
| -------- | ---- | ------ | ---- | --------- | ----------------- | -------------- |
| SCS-1L-1 | 1    | 1      | 0    | rbd\_fast | 0.512             | 1              |
| SCS-1L0  | 1    | 1      | 5    | rbd\_fast | 0.512             | 1              |
| SCS-1L1  | 2    | 1      | 0    | rbd\_fast | 0.512             | 1              |
| SCS-1L2  | 2    | 1      | 50   | rbd\_fast | 0.512             | 1              |
| SCS-1L3  | 4    | 2      | 0    | rbd\_fast | 0.512             | 1              |
| SCS-1L4  | 4    | 2      | 10   | rbd\_fast | 0.512             | 1              |
| SCS-1L5  | 8    | 4      | 0    | rbd\_fast | 1.25              | 2.5            |
| SCS-1L6  | 8    | 4      | 20   | rbd\_fast | 1.25              | 2.5            |
| SCS-1L7  | 16   | 8      | 0    | rbd\_fast | 1.25              | 2.5            |

## Pricing and Billing

At Wavestack, all instances are charged on an hourly basis, providing flexible and transparent pricing that adjusts to your usage.

### Pricing with CPU-Shares

In order to enable pricing that is as fair as possible, we have designed
“CPU shares”. This means we can also differentiate between the different
 core types in pricing. Each CPU type corresponds to a certain amount of
 shares. The amount of shares mentioned on the invoice corresponds to
the following table:

| CPU-Type | amount of shares |
| -------- | ---------------- |
| **L**    | 1 CPU Shares     |
| **V**    | 10 CPU Shares    |


## Notes

### Units

Information in this document is provided in the following units:

|                     |        |
| ------------------- | ------ |
| **Memory**          | GiB    |
| **Disk**            | GiB    |
| **Bandwidth**       | Gbit/s |
| **Disk Throughput** | GiB/s  |

<!-- References -->

[gh-scs-standards-v3-flavor-naming]: https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0100-v3-flavor-naming.md
