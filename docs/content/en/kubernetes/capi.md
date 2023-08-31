---
title: "Cluster API"
linkTitle: "Cluster API"
description: "Manage Kubernetes clusters using Cluster API"
type: "docs"
weight: 1

cascade:
- _target:
    path: "/**"
    kind: "page"
  type: "docs"
- _target:
    path: "/**"
    kind: "section"
  type: "docs"
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

## Overview

This guide details how to use [Cluster API Provider OpenStack
(CAPO)][gh-k8s-sigs-capo] for managing Kubernetes clusters on
Wavestack.

Specifically, you will learn how to:

- Create and upload node images
- Create a new Kubernetes cluster
- Access a cluster using kubectl
- Deploy a workload
- Delete a cluster

## Prerequisites

In order to follow this guide, the following tools have to be installed:

- [kubectl][k8s-tasks-install-tools]
- [clusterctl][capi-install-clusterctl]
- [helm][helm-docs-install]
- [cilium-cli][cilium-docs-install-cli]

### Repositories

Please also clone the following repository to a suitable location:

- https://github.com/wavecon/wavestack

This can, for example, be achieved by running the following commands:

```cli
❯ mkdir ~/src/github.com/wavecon
❯ git clone git@github.com:wavecon/wavestack.git
```

### Compatibility

The steps in this guide have been tested with the following versions:

|                                    | Version  |
|------------------------------------|----------|
| cilium-cli                         | v0.15.6  |
| cluster-api                        | v1.5.1   |
| cluster-api-provider-openstack     | v0.7.3   |
| clusterctl                         | v1.5.1   |
| flatcar                            | 3510.2.6 |
| helm                               | v3.12.2  |
| kubectl                            | v1.28.1  |
| kubernetes                         | v1.28.1  |
| openstack-cloud-controller-manager | v1.28.0  |

## Access the Wavestack dashboard

You will probably spend most of your time on the command line, but you
can also log into the [Wavestack dashboard][wvst-dashboard] with your
Wavestack account if you want to get an overview of the provisioned
resources.


### Management Cluster

Create a kind cluster:

```cli
❯ kind create cluster
```

## Install CAPI/CAPO control plane

Enable any [CAPI experimental features][capi-exp-features] you need,
specifically [Ignition][capi-exp-features-ignition] support for
[Flatcar Linux][flatcar]:

```cli
❯ export BOOTSTRAP_FORMAT_IGNITION=true
❯ export EXP_KUBEADM_BOOTSTRAP_FORMAT_IGNITION=true
```

and initialise CAPI/CAPO:

```cli
❯ clusterctl init --infrastructure openstack
```

## Cluster configuration

### Node Image

You have to provide a Cluster API compatible image for your control
plane and worker nodes. Suitable build manifests can be found in the
[image-builder][gh-k8s-sigs-image-builder] project. Documentation for
OpenStack can be found on:

- https://image-builder.sigs.k8s.io/capi/providers/openstack.html

#### Install build dependencies

Images have to be built on Linux. Start by installing and configuring
dependencies:

```cli
❯ sudo apt install --no-install-recommends qemu-system qemu-utils libvirt-daemon-system
❯ sudo adduser $(whoami) libvirt
❯ sudo adduser $(whoami) kvm
❯ sudo apt install git build-essential jq git python3 python3-pip unzip
```

Clone the [image-builder][gh-k8s-sigs-image-builder] repository to a
suitable location and install the build requirements for QEMU:

```cli
❯ cd ~/src/github.com/kubernetes-sigs/image-builder/images/capi
❯ make deps-qemu
❯ export $PATH=${PATH}:${HOME}/src/github.com/kubernetes-sigs/image-builder/capi/.local/bin
```

#### Flatcar

We are going to use a [flatcar][flatcar] based image in this guide,
but you can use another one if you prefer.

Flatcar is a Linux distribution designed for container workloads and
high security.


#### Configure Kubernetes version

You would currently build a separate image for each Kubernetes version
that you want to use. In order to configure this, you would create a
configuration file (e.g `flatcar-stable-kube-v1.28.1.json`) with
content similar to:

