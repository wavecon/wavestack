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

###### Prerequisites

To getting started with cli on wavestack, please login to the dashboard.
Here will you find below the item identity the sub-item application credentials
please create new application credentials and follow the menu, at least please
download clouds.yaml and rcfile, we will need them for later procedures. 

Next we would like to install the OpenStack client. The OpenStackClient is 
installable for all favorite linux distributions and other systems:

{{% alert color="info" %}}
the openstackclient version should not exceed version 6.2.0 
{{% /alert %}}

{{< tabpane text=true >}}{{% tab header="Debian and Ubuntu" %}}

for debian and ubuntu with apt:

```cli
sudo apt install python3-openstackclient
```

{{% /tab %}}{{% tab header="Ubuntu with snap" %}}

for ubuntu with snap openstack CLI is installable too:

```cli
sudo snap install openstackclients
```

{{% /tab %}}{{% tab header="RHEL with dnf" %}}

Here for example RHEL like Systems:

```cli
sudo dnf install python3 python3-devel gcc python3-pip
```

{{% /tab %}}{{% tab header="apt with Python Env" %}}

Here for example Debian and Ubuntu in python env:

```cli
sudo apt install python3-minimal python3-pip python3-venv python3-dev build-essential
```

{{% /tab %}}{{% tab header="in Python ENV SuSE" %}}

Here as example for SUSE

```cli
sudo zypper in python3-pip python3-venv python3-dev
```

{{% /tab %}}{{% tab header="Homebrew" %}}

Here for example with Apple's MacOS

```cli
brew install python3 python3-openstackclient
```

{{% /tab %}}
{{< /tabpane >}}

{{% alert color="info" %}}
Versions from Linux repository could be in a stable but in a old state.

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

Now you are able to continue   

```cli
❯ source myapplicationrc
❯ openstack token issue 
```

```
openstack image list
openstack flavor list
openstack server create
```
