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

## Verify Provided Images

To verify the integrity of an provided image by comparing it with the original vendor image, follow these steps.

### Prerequisites

- Ensure the OpenStack client is installed and configured on your system.
- Install qemu-img on your local machine.

{{< tabpane >}}
{{< tab header="Debian/Ubuntu" lang=bash >}}
sudo apt-get install qemu-utils
{{< /tab >}}
{{< tab header="CentOS/RHEL" lang=bash >}}
sudo yum install qemu-img
{{< /tab >}}
{{< tab header="macOS (using Homebrew)" lang=bash >}}
brew install qemu
{{< /tab >}}
{{< /tabpane >}}

### Steps To Verify Image

#### 1. Authenticate with OpenStack

Source your RC file to set the necessary environment variables:

```bash
source /path/to/your-openrc.sh
```

When prompted for a password, please create and use application credentials instead.

#### 2. Retrieve Image Details

List the images available in your OpenStack project:

```bash
openstack image list
```

Identify the image in question and retrieve its details:

```bash
openstack image show <image-id-or-name>
```

In the output, note the checksum value and look for the image_source property under properties. This property should
contain the URL to the original QCOW2 base image.

##### Example Output

```
+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                                       |
+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| checksum         | 7e261963781e56a3aaf10b100d2b3760                                                                                                                                                            |
| container_format | bare                                                                                                                                                                                        |
| created_at       | 2024-08-05T12:42:24Z                                                                                                                                                                        |
| disk_format      | raw                                                                                                                                                                                         |
| file             | /v2/images/2762f95c-2ed1-422d-944b-3fac27a426a6/file                                                                                                                                        |
| id               | 2762f95c-2ed1-422d-944b-3fac27a426a6                                                                                                                                                        |
| min_disk         | 8                                                                                                                                                                                           |
| min_ram          | 512                                                                                                                                                                                         |
| name             | Debian 10                                                                                                                                                                                   |
| owner            | ae99dfce26844f0c9fb1872e92f7439c                                                                                                                                                            |
| properties       | architecture='x86_64', hotfix_hours='0', hw_disk_bus='scsi', hw_qemu_guest_agent='True', hw_rng_model='virtio', hw_scsi_model='virtio-scsi', hw_watchdog_action='reset',                    |
|                  | hypervisor_type='qemu', image_build_date='2024-07-03', image_description='Debian 10', image_original_user='debian',                                                                         |
|                  | image_source='https://cloud.debian.org/images/cloud/buster/20240703-1797/debian-10-genericcloud-amd64-20240703-1797.qcow2', internal_version='20240703-1797', os_distro='debian',           |
|                  | os_glance_failed_import='', os_glance_importing_to_stores='', os_hash_algo='sha512',                                                                                                        |
|                  | os_hash_value='7b72b01c729f0df873db2a4998cf9230dd8bcffcb0f012072336717c09803f54811e2e7673a94fc61f26741a75dee8d6fad0aa832ff877ae1e6d86faf9f578f8', os_hidden='False', os_version='10',       |
|                  | owner_specified.openstack.md5='', owner_specified.openstack.object='images/Debian 10 (20240703-1797)', owner_specified.openstack.sha256='', provided_until='none',                          |
|                  | replace_frequency='monthly', stores='rbd', uuid_validity='last-3'                                                                                                                           |
| protected        | False                                                                                                                                                                                       |
| schema           | /v2/schemas/image                                                                                                                                                                           |
| size             | 2147483648                                                                                                                                                                                  |
| status           | active                                                                                                                                                                                      |
| tags             | managed_by_wavestack, os:debian                                                                                                                                                             |
| updated_at       | 2024-08-05T12:44:00Z                                                                                                                                                                        |
| virtual_size     | 2147483648                                                                                                                                                                                  |
| visibility       | public                                                                                                                                                                                      |
+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

#### 3. Download the Original QCOW2 Image

Using the URL found in the image_source property, download the original image:

```bash
curl -LO https://cloud.debian.org/images/cloud/buster/20240703-1797/debian-10-genericcloud-amd64-20240703-1797.qcow2
```

#### 4. Convert the QCOW2 Image to RAW Format

Use qemu-img to convert the downloaded QCOW2 image to RAW format:

```bash
qemu-img convert -f qcow2 -O raw debian-10-genericcloud-amd64-20240703-1797.qcow2 debian-10-genericcloud-amd64-20240703-1797.raw
```

Replace downloaded_image.qcow2 with the name of the downloaded QCOW2 file and original_image.raw with the desired name
for the RAW image.

#### 5. Generate the MD5 Checksum of the RAW Image

Calculate the MD5 checksum of the converted RAW image:

```bash
md5sum debian-10-genericcloud-amd64-20240703-1797.raw
```

This will output `7e261963781e56a3aaf10b100d2b3760` as md5sum

#### 6. Compare Checksums

Compare the checksum obtained from the openstack image show command with the checksum generated in the previous step.

- If the checksums match, the image in your OpenStack environment is identical to the original vendor image, confirming
  its integrity.
- If they differ, the image may have been altered or corrupted, and further investigation is warranted.

{{% alert title="Note" color="warning" %}}
- Ensure you have sufficient disk space to accommodate both the QCOW2 and RAW image files during the conversion process.
- Always download images from official and trusted sources to maintain security and integrity.
{{% /alert %}}




By following these steps, you can effectively verify that the images in your OpenStack environment have not been
tampered with and match the original vendor-provided images.

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
