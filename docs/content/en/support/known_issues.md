---
title: "Known Issues"
linkTitle: "Known Issues"
description: "Information on known issues in Wavestack services."
type: "docs"
weight: 2
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## RGW Objectstore

### Unable to create buckets

#### Details

An upgrade on our RGW clusters introduced a regression that made it
impossible to create buckets using the web frontend.

The underlying issue is a bug in Ceph RGW, in that a necessary string
for "storage policy" is empty. We reported it at:

- https://tracker.ceph.com/issues/62771

#### Workaround

Bucket creation is still possible using the command line interface,
which is not affected.