```json
{
  "kubernetes_semver": "v1.28.1",
  "kubernetes_series": "v1.28"
}
```

In that example `kubernetes_semver` refers to the specific [Kubernetes
release][k8s-releases] you want, while you specify the *series* (i.e.
major and minor version) in `kubernetes_series`.

With that in place, you can build the image with:

```cli
❯ PACKER_VAR_FILES=/path/to/flatcar-stable-kube-v1.28.1.json OEM_ID=openstack make build-qemu-flatcar
```

To learn more about configuring the image build, read [Image Builder -
CAPI Image Configuration][k8s-sigs-image-builder-capi-images].

#### Upload image

Once the image has been built, upload it to OpenStack using the
following command:

```cli
❯ openstack image create --progress --min-ram 1024 --min-disk 5
    --property image_build_date=(date +%F)
    --property image_original_user=core
    --property architecture=x86_64
    --property hypervisor_type=qemu
    --property os_distro=flatcar
    --property os_version="3510.2.6"
    --property hw_disk_bus=virtio
    --property hw_scsi_model=virtio-scsi
    --property hw_rng_model=virtio
    --file ./output/flatcar-stable-3510.2.6-kube-v1.28.1/flatcar-stable-3510.2.6-kube-v1.28.1
    flatcar-stable-3510.2.6-kube-v1.28.1
```

and configure the image name:

```cli
❯ export OPENSTACK_FLATCAR_IMAGE_NAME="flatcar-stable-3510.2.6-kube-v1.28.1"
```

### SSH

Add your SSH key to OpenStack:

```cli
❯ openstack keypair create --public-key ~/.ssh/id_ed25519.pub <ssh-key-name>
```

and configure Cluster API to use it:

```cli
❯ export OPENSTACK_SSH_KEY_NAME=<ssh-key-name>
```

### OpenStack credentials

In order to manage resources on OpenStack, you have to provide valid
credentials to the Cluster API controllers.

Create an [application credential][openstack-application-credentials]
for your project, which can be done by running:

```cli
❯ openstack application credential create <app-cred-name>
```

You can also create the application credential manually on the
[Wavestack dashboard][wvst-dashboard-application-credentials] if you
prefer that.

Create a [clouds.yaml][openstack-configuration] file similar to:

```yaml
clouds:
  wavestack:
    auth:
      auth_url: "<openstack-api-endpoint>"
      application_credential_id: "<app-cred-id>"
      application_credential_secret: "<app-cred-password>"
    region_name: <region>
    interface: "public"
    identity_api_version: 3
    auth_type: "v3applicationcredential"
    cacert: "<home directory path>/.config/openstack/DigiCertGlobalRootCA.crt.pem"
```

Please note that you have to provide a suitable CA certificate as
`auth_url` is behind https. Adjust the path in the template above and
download the Digicert CA certificate by running:

```cli
❯ curl https://cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem -o ~/.config/openstack/DigiCertGlobalRootCA.crt.pem
```

You can then export credentials with the following:

```cli
❯ curl https://raw.githubusercontent.com/kubernetes-sigs/cluster-api-provider-openstack/master/templates/env.rc -o env.rc
❯ source env.rc ~/.config/openstack/clouds.yaml wavestack
```

### Nameservers

Configure a nameserver of your choice:

```cli
❯ export OPENSTACK_DNS_NAMESERVERS="9.9.9.9"
```

### Node instance types

Wavestack offers a [wide variety of instance
types](../../reference/instance_types/) to suit different workloads.
You can list them all by running the following command:

```cli
❯ openstack flavor list
```

Our recommendation for control-plane nodes would be types with fast
local storage. You can easily find these by looking for names that end
in `s`, such as `SCS-4V-16-50s`.

Once you have made your choice, you can configure the instance types
by setting the following two variables for control-plane and worker
nodes respectively:

