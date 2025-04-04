---
title: "noris Sovereign Cloud"
linkTitle: "noris Sovereign Cloud"
description: "Manage Kubernetes clusters with nSC"
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

The noris Sovereign Cloud (nSC) automates management and operation of [Kubernetes][k8s] clusters as a service.

nSC is built using [Gardener][gardener], and you can find more information in their [documentation][gardener-docs].

{{% alert color="warning" %}}

Please note that nSC is currently in **internal-alpha-test** and has not yet been released as GA.

We encourage you to try nSC, but keep in mind that we are unable to provide SLAs for this product at this time.

{{% /alert %}}

## Overview

This guide will walk you through the steps to create, access and manage a Kubernetes cluster with nSC.

## Access the dashboard

Log in to the [Gardener dashboard][wvst-gardener-dashboard] with your Wavestack account.

## Create a cluster

Click the **+** button at the top to begin creating a new Kubernetes cluster or "shoot" in Gardener terminology.

![](/assets/kubernetes/nsc/gardener-db-create-cluster.png)

### Configuration

The cluster configuration wizard allows you to customize cluster settings according to your needs. For better readability only settings that usually need user attention are documented.

#### Infrastructure

Gardener supports the following providers:

- **openstack** - nSC

#### Cluster Details

In this section, you can customize various cluster settings:
![](/assets/kubernetes/nsc/gardener-create-cluster-details.png)

##### Cluster name

Gardener will generate a random default name for your cluster, or you can specify your own.

##### Kubernetes version

Clusters can be created with different Kubernetes versions. It's recommended to always use the latest supported version. Please note our policy on [supported kubernetes versions](#lifecycle-management)

##### Cluster purpose

This option indicates the intended use or production-readiness level of the cluster. For more information on the different configurations based on purpose, refer to [shoot purposes][gardener-docs-shoot-purposes].

#### Control Plane High Availability

![](/assets/kubernetes/nsc/gardener-create-cluster-ha-controlplane.png)

nSC supports HA control planes. Enabling this feature will setup a three instance etcd cluster, distributed over our datacenters within the same geolocation. If disabled, a single instance etcd cluster will be used instead. For production clusters, we recommend enabling ControlPlaneHighAvailability to ensure greater resilience.

#### DNS Configuration

![](/assets/kubernetes/nsc/gardener-create-cluster-dns.png)

