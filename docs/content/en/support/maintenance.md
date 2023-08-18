---
title: "Maintenance and Upgrades"
linkTitle: "Maintenance"
description: "Information on upgrades and maintenance procedures for Wavestack services."
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

We perform regular upgrades and other maintenance tasks on Wavestack
services to keep them fully up-to-date or roll out new features.

These tasks are not generally expected to impact service availability.

## Maintenance windows

Our core maintenance windows are summarised in the table below:

| Day       | Time               |
|-----------|--------------------|
| Monday    | 09:00 - 14:00 CEST |
| Tuesday   | 09:00 - 14:00 CEST |
| Wednesday | 09:00 - 14:00 CEST |

Maintenance tasks are performed progressively to mitigate any
unforeseen negative impact. If applicable, we will roll out changes
separately in individual zones, which ensures the availability of at
least two zones at any given point in time.

{{% alert color="warning" %}}
Please note that urgent security-upgrades may be performed outwith
these maintenance windows.
{{% /alert %}}

## Services

### OpenStack API

Certain maintenance actions might result in the limited availability
of the OpenStack API. This does, however, not typically affect any
running workloads.

We will announce any maintenance task that is expected to impact the
OpenStack API for more than 30 minutes at least 5 working days in
advance.

### Compute instances

If required, we will live-migrate compute instances to other
hypervisors to ensure their continuous operation.

{{% alert color="warning" %}}

Please note that data on local volumes will **not** be migrated.

We also recommend to ensure that your workloads can tolerate this
behaviour when utilising instances with local storage or to opt for a
different instance type.

All instance types with a trailing `s` in their name are affected by
this (e.g. `SCS-2V-4-20s`).

{{% /alert %}}
