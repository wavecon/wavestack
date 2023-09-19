---
title: "Service Level Agreements"
linkTitle: "SLAs"
description: "Information on SLAs of Wavestack services."
type: "docs"
weight: 3
---
<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

in this document you will learn more about our SLA
(Service-Level-Agreement) for our Zones, Regions and Services

We try our best to keep our services available and up-to-date.

Please refer to the following to learn more about our maintenance
procedures:

* https://docs.wavestack.cloud/support/maintenance/

## SLAs

| Service                        | Availability        |
|--------------------------------|---------------------|
| Compute instances (regional)   | 99.95%              |
| Compute instances (zonal)      | 99.5%               |
| OpenStack API                  | 99.9%               |
| RGW Objectstore                | 99.925%             |
| WKE (Waveon Kubernetes Engine) | None (tech preview) |

## Service level criteria

The system availability is the measure of the time at which the
respective system for the user was available as intended.

The system availability refers to the respective monthly
average after deduction of the following:

   1. Downtime during announced maintenance windows
   1. Denial of Service (DoS) attacks
   1. Malfunctions / failures due to force majeure
   1. Recovery times
   1. Time between the occurrence of the unavailability and the
      receipt of the fault report
   1. Regulatory interventions
   1. Unavailability caused by the user, either consciously or
      unconsciously
   1. Unavailability that could have been remedied independently by
      the user

## Service level penalties

| Availablity                                             | Credit (percentage of monthly spend) |
|---------------------------------------------------------|--------------------------------------|
| Less than agreed SLA but equal to or greater than 99.0% | 10%                                  |
| less than 99.0% but equal to or greater than 98%        | 20%                                  |
| Less than 98% but equal to or greater than 97%          | 30%                                  |
| Less than 97%                                           | 40%                                  |

The "percentage of monthly spend" is calculated based on the amount
the user would have normally been charged had there not been a service
outage and is specific to each service.

Service penalties count as credits and only apply to future payments
for Wavestack products within the respective customer space. All other
payments are due immediately.

Unless otherwise agreed, the credits detailed above constitute the
sole and only compensation a user is entitled to in accordance with
the provisions of the SLA.

To receive credit, the user must submit an application within 60 days
of the respective incident.

The client must submit the following information:

1. Date and time of the incident
1. Hostnames and IPs of the affected instances
1. Documentation that documents and confirms the failure(s)

Receipt of a credit is excluded if the application has not been
submitted.
