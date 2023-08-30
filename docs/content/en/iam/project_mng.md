---
title: "Project Management"
linkTitle: "Project Management"
description: "Manage Project on the Wavestack"
type: "docs"
weight: 2
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Overview

This guide walks you through creating, updating, and deleting Projects on the Wavestack.

Specifically, you will learn how to:

- Create a new Projects
- Update existing Projects
- Delete Projects

## Create a new Project

On the Dashboard click on the Projects-Button. This will forward you to the Projects-Overview.

![](/assets/iam/projects/projects-overview.png)

Om the Overview-Page you will find a default Project, which we created for you to get you jump started.

![](/assets/iam/projects/projects-default.png)

Click on 'Create New Project' to create a new Project. This will open an overlay to create a new Project

![](/assets/iam/projects/projects-new.png)

After entering the Name of you new Project, click on the blue Continue-Button. You will be forwarded to the newly created Project

![](/assets/iam/projects/projects-new-create.png)

Here you can check if all the necessary resources for roles are created. In the background there is a service running, which checks every 30 seconds on newly created resources and will synchronize them with [WKE][wvst-wke] and Openstack. Also it will create Roles required to access both services.

In the Project-Overview click on roles to review them.

![](/assets/iam/projects/projects-new-roles.png)

On the Project_role-Page you will see the following roles, as soon as the synchronization is done.

- ga-admin: User can access this Project in [WKE][wvst-wke] and see, create, update and delete Clusters
- ga-viewer: User can access this Project in [WKE][wvst-wke] and see Clusters
- os-creator:
- os-heat_stack_owner:
- os-load-balancer_member
- os-member: User can access this Project in Openstack and see, create, update and delete Resources.
- os-reader:
  
![](/assets/iam/projects/projects-new-roles-overview.png)

<!-- References -->

[wvst-wke]: /kubernetes 