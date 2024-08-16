---
title: "User Self Troubleshooting Help"
linkTitle: "Troubleshooting"
description: "Information on upgrades and maintenance procedures for Wavestack services."
type: "docs"
weight: 1

---

<!-- SPDX-License-Identifier: CC-BY-4.0 -->

<!-- Copyright (C) 2023 Wavecon GmbH -->

#### User Self Troubleshooting Help

This small guide will help you to investigate your virtual resources
on wavestack. it provides step-by-step troubleshooting advice to 
ensure what happened with your virtual resources.
the software's capabilities.

+ Check Compute-State of VMs in Horizon Action-Log or by API
  
  In Horizon you can check the Instance-State-Logs, when you click on "Instances",
  then select your Instance and there you can click on "Action Log"

```
     openstack server event list <instance-id>
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

+ Console Log for Instances by Horizon or API
  
  In Horizon you can check the consoles expenditure of virtual machines. 
  When you click on "Instances", then select your Instance and there you 
  can Click on "Console Log"
  
  You can also check this by Command line with

```
openstack console log show <instance-id> ...
```

      [[0;32m  OK  [0m] Started [0;1;39mchrony, an NTP client/server[0m.
      [[0;32m OK [0m] Reached target [0;1;39mSystem Time Synchronized[0m.
      [[0;32m OK [0m] Started [0;1;39mDaily apt download activities[0m.
      [[0;32m OK [0m] Started [0;1;39mDaily apt upgrade and clean activities[0m.
      [[0;32m OK [0m] Started [0;1;39mDaily dpkg database backup timer[0m.
      [[0;32m OK [0m] Started [0;1;39mPeriodic ext4 Onli���ata Check for All Filesystems[0m.
      [[0;32m OK [0m] Started [0;1;39mDiscard unused blocks once a week[0m.
      [[0;32m OK [0m] Started [0;1;39mDaily rotation of log files[0m.
      [[0;32m OK [0m] Started [0;1;39mDaily man-db regeneration[0m.
      [[0;32m OK [0m] Started [0;1;39mMessage of the Day[0m.
      [[0;32m OK [0m] Started [0;1;39mcontainerd container runtime[0m.
    
    
      webserver-001 login:

- IAM Logs by Webfrontend

- In IAM you see on the right side always an "Audit Log" for all Actions, that happened within your Users and within your Organisation