This is advanced configuration which is explained in [further detail](#dns) below. Beginners can likely ignore this.

#### Worker

![](/assets/kubernetes/nsc/gardener-create-cluster-worker-groups.png)

##### Group Name

Gardener will generate a random default name for your worker-group, or you can specify your own.

##### Machine Type

Choose the machine type for your worker nodes. Wavestack follows the [Sovereign Cloud Stack][scs] naming conventions:

- [SCS Flavor Naming Standard][scs-flavor-naming-v3]

##### Machine Image

Similar to Kubernetes version this specifies the operating system image. It's recommended to always use the latest supported version. Please note our policy on [supported machine images](#flatcar)

##### Autoscaling

Clusters with at least one worker group having `minimum < maximum` nodes will have an autoscaler deployment, allowing dynamic scaling of worker nodes based on demand.

The autoscaler is a modified version of the Kubernetes [cluster-autoscaler][k8s-cluster-autoscaler] with additional support for [gardener/machine-controller-manager][gh-gardener-machine-controller-manager].

nSC requires clusters to consist of at least two nodes.

##### Zones

Similar to [Control Plane High Availability](#control-plane-high-availability), you can also distribute your workers across different data centers within the same geolocation. Keep in mind that PVC storage is specific to each zone and not replicated between them. If you require replicated storage, consider our S3 storage.

#### Maintenance

![](/assets/kubernetes/nsc/gardener-create-cluster-maintenance.png)

Gardener configures an automated update window for tasks like:

- Kubernetes patch releases (for control plane and worker nodes)
- Worker node machine images

The default value of this time frame may differ between cluster creations.

For more details, refer to [shoot maintenance][gardener-docs-shoot-maintenance] in the Gardener documentation.

#### Hibernation
If your cluster doesn't need to run all the time, you can configure a hibernation schedule to automatically scale down all resources to zero.

Please notice that hibernation is subject to fair use and may be restricted at any time.

### Advanced Configuration - YAML

![](/assets/kubernetes/nsc/gardener-create-cluster-yaml.png)

You can edit the generated Gardener custom resources directly by selecting the **YAML** tab. This gives you the option to specify settings not available in the configuration wizard. Later parts of this guide will often ask you to add code snippets in your shoot's declaration, this is what they are meant for.

The full range of options is showcased in the upstream [example shoot config](https://github.com/gardener/gardener/blob/master/example/90-shoot.yaml). OpenStack specific settings are documented [here](https://github.com/gardener/gardener-extension-provider-openstack/blob/master/docs/usage/usage.md)

#### Networking

The default network layout for shoot clusters is limited to 64 nodes and can not be changed after initial cluster creation:
```
10.44.0.0/24 for nodes
10.98.0.0/18 for services
10.194.0.0/18 for pods
```

If you need more nodes, adjust the networking ranges in the shoots YAML configuration during setup. Please refrain from using any of the following prefixes which are reserved for our infrastructure:
```
10.42.0.0/15
10.96.0.0/15
10.192.0.0/15
```

## Access a Cluster

To follow this guide, you need to have the following tools installed:

- [kubectl][k8s-tasks-install-tools]
- [gardenlogin][gh-garden-gardenlogin-install]
- [kubelogin][gh-int128-kubelogin-setup]

Once the cluster has finished bootstrapping, you can set up kubectl access.

Gardener supports secure authentication via OIDC with [gardenlogin][gh-garden-gardenlogin] and [kubelogin][gh-int128-kubelogin]. [gardenctl](https://github.com/gardener/gardenctl-v2?tab=readme-ov-file#installation) can serve as a replacement for gardenlogin, but for simplicity of this guide, only gardenlogin will be explained.

#### Configure gardenlogin

Create the configfile `~/.garden/gardenctl-v2.yaml` with the following content:

```yaml
gardens:
  - identity: dev
    kubeconfig: ~/.garden/kubeconfig-garden.yaml
```

This kubeconfig referenced in `gardenctl-v2.yaml` can be configured and downloaded [here](https://dashboard.ingress.garden-runtime.staging-op.noris.de/account).

![](/assets/kubernetes/nsc/gardener-db-account-kubeconfig.png)

#### Use kubectl

You can download the appropriate kubeconfigs for your clusters from the [cluster overview][wvst-gardener-dashboard] page by clicking on the key symbol.

![](/assets/kubernetes/nsc/gardener-db-cluster-key.png)

Download the **Kubeconfig - Gardenlogin** file.

![](/assets/kubernetes/nsc/gardener-cluster-access.png)

Save it in the `~/.kube/` directory.

If you’re managing only a single cluster, you can rename the file to `~/.kube/config`. Otherwise, you can [configure kubectl][k8s-concepts-config-cluster-access] to use a specific file by setting the `KUBECONFIG` environment variable.

```cli
❯ export KUBECONFIG=~/.kube/kubeconfig-gardenlogin--<project_id>--<cluster_name>.yaml
```

Check your available nodes by running:

```cli
❯ kubectl get nodes
NAME
shoot--example--example-worker-r6tfb-z1-76469-ttt8g
shoot--example--example-worker-r6tfb-z1-76469-xb9dv
```

You can now utilize your new nSC kubernetes cluster.

## Lifecycle management

### Kubernetes

Our supported versions can be easily identified by the "supported" flag during cluster creation. In alignment with the [upstream Kubernetes project](#https://kubernetes.io/releases/#release-history), our goal is to support the latest patch release of the three most recent Kubernetes minor versions. The latest minor version may initially be available only as a preview while we work on adding full support. While we have confidence in the quality of our preview releases, we advise against using them for critical tasks and do not provide SLAs for them.

Older patch and minor releases will be flagged as deprecated, attaching an expiration date to them. If you configured automatic kubernetes upgrades, your shoot clusters be upgraded in the next maintenance window.

On reaching said expiration date, shoot clusters using that deprecated kubernetes versions will be **force-upgraded** to a supported version **regardless** of the automatic update configuration. Upstream kubernetes actively deprecates and removes legacy functions. It is recommmended to keep up with the [upstream deprecation notices](https://kubernetes.io/docs/reference/using-api/deprecation-guide/) and to test upgrades preemptively.

### Flatcar
Our supported version is easily identifyable by the "supported" flag during cluster creation. We aim to support the [latest flatcar LTS release](https://www.flatcar.org/releases#lts-release) and keep the lastest 3 images available for installation. The latest minor version may initially be available only as a preview while we work on adding full support. While we have confidence in the quality of our preview releases, we advise against using them for critical tasks and do not provide SLAs for them.

Older releases are flagged as deprecated attaching an expiration date to them. If you configured automatic OS upgrades, flatcar worker nodes running deprecated flatcar releases will be upgraded in the next maintenance window.

On reaching said expiration date, worker nodes using these deprecated flatcar OS images will be **force-upgraded** to a supported version **regardless** of the automatic update configuration. While impact of OS image upgrades on container workload is fairly seldom, we advice to keep track of upstream deprecation notices and to test upgrades preemptively.

## Controlplane Logs

Controlplane logs can be easily accessed through the web interface:

![](/assets/kubernetes/nsc/gardener-db-logging.png)

Search for "log":

![](/assets/kubernetes/nsc/gardener-db-logging-dashboard-search.png)

## Monitoring

You can configure email receivers in the shoot spec to automatically send email notifications for predefined control plane alerts. 

Example configuration:
```cli
spec:
  monitoring:
    alerting:
      emailReceivers:
      - <your_email_address>
```

If you require more customized alerts for control plane metrics, you can deploy your own Prometheus instance within your shoot's control plane. Using Prometheus federation, you can forward metrics from the Gardener-managed Prometheus to your custom Prometheus deployment. The credentials and endpoint for the Gardener-managed Prometheus are provided via the Gardener dashboard.

## Network observability

Our default CNI cilium offers a free network observability feature called hubble:
![](/assets/kubernetes/nsc/gardener-advanced-hubble.png)

To utilize it, add the following to your shoot declaration:
```
spec:
  networking:
    type: cilium
    providerConfig:
      overlay:
        enabled: true
      hubble:
        enabled: true
```

Afterwards, running the following code will allow you to access a webfrontend of your network flows on your local machine. The cilium binary can be obtained [here](https://github.com/cilium/cilium-cli/releases).

```cli
❯ cilium hubble ui
ℹ️  Opening "http://localhost:12000" in your browser...
```

## Openstack Server Groups

nSC allows the user to create a server group by the following shoot declaration add-ins. These make nSC aware of the cluster and can be useful to spread the workload over multiple hardware nodes.

```
spec:
  provider:
    type: openstack
    workers:
      - ...
        providerConfig:
          apiVersion: openstack.provider.extensions.gardener.cloud/v1alpha1
          kind: WorkerConfig
          serverGroup:
            policy: soft-anti-affinity
```

## Registry Cache

Our CRI containerd supports the use of [registry mirrors][registry-mirror]. This feature is helpful for bypassing registry rate limits (e.g., hub.docker.com) and integrating a centralized container security scanner. Here's an example of how to implement this:

```
spec:
  extensions:
  - type: registry-mirror
    providerConfig:
      apiVersion: mirror.extensions.gardener.cloud/v1alpha1
      kind: MirrorConfig
      mirrors:
      - upstream: docker.io
        hosts:
        - host: "https://mirror.gcr.io"
          capabilities: ["pull"]
```

## DNS

Per default, our openstack-designate creates a subdomain for your cluster:

![](/assets/kubernetes/nsc/gardener-db-default-domain.png)

Please refer to [this](https://github.com/gardener/gardener-extension-shoot-dns-service/blob/master/docs/usage/dns_names.md#request-dns-names-in-shoot-clusters) upstream documentation for instructions on how to automatically create DNS records that enable your services to be publicly accessible under this subdomain. In case your usecase is even more advanced, be sure to check out the complete [upstream documentation](https://github.com/gardener/gardener-extension-shoot-dns-service/tree/master/docs/usage).

### Utilize your own subdomain

Using your own subdomain can be achieved in different ways:
1) If you want to federate a subdomain of your own domain to our openstack-designate, please open a support ticket.

2) Additionally, gardener can manage the following DNS providers. Please note that we can't offer any SLA's regarding compatibility with external DNS servers:
- openstack-designate
- aws-route53
- azure-dns
- azure-private-dns
- google-clouddns
- alicloud-dns
- infoblox-dns
- netlify-dns
- rfc2136

![](/assets/kubernetes/nsc/gardener-db-dns-secrets.png)

## Certificates

nSC allows the user to comfortably request free certificates via DNS-based Let’s Encrypt challenges. This feature depends on properly setup [DNS](#dns), so we strongly advice to follow that section of the guide first. [This](https://github.com/gardener/gardener-extension-shoot-cert-service/blob/master/docs/usage/request_default_domain_cert.md) documentation covers, how to set up certificates for the default subdomain. For advanced usecases, check out the complete [upstream documentation](https://github.com/gardener/gardener-extension-shoot-cert-service/tree/master/docs/usage).

## Data Backup and Recovery

Please note that nSC does *not* back up customer-generated content. It is the customer's responsibility to back up worker node objects such as metadata (e.g., services, ingresses, pods, etc.), the content of PVs (e.g., prometheus-01), and S3 buckets. [Velero][velero]  is a popular open-source tool that can assist with this task.

However, nSC *does* take responsibility for backing up control-plane content, including the shoot's etcd, on a regular basis. Please specify the etcd resources that need to be [encrypted](#security---etcd-encryption).

## Security - Disabling ssh access

SSH access can be disabled by specifying the following setting in the shoot declaration:
```
spec:
  provider:
    workersSettings:
      sshAccess:
        enabled: false
```

## Security - etcd encryption

Shoot declarations can specify which etcd fields need to be encrypted:
```
spec:
  kubernetes:
    kubeAPIServer:
      encryptionConfig:
        resources:
          - configmaps
          - statefulsets.apps
          - customresource.fancyoperator.io
```
Full documentation on the etcd encryption feature can be found [here][etcd-encryption]

## Security - PodSecurity Admission Plugins

nSC allows users to configure their podsecurity admission defaults in the shoot declaration. These are used if mode labels are not set by an application:

```
spec:
  kubernetes:
    kubeAPIServer:
      admissionPlugins:
      - name: PodSecurity
        config:
          apiVersion: pod-security.admission.config.k8s.io/v1
          kind: PodSecurityConfiguration
          # Level label values must be one of:
          # - "privileged" (default)
          # - "baseline"
          # - "restricted"
          defaults:
            enforce: "baseline"
            audit: "baseline"
            warn: "baseline"
          exemptions:
            # Array of authenticated usernames to exempt.
            usernames: []
            # Array of runtime class names to exempt.
            runtimeClasses: []
            # Array of namespaces to exempt.
            namespaces: []
```

## Cost Estimation

Due to the service's dynamic nature, the actual costs can vary significantly based on factors such as cluster size, storage usage, load balancers, public IP reservations, hibernation and the utilization of the auto-scaling feature.

The minimal estimate for the smallest possible cluster (in cloud points) is as follows:

### Cloud Points
- **1x Shoot Cluster Fee**: 1.000.000 CP  # FIXME add real price and product name
- **2x SCS-4V-8-20**: 1.000.000 CP

A cluster must always include at least two nodes. This minimal configuration does not require additional load balancers, public IPs or PVC storage, though these can be added by the user as needed.

## Security + C5 Compliant Shoot Clusters

This section contains specific information about achieving [C5][c5] compliant shoot clusters. It may also provide helpful suggestions for general security improvements. Please contact your sales representative for consulting quotas.

{{% alert color="warning" %}}

The C5 certification is still in progress, but it is actively being worked on and is a top priority on the roadmap. It is highly likely that a C5-compliant shoot cluster will require customer adjustments. The following list offers an informed estimate of what a C5-compliant shoot cluster declaration may include.

{{% /alert %}}

### Ref. OPS-04 - OPS-05 | Protection Against Malware

Disabling [SSH](#security---disabling-ssh-access) is likely a step in the right direction. Our containers are pulled through a [container registry cache that implements vulnerability scanning](#registry-cache). Customers can utilize the same mechanism for their own registry cache (e.g., Harbor). Our default OS image, Flatcar, is immutable after the initial boot.

### Ref. OPS-06 - OPS-09 | Data Backup and Recovery

This topic is explained in [Data Backup and Recovery](#data-backup-and-recovery). etcd encryption is documented [here](#security---etcd-encryption).

## Extensions

The Gardener platform can be extended with the following additional extensions. If you'd like us adding this functionality, please request a quote.
- [extension-shoot-networking-filter](https://github.com/gardener/gardener-extension-shoot-networking-filter/blob/master/docs/usage/shoot-networking-filter.md#ingress-filtering) - firewalls e.g. blackholing
- [shoot-rsyslog-relp](https://github.com/gardener/gardener-extension-shoot-rsyslog-relp/blob/main/docs/usage/configuration.md) - forward audit logs to remote relp server
- [shoot-falco-service](https://github.com/gardener/gardener-extension-shoot-falco-service/blob/main/docs/FalcoExtension.md#alternatives) - anomaly detection
- [shoot-lakom-service](https://github.com/gardener/gardener-extension-shoot-lakom-service/blob/main/docs/usage/lakom.md) - enforce container image signing


## Deleting a Cluster

To delete a cluster, click on the three dots on the left side of the cluster overview and select "Delete Cluster"

![](/assets/kubernetes/nsc/gardener-db-cluster-delete.png)

<!-- References -->

[gardener]: https://gardener.cloud/
[gardener-api-shoot]: https://gardener.cloud/docs/gardener/api-reference/core/#core.gardener.cloud/v1beta1.Shoot
[gardener-cluster-autoscaler]: https://github.com/gardener/autoscaler/
[gardener-docs]: https://gardener.cloud/docs/gardener/
[gardener-docs-api-ref]: https://gardener.cloud/docs/gardener/api-reference/
[gardener-docs-dashboard]: https://gardener.cloud/docs/dashboard/
[gardener-docs-shoot-hibernation]: https://gardener.cloud/docs/gardener/shoot/shoot_hibernate/
[gardener-docs-shoot-maintenance]: https://gardener.cloud/docs/gardener/shoot/shoot_maintenance/
[gardener-docs-shoot-purposes]: https://gardener.cloud/docs/gardener/shoot/shoot_purposes/
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
[wvst-gardener-dashboard]: https://dashboard.ingress.garden-runtime.staging-op.noris.de
[wvst-gardener-account]: https://dashboard.ingress.garden-runtime.staging-op.noris.de/account
[extensions]:  https://github.com/orgs/gardener/repositories?language=&q=extension&sort=&type=all
[c5]: https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/CloudComputing/ComplianceControlsCatalogue/2020/C5_2020_editable.html
[registry-mirror]: https://github.com/gardener/gardener-extension-registry-cache/blob/main/docs/usage/registry-mirror/configuration.md#shoot-configuration
[velero]: https://gardener.cloud/docs/guides/administer-shoots/backup-restore/
[etcd-encryption]: https://gardener.cloud/docs/gardener/security/etcd_encryption_config/
