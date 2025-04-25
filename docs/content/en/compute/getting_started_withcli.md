
---
title: "Getting Started with OpenStack Client"
linkTitle: "GettingStarted with Cli"
description: "Instance types on OpenStack"
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->


## Getting Started with OpenStack CLI

## Get the OpenStackClient Software (CLI)

The OpenStackClient is installable via all major Linux Distributions:

for debian and ubuntu with apt:

```bash
sudo apt install python3-openstackclient
```

for ubuntu with snap openstack CLI is installable too:

```bash
sudo snap install openstackclients
```

>[!NOTE]
>Versions from Linux repository could be in a stable but old state.

Install it directly via [pypi](https://pypi.org/project/python-openstackclient)
from upstream is the recommend way.

Here for example RHEL:

```bash
sudo dnf install python3 python3-devel gcc python3-pip
```

Here for example Debian and Ubuntu:

```bash
sudo apt install python3-minimal python3-pip python3-venv python3-dev build-essential
```

 Here as example for SUSE

```bash
sudo zypper in python3-pip python3-venv python3-dev
```

Here for example with Apple's MacOS

```bash
brew install python3
```

>[!NOTE]
>Python installation for windows systems please use the [python installation guide](https://www.python.org/downloads/windows/)
>or recommend use the [Linux Subsystem WSL](https://learn.microsoft.com/de-de/windows/wsl/install)

Python Virtualenv

It is also recommended to use virtual environments, here as an example for
Linux and MacOS:

```bash
python3 -m venv oscli
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

For further Information see the OpenStack Project upstream website
[python-openstackclient](https://docs.openstack.org/python-openstackclient/latest/index.html).


## Configure OpenStack Client


Next to use the OpenStack client we will need application credentials
Login in to Horizon:

- At the UI in the downer left side you will find `<identity>`
- here please create a new application credential
   1. Please choose lease one role, for example *member*
   2. recommended is also a Expiration Date/Time
   3. inadvisable is to the  unrestricted flag.

- with clicking the Button "Create Application Credentials",
  as next your able to download clouds.yaml and <name>-application-cred.openrc.sh

  the clouds.yaml should placed in ~/.config/openstack/
  

```bash
$ source ./<name>-application-cred.openrc.sh
```

```bash
openstack --help
```

when you're using clouds.yaml you can specify multiple endpoints and
select the specific endpoint by passing `--os-cloud=` to the
openstack cmdline or setting `OS_CLOUD`.

```bash
openstack --os-cloud openstack
```

or

```bash
export OS_CLOUD=openstack
openstack
```


## OpenStack Create first resources

1. Network things

```bash
openstack create network mynetwork
openstack subnet create mysubnet --network  mynetwork --subnet-range 192.168.42.0/24
openstack router create  myrouter   --enable-snat  --external-gateway external
openstack port create  --network  mynetwork  --fixed-ip subnet=mysubnet,ip-address=192.168.42.1 myrouter-port
openstack router add port myrouter myrouter-port
openstack security group create mysecurity
openstack security group rule create  --ingress --dst-port 22 --protocol tcp  mysecurity
```

2.  Start the first instance

```bash
openstack keypair create  mykey >  id_rsa.cloud  
chmod 0600 id_rsa.cloud
openstack server create --flavor  SCS-1V-2  --image 'Debian 12' --key-name mykey --network mynetwork  --security-group mysecurity --boot-from-volume 10 myvm
openstack floating ip create external
openstack server add floating ip myvm 31.172.1xx.xxx  

ssh -i id_rsa.cloud  debian@31.172.1xx.xxx
```


## References

- [OpenStack](https://www.openstack.org "OpenStack Site")
- [ansible](https://docs.ansible.com/ansible/latest/collections/openstack/cloud/index.html "Ansible Module OpenStack")
- [terraform](https://registry.terraform.io/providers/terraform-provider-openstack/openstack/latest/docs "OpenStack Terraform Provider")
- [cloud-init](https://cloudinit.readthedocs.io/en/latest/ "cloud-init documentation")

