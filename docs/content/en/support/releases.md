---
title: "Release"
linkTitle: "Releases"
description: "Information on upgrades and maintenance procedures for Wavestack services."
type: "docs"
weight: 1
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

#### Release Notes Wavestack 2023.11 - Antelope - SCS R5

###### Announcement  26.10.2023

From Monday 23.10.2023 to Wednesday 25.10.2023 we upgraded wavestack to SCS  Release 5.

Our upgrade included OSISM 6.0.0 and OpenStack Antelope 2023.1. This upgrade shipped several OpenStack-based changes for further reliability as well as core
 fixes and improvements as they came up on OpenDev in the last few 
month. 

+ `Features`
  
  + Designate
    
    zones can now be shared with other projects, allowing them to create and
    manage recordsets and records in the zone.
  - many Improvements done by upstream work

+ `Fixes` 
  
  + CVE-2023-4623,  CVE-2023-4244, CVE-2023-20569 and many much more ...
  
  + AMD EPYC Live Migration Issue fixed by  Ubuntu LP: 2032164 linux  with last
    
    Ubuntu Linux Kernel

+ `Obsolete`
  
   _ member  _  role not supported anymore 

+ `Notice` OpenStack 2023.1 will maintain until 2024-09-22

[SCS R5](https://github.com/SovereignCloudStack/release-notes/blob/main/Release5.md) 

[OSISM (6.0.0)](https://release.osism.tech/notes/6.0.0.html)

[OpenStack 2023.1 (Antelope)](https://releases.openstack.org/antelope/index.html)

[Ubuntu 22.04.3 LTS (Jammy Jellyfish)](https://discourse.ubuntu.com/t/jammy-jellyfish-point-release-changes/29835/4#Server)
