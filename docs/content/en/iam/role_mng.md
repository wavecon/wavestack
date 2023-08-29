---
title: "Role Management"
linkTitle: "Role Management"
description: "Manage Roles on the Wavestack"
type: "docs"
weight: 3
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Overview

This guide walks you through assigning and revoking Roles to Users in Projects on the Wavestack.

Specifically, you will learn how to:

- Assign roles to a User
- Revoke a roles from a User

## Assign a Role to a User in a Project

On the Dashboard-Overview click on Authorizations to get forwarded to the Role section.

![](/assets/iam/roles/roles-overview.png)

On the Authorizations-Page you will find a default authorization which we created for you. This authorization will let you access the default Project with your first user.

![](/assets/iam/roles/roles-default.png)

To authorize Users to Projects click on the blue New-Button. This will open up an overlay, where you can allow selected Users to have access to Projects.

![](/assets/iam/roles/roles-new.png)

On the Overlay select the Users you want to grant permission from the Drop-Down-Selection.

![](/assets/iam/roles/roles-new-select-users.png)

Select the Project, to which the previously chosen Users should get permissions.

![](/assets/iam/roles/roles-new-select-project.png)

Click on the blue Continue-Button to get to the next Page, where you can select the Roles granted to the Users.

![](/assets/iam/roles/roles-new-create.png)

Select the Roles you wish to grant the Users and confirm it by clicking the blue Save-Button.

![](/assets/iam/roles/roles-new-select-roles.png)

Finally you will be forwarded back to the Authorization-Overview. You should see that the selected Users have now the chosen Roles in that Project.

![](/assets/iam/roles/roles-new-overview.png)