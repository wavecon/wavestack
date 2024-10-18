---
title: "Role Management"
linkTitle: "Role Management"
description: "Manage user roles on Wavestack"
type: "docs"
weight: 300
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Overview

This guide walks you through assigning and revoking roles for users in
projects on Wavestack.

Specifically, you will learn how to:

- Assign roles to a user
- Revoke roles from a user

## Authorisation dashboard

On the dashboard click on the **Authorizations** tag to see an
overview of all authorisations that are currently in place.

![](/assets/iam/roles/roles-overview.png)

### Default authorisation

If you open this for the first time, you will notice that Wavestack
has already created a single default authorisation for you. This
authorisation will let you access the default project with your
initial user.

![](/assets/iam/roles/roles-default.png)

## Add an authorisation

To add a new authorisation, click on the blue **New** button. This
will open a wizard that allows you to grant roles to users in specific
projects.

![](/assets/iam/roles/roles-new.png)

### User selection

Select the users that you want to authorise from the list of users.

![](/assets/iam/roles/roles-new-select-users.png)

### Project selection

Select the project in which you want to grant roles.

![](/assets/iam/roles/roles-new-select-project.png)

Click on the blue **Continue** button to get to the next page.

![](/assets/iam/roles/roles-new-create.png)

### Role selection

You can grant different roles to users in your projects, which allows
for a more fine-grained control over their permissions.

You can select from the following roles:

| Role                    | Service                 | Permissions                                     |
|-------------------------|-------------------------|-------------------------------------------------|
| ga-admin                | [WKE][wvst-wke]         | **create**, **read**, **update** and **delete** |
| ga-viewer               | [WKE][wvst-wke]         | **read**                                        |
| os-creator              | [Barbican][os-barbican] | **create**, **read**, **update** and **delete** |
| os-heat_stack_owner     | [Heat][os-heat]         | **create**, **read**, **update** and **delete** |
| os-load-balancer_member | [Octavia][os-octavia]   | **create**, **read**, **update** and **delete** |
| os-member               | [OpenStack (all)][os]   | **create**, **read**, **update** and **delete** |
| os-reader               | [OpenStack (all)][os]   | **read**                                        |

### Finish creation

Once you are happy with your selection, click the blue **Save** button.

![](/assets/iam/roles/roles-new-select-roles.png)

You will then return to the overview of all authorisations, where you
will see your newly created authorisation.

![](/assets/iam/roles/roles-new-overview.png)

<!-- References -->

[os]: https://docs.openstack.org
[os-barbican]: https://docs.openstack.org/barbican/latest
[os-heat]: https://docs.openstack.org/heat/latest
[os-octavia]: https://docs.openstack.org/octavia/latest
[wvst-os]: https://dashboard.wavestack.de
[wvst-wke]: https://dashboard.gardener.wavestack.cloud
