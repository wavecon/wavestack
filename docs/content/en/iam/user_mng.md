---
title: "User Management"
linkTitle: "User Management"
description: "Manage Users on Wavestack"
type: "docs"
weight: 100
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Overview

This guide walks you through basic user management tasks on Wavestack.

Specifically, you will learn how to:

- Create a user

## User dashboard

Click on the **Users** tab to get to an overview of all user accounts.

![](/assets/iam/users/users-overview.png)


## Create a user

Click the blue **New** button to create a new user.

![](/assets/iam/users/users-new.png)

### User details

#### Mandatory

**E-mail address**

This is the e-mail address of the user. The user will be sent a
registration code to this address, so it is important to provide a
valid address.

**Username**

The username that will be used for the new user.

Note that you could set this to the user's e-mail address again for
sake of simplicity.

**Given Name**

The forename(s) of the user.

**Family Name**

The surname of the user.

#### Optional

**Nickname**

This will be used as display name if used

**Email Verified**

This marks the given e-mail address as already verified. If unset the
user will be sent a verification code.

{{% alert color="warning" %}}

Do **not** mark e-mail as verified

Leave this in the default (i.e. unset) state to ensure that users
have to verify their e-mail addresses.

{{% /alert %}}

**Set Initial Password**

You can set an initial Password for the user. If unset the user will
have to set a password during registration.

**Gender**

The gender of the user.

**Language**

The preferred language of the user

**Phone Number**

Phone number of the user. This number can be used for sending text
messages.

### Finish creation

Click on the blue **Create** button to create the user.

![](/assets/iam/users/users-new-create.png)
