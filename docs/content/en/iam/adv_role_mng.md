---
title: "Advanced Role and Permission Management"
linkTitle: "Advanced Roles"
weight: 400
description: >
  Guide to creating and configuring application credentials in Wavestack for managing VMs, images, and networks.
---

While basic role management works well for most standard use cases, advanced scenarios may require more control.
[Application Credentials](https://docs.openstack.org/keystone/latest/user/application_credentials.html) give you more flexibility in defining roles and more precise control over how you manage
VMs, images, and networks in Wavestack. This guide will walk you through creating and configuring these credentials to
manage virtual machines (VMs), images, and networks effectively.

## Scope

Managing volumes is out of scope for this guide. You can still create functioning VMs without any problems.
Connectivity to external networks is also out of scope for this guide (internal networking works completely fine). Creating ports is also out of scope, but you can add existing ports to routers. Please contact your network admin if you need help setting up this kind of things.

## Steps to Create an Application Credential

1. Navigate to Identity - Application Credentials in the [Wavestack Dashboard](https://dashboard.wavestack.de/identity/application_credentials/) and click on `Create Application Credential`.

1. **Name:** Enter a descriptive name for the application credential, for example, `VM-Management-Credential`.

1. **Description:** Provide a brief description, such as `Credential for managing VM states`.

1. **Secret:** Either generate a secret or provide your own. Make sure to note it down, as it will only be displayed
   once.

1. **Expiration Date/Time:** Optionally set an expiration date and time. Without these, the credential will not expire.

1. **Roles:** Select the relevant [role(s)](https://docs.openstack.org/keystone/latest/admin/service-api-protection.html) necessary for managing the resources. Typically, roles like `admin`, `image_admin`, or `network_admin` are used.

1. **Access Rules:** Define the access rules in JSON or YAML format for more fine-grained control. Below are examples
   for managing VM states, images, and networks.

1. **Unrestricted:** By default, for security reasons, application credentials are forbidden from being used for creating
  additional application credentials or keystone trusts. If your application credential needs to be able to perform
  these actions, check `Unrestricted`. However, this is generally not recommended for security purposes.

## Example Configuration for VM Management

```json
[
    {
        "path": "/v2.1/servers",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/detail",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}",
        "method": "PUT",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}",
        "method": "DELETE",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/action",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/remote-consoles",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/diagnostics",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/os-volume_attachments",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/os-volume_attachments/{volume_id}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/flavors",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/flavors/detail",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/flavors/{flavor_id}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs/{keypair_name}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs/{keypair_name}",
        "method": "DELETE",
        "service": "compute"
    }
]
```

## Example Configuration for Image Management

```json
[
    {
        "path": "/v2/images",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}",
        "method": "PATCH",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}",
        "method": "DELETE",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/actions/deactivate",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/actions/reactivate",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/file",
        "method": "PUT",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/file",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members/{member_id}",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members/{member_id}",
        "method": "PUT",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members/{member_id}",
        "method": "DELETE",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/tags/{tag}",
        "method": "PUT",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/tags/{tag}",
        "method": "DELETE",
        "service": "image"
    }
]
```

## Example Configuration for Network Management

```json
[
    {
        "path": "/v2.0/networks",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/networks",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/networks/{network_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/networks/{network_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/networks/{network_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets/{subnet_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets/{subnet_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets/{subnet_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules/{security_group_rule_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules/{security_group_rule_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups/{security_group_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups/{security_group_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups/{security_group_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/ports",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/ports",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}/add_allowed_address_pairs",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}/remove_allowed_address_pairs",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/routers",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/add_router_interface",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/remove_router_interface",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/add_extraroutes",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/remove_extraroutes",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/add_external_gateways",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/update_external_gateways",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/remove_external_gateways",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips/{floatingip_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips/{floatingip_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips/{floatingip_id}",
        "method": "DELETE",
        "service": "network"
    }
]
```

## Comprehensive Example

Here is a comprehensive configuration that includes VM management, image management, and network management:

- **Name:** Comprehensive-Management-Credential
- **Description:** Credential for managing VM states, images, and networks
- **Secret:** (generated or provided)
- **Expiration Date/Time:** (optional)
- **Roles:** admin (or multiple specified roles such as `vm_admin`, `image_admin`, `network_admin`)
- **Access Rules:**

```json
[
    {
        "path": "/v2.1/servers",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/detail",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}",
        "method": "PUT",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}",
        "method": "DELETE",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/action",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/remote-consoles",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/diagnostics",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/os-volume_attachments",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/servers/{server_id}/os-volume_attachments/{volume_id}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/flavors",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/flavors/detail",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/flavors/{flavor_id}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs",
        "method": "POST",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs/{keypair_name}",
        "method": "GET",
        "service": "compute"
    },
    {
        "path": "/v2.1/os-keypairs/{keypair_name}",
        "method": "DELETE",
        "service": "compute"
    },
    {
        "path": "/v2/images",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}",
        "method": "PATCH",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}",
        "method": "DELETE",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/actions/deactivate",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/actions/reactivate",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/file",
        "method": "PUT",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/file",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members",
        "method": "POST",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members/{member_id}",
        "method": "GET",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members/{member_id}",
        "method": "PUT",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/members/{member_id}",
        "method": "DELETE",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/tags/{tag}",
        "method": "PUT",
        "service": "image"
    },
    {
        "path": "/v2/images/{image_id}/tags/{tag}",
        "method": "DELETE",
        "service": "image"
    },
    {
        "path": "/v2.0/networks",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/networks",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/networks/{network_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/networks/{network_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/networks/{network_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets/{subnet_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets/{subnet_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/subnets/{subnet_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules/{security_group_rule_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-group-rules/{security_group_rule_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups/{security_group_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups/{security_group_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/security-groups/{security_group_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/ports",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/ports",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}/add_allowed_address_pairs",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/ports/{port_id}/remove_allowed_address_pairs",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/routers",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}",
        "method": "DELETE",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/add_router_interface",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/remove_router_interface",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/add_extraroutes",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/remove_extraroutes",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/add_external_gateways",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/update_external_gateways",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/routers/{router_id}/remove_external_gateways",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips",
        "method": "POST",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips/{floatingip_id}",
        "method": "GET",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips/{floatingip_id}",
        "method": "PUT",
        "service": "network"
    },
    {
        "path": "/v2.0/floatingips/{floatingip_id}",
        "method": "DELETE",
        "service": "network"
    }
]
```

## Additional Information

For more information, visit the [OpenStack Application Credentials documentation](https://docs.openstack.org/keystone/latest/user/application_credentials.html).

[Compute Service API reference](https://docs.openstack.org/api-ref/compute/index.html)

[Image Service API reference](https://docs.openstack.org/api-ref/image/index.html)

[Networking Service API reference](https://docs.openstack.org/api-ref/network/index.html)
