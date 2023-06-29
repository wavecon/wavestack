---
title: "Wavestack Kubernetes Engine"
linkTitle: "Wavestack Kubernetes Engine"
description: "Manage Kubernetes clusters using WKE"
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

Wavestack Kubernetes Engine (WKE) implements the automated management
and operation of [Kubernetes][k8s] clusters as a service.

WKE is build using [Gardener][gardener] and you can find additional
information in their [documentation][gardener-docs].

## Overview

This guide walks you through creating, accessing, and using a
Kubernetes cluster using WKE.

Specifically, you will learn how to:

- Create a new Kubernetes cluster
- Access a cluster using kubectl
- Deploy a workload
- Delete a cluster

## Prerequisites

In order to follow this guide, the following tools have to be installed:

- [kubectl][k8s-tasks-install-tools]
- [gardenlogin][gh-garden-gardenlogin-install]
- [kubelogin][gh-int128-kubelogin-setup]

### Compatibility

The steps in this guide have been tested with the following versions:

|             | Version |
|-------------|---------|
| kubectl     | v1.27.3 |
| gardenlogin | v0.4.0  |
| kubelogin   | v1.27.0 |

## Access the dashboard

You can log into the [Gardener dashboard][wvst-gardener-dashboard]
with your Wavestack account.

![](/assets/kubernetes/wke/gardener-dashboard.png)

Additional documentation can be found in the [Gardener Dashboard
Documentation][gardener-docs-dashboard].

## Create a cluster

Click the **+** button at the top to start the creation of a new
Kubernetes cluster or [shoot][gardener-api-shoot] in Gardener
parlance.

![](/assets/kubernetes/wke/gardener-db-create-cluster.png)

### Configuration

The cluster configuration wizard allows you to tailor the cluster
configuration to your needs.

{{% alert title="Gardener Resources" %}}

Please note that you can edit the generated Gardener custom resources
directly by clicking on the **YAML** tab. This allows you to specify
settings that are not available in the configuration wizard.

