---
title: "Getting started"
linkTitle: "Getting Started"
description: "Getting started on Wavestack"
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Handle wavestack with openstackclient

To getting started with cli on wavestack, please login to the dashboard.
Here will you find below the item identity the sub-item application credentials
please create new application credentials and follow the menu, at least please
download clouds.yaml and rcfile, we will need them for later procedures. 

Next we would like to install the OpenStack client. The OpenStackClient is 
installable for all favorite linux distributions and other systems:

{{% alert color="info" %}}
the openstackclient version should not exceed version 6.2.0 
{{% /alert %}}

for debian and ubuntu with apt:

```cli
sudo apt install python3-openstackclient
```

for ubuntu with snap openstack CLI is installable too:

```cli
sudo snap install openstackclients
```

Here for example RHEL like Systems:

```cli
sudo dnf install python3 python3-devel gcc python3-pip
```

Here for example Debian and Ubuntu:

```cli
sudo apt install python3-minimal python3-pip python3-venv python3-dev build-essential
```

Here as example for SUSE

```cli
sudo zypper in python3-pip python3-venv python3-dev
```

Here for example with Apple's MacOS

```cli
brew install python3 python3-openstackclient
```

{{% alert color="info" %}}
Versions from Linux repository could be in a stable but old state.

Install it directly via [pypi](https://pypi.org/project/python-openstackclient)
from upstream is the recommend way. It is also recommended to use virtual 
environment, here as an example for Linux and MacOS:

```cli
python3 -m venv wavestack
source oscli/bin/activate
pip install --upgrade pip
pip install python-openstackclient \
python-cinderclient \
python-designateclient \
python-glanceclient  \
python-neutronclient \
python-novaclient \
python-octaviaclient \
python-barbicanclient
```

{{% /alert %}}

Now we are able to continue  

```cli
❯ source myapplicationrc
❯ openstack token issue 
```
