---
title: "Advanced Role and Permission Concept"
linkTitle: "Advanced Roles"
weight: 2
description: >
  Guide to creating and configuring application credentials in Wavestack for managing VMs, images, and networks.
---

## Advanced Role and Permission Concept

Application Credentials are a powerful way to define and assign roles flexibly and granularly in Wavestack. This guide
will walk you through creating and configuring these credentials to manage virtual machines (VMs), images, and networks
effectively.

### Steps to Create an Application Credential

1. Navigate to Identity - Application Credentials in the [Wavestack Dashboard](https://dashboard.wavestack.de/identity/application_credentials/) and click on `Create Application Credential`.

1. **Name:** Enter a descriptive name for the application credential, for example, `VM-Management-Credential`.

1. **Description:** Provide a brief description, such as `Credential for managing VM states`.

1. **Secret:** Either generate a secret or provide your own. Make sure to note it down, as it will only be displayed
   once.

1. **Expiration Date/Time:** Optionally set an expiration date and time. Without these, the credential will not expire.

1. **Roles:** Select the relevant [role(s)](https://docs.openstack.org/keystone/latest/admin/service-api-protection.html) necessary for managing the resources. Typically, roles
   like `admin`, `image_admin`, or `network_admin` are used.

1. **Access Rules:** Define the access rules in JSON or YAML format for more fine-grained control. Below are examples
   for managing VM states, images, and networks.

### Example Configuration for VM Management

```json
// TODO
```

### Example Configuration for Image Management

```json
// TODO
```

### Example Configuration for Network Management

```json
// TODO
```

### YAML Format for Access Rules

If you prefer using YAML format, here are the same rules:

#### VM Management

```yaml
# TODO
```

#### Image Management

```yaml
# TODO
```

#### Network Management

```yaml
# TODO
```

### Unrestricted Access

- **Unrestricted:** By default, for security reasons, application credentials are forbidden from being used for creating
  additional application credentials or keystone trusts. If your application credential needs to be able to perform
  these actions, check `Unrestricted`. However, this is generally not recommended for security purposes.

### Comprehensive Example

Here is a comprehensive configuration that includes VM management, image management, and network management:

- **Name:** Comprehensive-Management-Credential
- **Description:** Credential for managing VM states, images, and networks
- **Secret:** (generated or provided)
- **Expiration Date/Time:** (optional)
- **Roles:** admin (or multiple specified roles such as `vm_admin`, `image_admin`, `network_admin`)
- **Access Rules:**

```json
// TODO
```

Or in YAML format:

```yaml
# TODO
```

### Additional Information

For more information, visit
the [OpenStack Application Credentials documentation](https://docs.openstack.org/keystone/latest/user/application_credentials.html).

[Compute Service API](https://docs.openstack.org/api-ref/compute/index.html)

[Image Service API](https://docs.openstack.org/api-ref/image/index.html)

[Networking Service API](https://docs.openstack.org/api-ref/network/index.html)