```
❯ export OPENSTACK_CONTROL_PLANE_MACHINE_FLAVOR="SCS-4V-16-50s"
❯ export OPENSTACK_NODE_MACHINE_FLAVOR="SCS-2V-4"
```

### External network

An [external network][docs-os-neutron-concepts] is required for
communication with the outside world, for example when using load
balancers.

You can list external networks with the following command:

```cli
❯ openstack network list --external
```

and use that information to define the relevant environment variable:

```cli
❯ export OPENSTACK_EXTERNAL_NETWORK_ID=<external network id>
```

## Generate cluster manifests

Cluster API extends the Kubernetes API through custom resource
definitions that allow you to define clusters using a wide variety of
resources.

Whilst you will certainly want to define your own set of manifests for
production clusters, we will generate suitable manifests from a
template using the following command:

```cli
❯ clusterctl generate cluster k8s-wvst-capo-quickstart \
    --kubernetes-version v1.28.1 \
    --control-plane-machine-count=3 \
    --worker-machine-count=1 \
    --from ~/src/github.com/wavecon/wavestack/cluster-api/templates/cluster-template-flatcar.yaml
    > k8s-wvst-capo-quickstart.yaml
```

If you are curious what kind of resources are used for managing
clusters, take a look at the generated manifests.

You can find an overview of all currently supported custom resources on:

- https://doc.crds.dev/github.com/kubernetes-sigs/cluster-api
- https://doc.crds.dev/github.com/kubernetes-sigs/cluster-api-provider-openstack

## Create a cluster

Create the cluster simply by applying the newly generated manifests to
your management cluster:

```cli
❯ kubectl apply -f k8s-wvst-capo-quickstart.yaml
```

Inspect the cluster status:

```cli
❯ clusterctl describe cluster k8s-wvst-capo-quickstart
NAME                                                                         READY  SEVERITY  REASON                       SINCE  MESSAGE
Cluster/k8s-wvst-capo-quickstart                                             False  Warning   ScalingUp                    7m40s  Scaling up control plane to 3 replicas (actual 1)
├─ClusterInfrastructure - OpenStackCluster/k8s-wvst-capo-quickstart
├─ControlPlane - KubeadmControlPlane/k8s-wvst-capo-quickstart-control-plane  False  Warning   ScalingUp                    7m40s  Scaling up control plane to 3 replicas (actual 1)
│ └─Machine/k8s-wvst-capo-quickstart-control-plane-xsdcr                     True                                          6m24s
└─Workers
  └─MachineDeployment/k8s-wvst-capo-quickstart-md-0                          False  Warning   WaitingForAvailableMachines  9m40s  Minimum availability requires 1 replicas, current 0 available
    └─Machine/k8s-wvst-capo-quickstart-md-0-rz8dq-5w8t5
```

and wait for the control-plane to initialise:

```cli
❯ k get kubeadmcontrolplane --watch
NAME                                     CLUSTER                    INITIALIZED   API SERVER AVAILABLE   REPLICAS   READY   UPDATED   UNAVAILABLE   AGE   VERSION
k8s-wvst-capo-quickstart-control-plane   k8s-wvst-capo-quickstart                                                                                   39s   v1.28.1
k8s-wvst-capo-quickstart-control-plane   k8s-wvst-capo-quickstart                                                                                   109s   v1.28.1
k8s-wvst-capo-quickstart-control-plane   k8s-wvst-capo-quickstart                                                                                   2m     v1.28.1
k8s-wvst-capo-quickstart-control-plane   k8s-wvst-capo-quickstart                                        1                  1         1             2m     v1.28.1
k8s-wvst-capo-quickstart-control-plane   k8s-wvst-capo-quickstart                                        1                  1         1             3m27s   v1.28.1
k8s-wvst-capo-quickstart-control-plane   k8s-wvst-capo-quickstart                                        1                  1         1             4m16s   v1.28.1
k8s-wvst-capo-quickstart-control-plane   k8s-wvst-capo-quickstart   true                                 1                  1         1             4m16s   v1.28.1
```

{{% alert title="Quotas" %}}

