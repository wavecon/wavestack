---
title: "Operating System Images"
linkTitle: "OS Images"
weight: 100
description: >
  Overview of the operating system images provided by Wavestack for instance deployment.
---

## Default Available Images

Wavestack provides the following operating system images by default for the deployment of instances. These images are
maintained and regularly updated to ensure access to the latest versions:

- **AlmaLinux**
- **Cirros**
- **Debian**
- **Fedora**
- **Rocky Linux**
- **Ubuntu LTS**
- **Ubuntu Minimal LTS**

Each of these operating system variants is kept up-to-date with the latest stable releases and maintained until their
security support ends. This ensures that your instances are always running on the most secure and reliable versions
available.

For a full and current list of available images, visit
the [Wavestack Horizon Dashboard](https://dashboard.wavestack.de/project/images).

## Custom Images

In addition to the default images provided by Wavestack, customers have the flexibility to upload and use their own
custom images for instance deployment, offering greater flexibility and customization to meet specific project needs.

### Uploading Custom Images via OpenStack CLI

To upload your own private image using the OpenStack CLI, follow these steps:

1. **Prepare the Image**: Ensure the image file is in a compatible format, such as QCOW2 or RAW. If necessary, convert
   the image to the desired format.

2. **Authenticate**: Source your OpenStack RC file to authenticate:
   ```bash
   source /path/to/your-openrc.sh
   ```

3. **Upload the Image**:
   Use the `openstack image create` command to upload the image:
   ```bash
   openstack image create "YourImageName" \
     --file /path/to/your-image-file.qcow2 \
     --disk-format qcow2 \
     --container-format bare
   ```

    - Replace `"YourImageName"` with the desired name for your image.
    - Replace `/path/to/your-image-file.qcow2` with the path to your image file.
    - Adjust the `--disk-format` and `--container-format` if you're using a different format.

4. **Verify the Upload**: Check that your image was successfully uploaded:
   ```bash
   openstack image list
   ```

   Your custom image should now appear in the list and be ready for use in instance deployment.
