---
title: "Project Management"
linkTitle: "Project Management"
description: "Manage projects on Wavestack"
type: "docs"
weight: 200
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Overview

This guide walks you through managing projects on Wavestack.

Specifically, you will learn how to:

- Create a project
- Manage roles in a project

## Project dashboard

On the dashboard click on the **Projects** tab to see an overview of
all projects in the organisation.

![](/assets/iam/projects/projects-overview.png)

You will see that Wavestack has already created a default project for
you.

![](/assets/iam/projects/projects-default.png)

## Create a project

To add a new projects, click on 'Create New Project'.

![](/assets/iam/projects/projects-new.png)

After entering a suitable name for your new project, click on the blue
**Continue** button to create it.

![](/assets/iam/projects/projects-new-create.png)

## Manage roles

Suitable roles will be automatically created for new projects, but it
might take a couple of minutes for them to be synced to various
systems.

Default roles are listed in the table below.

| Role                    | Service                 | Permissions                                     |
|-------------------------|-------------------------|-------------------------------------------------|
| ga-admin                | [WKE][wvst-wke]         | **create**, **read**, **update** and **delete** |
| ga-viewer               | [WKE][wvst-wke]         | **read**                                        |
| os-creator              | [Barbican][os-barbican] | **create**, **read**, **update** and **delete** |
| os-heat_stack_owner     | [Heat][os-heat]         | **create**, **read**, **update** and **delete** |
| os-load-balancer_member | [Octavia][os-octavia]   | **create**, **read**, **update** and **delete** |
| os-member               | [OpenStack (all)][os]   | **create**, **read**, **update** and **delete** |
| os-reader               | [OpenStack (all)][os]   | **read**                                        |

You can click on **Roles** to review them.

![](/assets/iam/projects/projects-new-roles-overview.png)

<!-- References -->

[os]: https://docs.openstack.org
[os-barbican]: https://docs.openstack.org/barbican/latest
[os-heat]: https://docs.openstack.org/heat/latest
[os-octavia]: https://docs.openstack.org/octavia/latest
[wvst-os]: https://dashboard.wavestack.de
[wvst-wke]: https://dashboard.gardener.wavestack.cloud