Note that the creation of cluster resources is subject to quotas on
your Wavestack project. You should make sure that you choose suitable
worker node instance types to ensure that you stay within your quotas.

Get in touch with [support](../../support) if you require a quota
increase.

{{% /alert %}}

## Access a cluster

Once your new cluster has finished bootstrapping, you can configure
access to it via kubectl.

Grab a suitable kubeconfig file from the management cluster by
running:

```cli
❯ clusterctl get kubeconfig k8s-wvst-capo-quickstart > k8s-wvst-capo-quickstart.kubeconfig
```

and open a new shell to inspect your freshly minted cluster:

```cli
❯ export KUBECONFIG=$(pwd)/k8s-wvst-capo-quickstart.kubeconfig
❯ kubectl get nodes
NAME                                           STATUS     ROLES           AGE   VERSION
k8s-wvst-capo-quickstart-control-plane-krlcd   NotReady   control-plane    5m   v1.28.1
k8s-wvst-capo-quickstart-md-0-fnjkc            NotReady   <none>           4m   v1.28.1
```

The nodes are `NotReady` as the cluster has no CNI or
cloud-controller-manager yet, which you will address next.

## Cluster addon installation

### Cilium CNI

Cilium can be trivially installed on your new cluster by running:

```cli
❯ cilium install
ℹ️  Using Cilium version 1.14.0
🔮 Auto-detected cluster name: k8s-wvst-capo-quickstart
🔮 Auto-detected datapath mode: tunnel
🔮 Auto-detected kube-proxy has been installed
```

### OpenStack CCM

The OpenStack CCM can be installed via the helm chart provided by
upstream:

- https://github.com/kubernetes/cloud-provider-openstack/tree/master/charts/openstack-cloud-controller-manager

Create a `openstack-ccm-values.yaml` file similar to:

```yaml
# wavestack
#
# openstack-ccm /w capo configuration for wavestack

cloudConfig:
  global:
    auth-url: https://api.wavestack.de:5000
    application-credential-id: "s3cr1t"
    application-credential-secret: "s0v3rys3cr1t"

cluster:
  name: k8s-capo-wvst-quickstart

nodeSelector:
  node-role.kubernetes.io/control-plane: ""

tolerations:
  - key: node.cloudprovider.kubernetes.io/uninitialized
    value: "true"
    effect: NoSchedule
  - key: node-role.kubernetes.io/control-plane
    effect: NoSchedule
```

Use helm to install the OpenStack cloud controller manager:

