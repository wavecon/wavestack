---
title: "Creating your first Compute Instance via the horizon ui"
linkTitle: "GettingStarted with UI"
weight: 100
description: >
  Description how to get started in the Wavestack by creating a compute instance (virtual machine) with Internet access plus SSH access to the VM.
---

Together We will build a network with a router and connect an instance to it.
 This is only a very simple getting started introduction to get a grasp of how to handle those utilities. We will use the [Wavestack dashboard](https://dashboard.wavestack.de/).

# Introductory

Cloud computing resembles the classical way like with real hardware but it's all virtual now. We start with a blank environment which we will guide you to fill and configure. The fundamental concepts of a classical / physical environment basically remain the same but the wording and handling differ at some points.

| Classical term | OpenStack Cloud term | Description |
| - | - | - |
| Virtual Machine | Compute Instance or short: Instance | A computer system that is virtualized |
| Network | Network | A network. In physical environments there are switches, in OpenStack they are virtual and transparent to the user |
| Router | Router | A device that connects network segments |
| Firewall rule | [Security Group](https://docs.openstack.org/nova/queens/admin/security-groups.html) | Controlling what traffic is allowed to pass in or out of a router |
| Floating IP | Floating IP | An IP address that is public and can be addressed from the Internet. It is kind of a virtual IP (floating) as it is fully managed by the virtual router. Floating IPs are mapped (1:1) to an internal compute instance. |

Working with IP addresses in OpenStack is also somewhat different. There is a default resource called *external* containing a pool of public IP addresses you can use. In order for you to use them properly they will be mapped 1:1 to an internal IP of your instance. That way a host can be fully accessible from the internet, even though the actual compute instance only has an internal IP address. Yet this mapping has to be configured manually by using a floating IP. By default an instance is not reachable from the Internet in any way.

# Overview

Overview of the steps we will take in the given order:

1. Creating a Network + Subnet
1. Creating a Router
1. Creating a Security Group
1. Creating an SSH keypair
1. Creating a Compute Instance
1. Connect it to the Internet
1. Connect via SSH

![](/assets/compute/getting_started_withui/00-overview.png)

# Network overview

First we need to build up a (virtual) network. The compute instance will become part of that network as well as the router.

Below image depicts what we're building during first steps. Visually verify in *Network Topology* view under *Network*. First we will create an internal network (here: *my_nsc_network*). Secondly a router will be created, connected to both nets.

![](/assets/compute/getting_started_withui/00-network-overview-1.png)

# Creating a Network + Subnet (1)

Go to *Network* -> *Networks*

![](/assets/compute/getting_started_withui/01-network-0.png)

Hit *CREATE NETWORK*.

![](/assets/compute/getting_started_withui/01-network-1.png)

Give your network a name. For this guide we will call it *my_nsc_network* which will be our local net.

![](/assets/compute/getting_started_withui/01-network-2.png)

Enter the subnet's name and define a subnet range to work with. Stick to IPv4 only as IPv6 support is yet to come.

![](/assets/compute/getting_started_withui/01-network-3.png)

Leave *Enable DHCP* checked and hit *CREATE*.

![](/assets/compute/getting_started_withui/01-network-4.png)


# Creating a Router (2)

Navigate to *Network* -> *Routers*.

![](/assets/compute/getting_started_withui/02-router-0.png)

*CREATE ROUTER*

![](/assets/compute/getting_started_withui/02-router-1.png)

Choose a name and select the *external* network under *External Network*. Finalize by clicking *CREATE ROUTER*.

![](/assets/compute/getting_started_withui/02-router-2.png)

The virtual router instance will now be listed, click its name.

![](/assets/compute/getting_started_withui/02-router-3.png)

Navigate to the *Interfaces* tab.

![](/assets/compute/getting_started_withui/02-router-4.png)

Click *ADD INTERFACE*.

![](/assets/compute/getting_started_withui/02-router-5.png)

In the dropdown choose our local network.

![](/assets/compute/getting_started_withui/02-router-6.png)

# Create a Security Group (3)

By default inbound traffic is restricted. In order to open up SSH access we will create and apply a security group.

Navigate to *Network* -> *Security Groups*.

![](/assets/compute/getting_started_withui/03-securitygroup-0.png)

*CREATE SECURITY GROUP*

![](/assets/compute/getting_started_withui/03-securitygroup-1.png)

Give it a decent name.

![](/assets/compute/getting_started_withui/03-securitygroup-2.png)

*ADD RULE*

![](/assets/compute/getting_started_withui/03-securitygroup-3.png)

Enter port number *22* and hit *ADD*.

![](/assets/compute/getting_started_withui/03-securitygroup-4.png)

# Creating an SSH keypair (4)

SSH authentication will be password less via key so we will have to setup key based authentication. If you have a private key already adjust the steps accordingly by using the *IMPORT PUBLIC KEY* button instead of *CREATE KEY PAIR* and continue with (5) Create Compute Instance. 

Navigate to *Compute* -> *Key Pairs* and click *CREATE KEY PAIR*.

![](/assets/compute/getting_started_withui/04-keypair-1.png)

Choose a decent name and select *SSH Key* in the dropdown menu. Finalize by clicking *CREATE KEY PAIR*. The data will automatically be download through your browser.

![](/assets/compute/getting_started_withui/04-keypair-2.png)

# Create a Compute Instance (5)

Navigate to *Compute* -> *Instances*.

![](/assets/compute/getting_started_withui/05-instance-0.png)

*LAUNCH INSTANCE*.

![](/assets/compute/getting_started_withui/05-instance-1.png)

Choose a name and hit *NEXT*.

![](/assets/compute/getting_started_withui/05-instance-2.png)

Enter desired storage capacity and choose your OS image. Next go to *Flavor*.

![](/assets/compute/getting_started_withui/05-instance-3.png)

The flavor will determine what resources your virtual cloud machine will have. Arrange according to your quotas and host requirements. Click on *Security Groups*.

![](/assets/compute/getting_started_withui/05-instance-4.png)

Choose the security group for SSH access we created earlier.

![](/assets/compute/getting_started_withui/05-instance-5.png)

# Connect it to the Internet (6)

Your compute instance will be associated with a floating IP. It will be drawn from the prebuilt *external* network and make your instance publicly routable. As long as you don't release this IP it will remain yours.

In the dropdown menu of your compute instance select *ASSOCIATE FLOATING IP*.

![](/assets/compute/getting_started_withui/05-instance-floating-0.png)

Hit the *plus* button.

![](/assets/compute/getting_started_withui/05-instance-floating-1.png)

*ALLOCATE IP*

![](/assets/compute/getting_started_withui/05-instance-floating-2.png)

*ASSOCIATE*

![](/assets/compute/getting_started_withui/05-instance-floating-3.png)

# Connect via SSH (7)

In the *Instances* view you should now see the floating IP as well as the Key pair. 

![](/assets/compute/getting_started_withui/06-finish.png)

When you click your Instance go to *Log*. You can see the host's fingerprint. When you connect to your host via SSH compare and verify for security reasons.

![](/assets/compute/getting_started_withui/07-host_fingerprint.png)

We will now move and rename the SSH key downloaded earlier. Then we use it via the `-i`.


```
user@localhost $ mv ~/Downloads/my_ssh_key.pem ~/.ssh/id_rsa_nsc
user@localhost $ chmod go= ~/.ssh/id_rsa_nsc
user@localhost $ ssh -l debian -i ~/.ssh/id_rsa_nsc 31.172.116.132
The authenticity of host '31.172.116.132 (31.172.116.132)' can't be established.
ED25519 key fingerprint is SHA256:xk1rhORbfkhM5k3E5dwUPqENU9/9Zlq55GVmp2dN5ys.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '31.172.116.132' (ED25519) to the list of known hosts.
Linux my-first-noris-cloud-instance 6.1.0-28-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.119-1 (2024-11-22) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Feb 21 16:35:57 2025 from 62.128.1.62
debian@my-first-noris-cloud-instance:~$
```
CONGRATULATIONS You have now full control over your cloud based compute instance.🎉 