A specification of these resources can be found in the [Gardener API
Reference](https://gardener.cloud/docs/gardener/api-reference/).

{{% /alert %}}

#### Infrastructure

![](/assets/kubernetes/wke/gardener-create-cluster-infra.png)

Gardener supports multiple providers, specifically:

- **aws** - Amazon Web Services
- **azure** - Microsoft Azure
- **openstack** - Wavestack

#### Cluster Details

![](/assets/kubernetes/wke/gardener-create-cluster-details.png)

The cluster details section allows you to customise various cluster
specific settings.

**Cluster name**

Gardener will generate a random default name for your new cluster or
you can define one yourself.

**Kubernetes version**

New clusters can be created using a number of different Kubernetes
versions. It is recommended to always use the newest release unless
you have reasons not to.

An overview of current Kubernetes releases can be found on:

- https://kubernetes.io/releases/

**Cluster purpose**

This setting denotes the intended purpose or level of production
readiness of the cluster.

Please refer to [shoot purposes][gardener-docs-shoot-purposes] for
further information on differences in the way the shoot clusters are
set up based on the selected purpose.

#### Infrastructure Details

![](/assets/kubernetes/wke/gardener-create-cluster-infra-details.png)

**Secret**

This secret will be used by Gardener and the [Kubernetes Cloud
Controller Manager][k8s-concepts-arch-ccm] when interacting with
the underlying cloud provider.

**Region**

Choose a suitable region for your new cluster.

**Networking Type**

Kubernetes [cluster networking][k8s-concepts-admin-networking] can be
easily customised by selecting a suitable [Container Network
Interface][gh-containernetworking-cni] (CNI) for your use case.

Wavestack currently supports the following:

- [Calico][gh-calico]
- [Cilium][gh-cilium]

#### Worker Groups

![](/assets/kubernetes/wke/gardener-create-cluster-worker-groups.png)

**Machine Type**

Select the flavor that will be used for your worker node. Wavestack
follows [Sovereign Cloud Stack][scs] standards for instance naming:

- [SCS Flavor Naming Standard][scs-flavor-naming-v3]

**Autoscaling**

Every cluster that has at least one worker group with `minimum <
maximum` nodes will get a suitably configured
[autoscaler][gardener-cluster-autoscaler] deployment, which allows
Gardener to dynamically scale the number of worker nodes in line with
demand.

The Gardener autoscaler is a fork of the Kubernetes
[cluster-autoscaler][k8s-cluster-autoscaler], with additional support
for
[gardener/machine-controller-manager][gh-gardener-machine-controller-manager].

#### Maintenance

![](/assets/kubernetes/wke/gardener-create-cluster-maintenance.png)


Gardener configures a time window for automated cluster update tasks.
You can configure Gardener to perform the following updates
automatically:

- Kubernetes patch releases (control plane and worker nodes)
- Worker node machine images

If you want to learn more about this refer to [shoot
maintenance][gardener-docs-shoot-maintenance] in the Gardener
documentation.

#### Hibernation

![](/assets/kubernetes/wke/gardener-create-cluster-hibernation.png)

Some clusters are not required to run all the time and Gardener allows
you to automatically scale-down all cluster resources to zero by
configuring a [hibernation schedule][gardener-docs-shoot-hibernation].

### Creation

Create the cluster by clicking **Create** in the bottom right corner.

You should see the new shoot bootstrapping in the list of clusters.
This process can take several minutes.

![](/assets/kubernetes/wke/gardener-db-cluster-bootstrap.png)

{{% alert title="Quotas" %}}

Note that the creation of cluster resources is subject to quotas on
your Wavestack project. You should make sure that you choose suitable
worker node instance types (i.e. flavors) to ensure that you stay
within your quotas.

Get in touch with wavestack-support@wavecon.de if you require a quota
increase.

{{% /alert %}}

## Access a Cluster

Once your new cluster has finished bootstrapping, you can configure
access to it via kubectl.

Gardener supports secure authentication via OIDC with
[gardenlogin][gh-garden-gardenlogin] and
[kubelogin][gh-int128-kubelogin].

### Configure gardenlogin

Create `~/.garden/gardenlogin.yaml` with the following content:

```yaml
gardens:
  - identity: wavestack
    kubeconfig: ~/.garden/gardenctl-v2.yaml
```

Navigate to [your account][wvst-gardener-account] on the Gardener
dashboard and download the kubeconfig to the garden cluster. Save it as
`~/.garden/gardenctl-v2.yaml`.

![](/assets/kubernetes/wke/gardener-db-account-kubeconfig.png)

### Use kubectl

The `kubectl` command [can be
configured][k8s-concepts-config-cluster-access] to access clusters
using kubeconfig files. You can download suitable ones files for
your clusters from the [cluster overview][wvst-gardener-dashboard]
page by clicking on the key symbol.

![](/assets/kubernetes/wke/gardener-db-cluster-key.png)

Download the **Kubeconfig - Gardenlogin** file.

![](/assets/kubernetes/wke/gardener-cluster-access.png)

The file will be named similar to
`kubeconfig-gardenlogin--<project_id>--<cluster_name>.yaml`. Save the downloaded
file in the `~/.kube/` directory.

You can either rename it to `~/.kube/config` if you will only ever
manage a single cluster, or [configure
kubectl][k8s-concepts-config-cluster-access] to use a specific one by
setting `KUBECONFIG` environment variable.

```cli
❯ export KUBECONFIG=~/.kube/kubeconfig-gardenlogin--<project_id>--<cluster_name>.yaml
```

Check your available nodes by running:

```cli
❯ kubectl get nodes
NAME                                                        STATUS   ROLES    AGE   VERSION
shoot--d51qb1zx3h--h9jx8ess44-worker-wwug5-z1-75d64-7hwx2   Ready    <none>   14m   v1.26.5
shoot--d51qb1zx3h--h9jx8ess44-worker-wwug5-z1-75d64-j6mm4   Ready    <none>   14m   v1.26.5
```

## Deploy a Workload

### Create a Deployment

Use the `kubectl create` command to create a simple Deployment that
manages a Pod running the [agnhost][gh-k8s-k8s-test-images-agnhost] image.

```cli
❯ kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost netexec --http-port=8080
deployment.apps/hello-node created
```

View the deployment:

```cli
❯ kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   1/1     1            1           51m
```

### Create a Service

Expose the Pod to the public internet using the kubectl expose
command:

```cli
❯ kubectl expose deployment hello-node --type=LoadBalancer --port=8080
service/hello-node exposed
```

The `--type=LoadBalancer` flag indicates that you want to expose your
Service outside of the cluster and triggers the creation of a load
balancer.

View the newly created Service:

```cli
❯ kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-node   LoadBalancer   100.98.31.115   <pending>     8080:31058/TCP   5s
kubernetes   ClusterIP      100.96.0.1      <none>        443/TCP          18h
```

You can see that the external IP of the service is still `<pending>`
while the load balancer is being provisioned.

Once the provisioning has finished, the Service status will be
updated:

```cli
❯ kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
hello-node   LoadBalancer   100.98.31.115   31.172.116.232   8080:31058/TCP   2m4s
kubernetes   ClusterIP      100.96.0.1      <none>           443/TCP          18h
```

You can test the service by running the following command:

```cli
❯ curl http://31.172.116.232:8080
NOW: 2023-06-27 06:51:28.925024573 +0000 UTC m=+144.187462730
```

## Delete a Cluster

You can delete clusters by clicking on the three dots to the left on
the cluster overview and choosing **Delete Cluster**

![](/assets/kubernetes/wke/gardener-db-cluster-delete.png)

<!-- References -->

[gardener]: https://gardener.cloud/
[gardener-api-shoot]: https://gardener.cloud/docs/gardener/api-reference/core/#core.gardener.cloud/v1beta1.Shoot
[gardener-cluster-autoscaler]: https://github.com/gardener/autoscaler/
[gardener-docs]: https://gardener.cloud/docs/gardener/
[gardener-docs-api-ref]: https://gardener.cloud/docs/gardener/api-reference/
[gardener-docs-dashboard]: https://gardener.cloud/docs/dashboard/
[gardener-docs-shoot-hibernation]: https://gardener.cloud/docs/gardener/usage/shoot_hibernate/
[gardener-docs-shoot-maintenance]: https://gardener.cloud/docs/gardener/usage/shoot_maintenance/
[gardener-docs-shoot-purposes]: https://gardener.cloud/docs/gardener/usage/shoot_gh/
[gh-calico]: https://github.com/projectcalico/calico
[gh-cilium]: https://github.com/cilium/cilium/
[gh-containernetworking-cni]: https://github.com/containernetworking/cni
[gh-garden-gardenlogin]: https://github.com/gardener/gardenlogin
[gh-garden-gardenlogin-install]: https://github.com/gardener/gardenlogin#installation
[gh-gardener-machine-controller-manager]: https://github.com/gardener/machine-controller-manager
[gh-int128-kubelogin]: https://github.com/int128/kubelogin
[gh-int128-kubelogin-setup]: https://github.com/int128/kubelogin#setup
[gh-k8s-k8s-test-images-agnhost]: https://github.com/kubernetes/kubernetes/tree/master/test/images/agnhost
[k8s]: https://kubernetes.io/
[k8s-cluster-autoscaler]: https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler
[k8s-concepts-arch-ccm]: https://kubernetes.io/docs/concepts/architecture/cloud-controller/
[k8s-concepts-admin-networking]: https://kubernetes.io/docs/concepts/cluster-administration/networking/
[k8s-concepts-config-cluster-access]: https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/
[k8s-tasks-install-tools]: https://kubernetes.io/docs/tasks/tools/
[scs]: https://scs.community/
[scs-flavor-naming-v3]: https://docs.scs.community/standards/scs-0100-v3-flavor-naming/
[wvst-gardener-dashboard]: https://dashboard.gardener.wavestack.cloud
[wvst-gardener-account]: https://dashboard.gardener.wavestack.cloud/account
