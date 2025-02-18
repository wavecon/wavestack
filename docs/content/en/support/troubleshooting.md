---
title: "User Self Troubleshooting Help"
linkTitle: "Troubleshooting"
description: "User Self Troubleshooting Help"
type: "docs"
weight: 1

---

<!-- SPDX-License-Identifier: CC-BY-4.0 -->

<!-- Copyright (C) 2023 Wavecon GmbH -->

This small guide will help you to investigate your virtual resources on wavestack. It provides step-by-step troubleshooting advice to ensure what happened with your virtual resources the software's capabilities.

## Troubleshooting Guide Compute

+ ### Check Compute-State of VMs in Horizon Action-Log or by API
  
  In Horizon you can check the Instance-State-Logs, when you click on "Instances", then select your Instance and there you can click on "Action Log" You can also check this by Command line with:

```
 openstack server event list 515f0424-215a-4aec-b75f-56b4e7667192
```

```
+------------------------------------------+--------------------------------------+----------------+----------------------------+
 | Request ID | Server ID | Action | Start Time |
 +------------------------------------------+--------------------------------------+----------------+----------------------------+
 | req-9c3906cf-0875-4518-98b2-199b3dcc1cfc | 515f0424-215a-4aec-b75f-56b4e7667192 | start | 2024-08-05T08:27:20.000000 |
 | req-fe01eb90-ef3d-425a-bfca-8ce714230dc4 | 515f0424-215a-4aec-b75f-56b4e7667192 | stop | 2024-08-05T07:49:08.000000 |
 | req-0b46174a-c279-4d8d-9103-91008190dcc8 | 515f0424-215a-4aec-b75f-56b4e7667192 | live-migration | 2024-08-05T07:21:15.000000 |
 | req-5f276b06-df11-4d74-99c1-8e2cb12db39c | 515f0424-215a-4aec-b75f-56b4e7667192 | live-migration | 2024-08-05T07:01:02.000000 |
 | req-3cd03a99-a794-4bc6-a031-f616cc679d06 | 515f0424-215a-4aec-b75f-56b4e7667192 | create | 2024-06-18T06:11:39.000000 |
 +------------------------------------------+--------------------------------------+----------------+----------------------------+
```

- ### Console Log for Instances by Horizon or API
  
  In Horizon you can check the consoles expenditure of virtual machines. When you click on "Instances", then select your Instance and there you can Click on "Console Log": You can also check this by Command line with:

```
openstack console log show <instance-id> ...
```

       OK  Started chrony, an NTP client/server
       OK  Reached target System Time Synchronized
       OK  Started Daily apt download activities
       OK  Started Daily apt upgrade and clean activities
       OK  Started Daily dpkg database backup timer
       OK  Started Periodic ext4 Online data Check for All Filesystems
       OK  Started Discard unused blocks once a week
       OK  Started Daily rotation of log files
       OK  Started Daily man-db regeneration
       OK  Started Message of the Day
       OK  Started containerd container runtime
    
    
      webserver-001 login:

### Troubleshooting Guide IAM

 IAM Logs by Webfrontend

- In IAM you see on the right side always an "Audit Log" for all Actions, that happened within your Users and within your Organisation






