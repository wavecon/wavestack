---
title: "Availability Zone Locations"
linkTitle: "AZ Locations"
description: "Information about the Wavestack service availability zones and their datacenter locations"
type: "docs"
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Regions and Locations

### Overview

This document provides an introduction to assigning availability zones (AZs) to datacenter locations. Availability
Zones are isolated locations that are used to separate resources from each other to provide greater resilience and
operational redundancy.

In our configuration, we use three Availability Zones, each located in a different datacenter of the noris network GmbH
datacenter campus Munich East. This configuration provides mutual operational redundancy so that if one zone fails, the
other two can take over.

### Why Availability Zones?

Availability zones provide several benefits:

* **Fault Tolerance**: Because resources are physically and logically separated, failures in one zone cannot affect the
  other zones.
* **Load Balancing**: Resources can be evenly distributed across multiple zones to achieve optimal utilization.
* **Maintainability**: Maintenance can be performed in one zone without affecting the others.

#### Important Note on High Availability

For setups that require high availability, it is essential to use at least two availability zones. This ensures that if
one zone experiences problems, the other can take over and maintain service availability. Implementing this multi-zone
strategy is the responsibility of the customer. If you have any questions or need guidance on how to achieve this, we
are happy to assist.

### Overview of Availability Zones and their Datacenter Locations

Currently, you can choose between three Availability Zones (AZs), each assigned to different datacenters within the
MUC-5 campus location. The specific details are as follows:

| AZ         | Region | Location              |
|------------|--------|-----------------------|
| **muc5-a** | MUC-5  | MUC5ITCA (Building 1) |
| **muc5-b** | MUC-5  | MUC5ITCB (Building 1) |
| **muc5-d** | MUC-5  | MUC5ITCD (Building 2) |

Each of these availability zones provides an isolated environment for resources and is configured to provide mutual
operational redundancy. This means that in the event of a failure or maintenance in one of the zones, the other zones
can take over the load and maintain operations.

## About Datacenter Campus Munich East (MUC-5)

The Munich East datacenter campus consists of a total of four datacenters with the highest availability according to
BSI criteria, housed in two structurally separate buildings. Currently, three of these four datacenters are used to
provide Wavestack. By using the maximum-availability datacenter campus, we achieve operational redundancy for the
Wavestack that would otherwise only be possible by using physically separate datacenters.
In addition, each of the datacenters has the following certifications, tests and awards

* ISO/IEC 27001: Information Security Management System
* ISO 27001: IT-Grundschutz certification
* ISO/IEC 20000-1: Service Management System
* ISO 9001: Quality Management System
* EN 50600: Design and operation of secure data centres
* VdS 3406: Property Security Management System
* PCI DSS: Payment Card Industry Data Security Standard
* ISAE 3402 Type II: Internal Control System based on COBIT 5
* German Datacenter Award 2016

For more information on the Datacenter Campus, visit https://www.noris.de/rechenzentren/rz-muenchen-ost/