```cli
❯ helm repo add cpo https://kubernetes.github.io/cloud-provider-openstack
"cpo" has been added to your repositories

❯ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "cpo" chart repository

❯ helm upgrade -n kube-system -i openstack-ccm cpo/openstack-cloud-controller-manager --version 2.29.0-alpha.1 --values ~/src/localhost/capo/live/occm/helm/openstack-ccm.yaml
Release "openstack-ccm" does not exist. Installing it now.
NAME: openstack-ccm
LAST DEPLOYED: Wed Aug 30 15:24:49 2023
NAMESPACE: kube-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

Once these two cluster addons are in place, you can wait for all
three control-plane nodes to become ready:

```cli
❯ k get nodes --watch
NAME                                           STATUS   ROLES           AGE   VERSION
k8s-wvst-capo-quickstart-control-plane-krlcd   Ready    control-plane     5m   v1.28.1
k8s-wvst-capo-quickstart-md-0-fnjkc            Ready    <none>            4m   v1.28.1
k8s-wvst-capo-quickstart-control-plane-wdzlx   NotReady   <none>          0s    v1.28.1
k8s-wvst-capo-quickstart-control-plane-wdzlx   NotReady   control-plane   17s   v1.28.1
k8s-wvst-capo-quickstart-control-plane-wdzlx   Ready      control-plane   32s   v1.28.1
k8s-wvst-capo-quickstart-control-plane-2g2pj   NotReady   <none>          0s    v1.28.1
k8s-wvst-capo-quickstart-control-plane-2g2pj   NotReady   control-plane   10s   v1.28.1
k8s-wvst-capo-quickstart-control-plane-2g2pj   Ready      control-plane   17s   v1.28.1
```

## Deploy a Workload

### Create a Deployment

Use the `kubectl create` command to create a simple Deployment that
manages a Pod running the [agnhost][gh-k8s-k8s-test-images-agnhost] image.

```cli
❯ kubectl create deployment hello-wvst --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost netexec --http-port=8080
deployment.apps/hello-wvst created
```

View the deployment:

```cli
❯ kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-wvst   1/1     1            1           1m
```

### Create a Service

Expose the Pod to the public internet using the kubectl expose
command:

```cli
❯ kubectl expose deployment hello-wvst --type=LoadBalancer --port=8080
service/hello-wvst exposed
```

The `--type=LoadBalancer` flag indicates that you want to expose your
Service outside of the cluster and triggers the creation of a load
balancer.

View the newly created Service:

```cli
❯ kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-wvst   LoadBalancer   100.98.31.115   <pending>     8080:31058/TCP   5s
kubernetes   ClusterIP      100.96.0.1      <none>        443/TCP          10m
```

You can see that the external IP of the service is still `<pending>`
while the load balancer is being provisioned.

Once the provisioning has finished, the service status will be
updated:

```cli
❯ kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
hello-wvst   LoadBalancer   100.98.31.115   31.172.116.232   8080:31058/TCP   2m4s
kubernetes   ClusterIP      100.96.0.1      <none>           443/TCP          12m
```

You can test the service by running the following command:

```cli
❯ curl http://31.172.116.232:8080
NOW: 2023-08-30 12:26:05.631905038 +0000 UTC m=+2045.885709493
```

Delete the service to clean up the load balancer and floating IP:

```cli
❯ kubectl delete service hello-wvst
service "hello-wvst" deleted
```

## Delete a cluster

Finish by deleting the tenant cluster resource on the management
cluster:

```cli
❯ kubectl delete cluster k8s-wvst-capo-quickstart
cluster.cluster.x-k8s.io "k8s-wvst-capo-quickstart" deleted
```

{{% alert color="warning" %}}

Please make sure to delete the cluster explicitly, rather than
removing all cluster manifests with `kubectl delete -f` as that would
also remove the controller credentials required for removing resources
on Wavestack.

{{% /alert %}}

<!-- References -->

[capi-install-clusterctl]: https://cluster-api.sigs.k8s.io/user/quick-start.html#install-clusterctl
[capi-exp-features]: https://cluster-api.sigs.k8s.io/tasks/experimental-features/experimental-features.html
[capi-exp-features-ignition]: https://cluster-api.sigs.k8s.io/tasks/experimental-features/ignition
[cilium-docs-install-cli]: https://docs.cilium.io/en/stable/gettingstarted/k8s-install-default/#install-the-cilium-cli
[docs-os-neutron-concepts]: https://docs.openstack.org/neutron/2023.1/install/concepts.html
[flatcar]: https://www.flatcar.org/
[gh-k8s-sigs-capo]: https://github.com/kubernetes-sigs/cluster-api-provider-openstack/
[gh-k8s-sigs-image-builder]: https://github.com/kubernetes-sigs/image-builder
[helm-docs-install]: https://helm.sh/docs/intro/install/
[k8s-releases]: https://kubernetes.io/releases/
[k8s-sigs-image-builder-capi-images]: https://github.com/kubernetes-sigs/image-builder/blob/master/docs/book/src/capi/capi.md#capi-images
[k8s-tasks-install-tools]: https://kubernetes.io/docs/tasks/tools/
[kind]: https://kind.sigs.k8s.io/
[openstack-application-credentials]: https://docs.openstack.org/keystone/latest/user/application_credentials.html
[openstack-configuration]: https://docs.openstack.org/python-openstackclient/latest/configuration/index.html
[wvst-dashboard]: https://dashboard.wavestack.de/
[wvst-dashboard-application-credentials]: https://dashboard.wavestack.de/identity/application_credentials/
