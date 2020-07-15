---
# required metadata

title: Commerce Data Exchange best practices
description: This topic is intended for people who implement functionality that is related to data synchronization (Commerce Data Exchange, often known as CDX) in a Commerce environment. It gives best practices advice for implementations.
author: jashanno
manager: AnnBe
ms.date: 05/01/2020
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

ms.search.form: RetailTerminalTable, RetailDevice
# ROBOTS: 
audience: IT Pro
# ms.devlang: 
ms.reviewer: sericks
ms.search.scope: Core, Operations, Retail
# ms.tgt_pltfrm: 
ms.custom: 
ms.assetid: 
ms.search.region: global
ms.search.industry: Retail
ms.author: jashanno
ms.search.validFrom: 2020-06-01
ms.dyn365.ops.version: Retail July 2017 update
---

# Commerce Data Exchange best practices
This topic is intended for people who implement functionality that is related to data synchronization (Commerce Data Exchange, often known as CDX) in a Commerce environment. It gives best practices advice for implementations.

## Overview
Data configuration and synchronization is crucial to a proper implementation.  Regardless of business needs, IT infrastructure, and overall preparedness, if data is not being correctly synchronized, the entire environment is effectively useless.  As such, it is top priority to understand what is required to configure, generate, synchronize, and verify data synchronization across a full implementation. From Headquarters, through the Commerce Scale Unit, to the brick and mortar stores utilizing Modern POS and other in-store components.

Before going through this document, it is important to know the concepts of a channel (Store), registers and devices, and the concept of the offline database as a part of Modern POS.  As such, it is recommended to review some of the resources at the bottom of this document such as the Devices implementation guide and the Commerce architecture documents. 


### Valuable configurations
Navigate to **Retail and Commerce > Headquarters setup > Parameters > Commerce scheduler parameters** and set the **Try count** value to **3**.  Having this value too high could potentially cause a download session to fail during high usage times.

**Retail and Commerce > Channel setup > POS setup > POS profiles > Functionality profile**, under the **Functions** sub-heading, and set the **Days transactions exist** value to the same or near the value of the return policy (For example, if the return policy states an item may be returned in 30 days or less, then set this value to 30, 31, or 60).

### Performance impacting practices
Take note that a new Modern POS offline database (Added to the relevant Channel database group) inherits all existing download sessions since the last full database sync occurred.  As an example, let's assume creation of 100 new Modern POS with offline terminals occurs and a full sync has not occurred in two months.  Let's assume only five different scheduler jobs have actual changes every 20 minutes. This generates immediately a potential of 2,000,000 (Two million) download sessions to be applied, regardless of whether the newly created terminals are activated and capable of applying this data.  This sort of exceptional data generation is large and performance impacting at the best possible times and crippling to the environment performance at the worst (Busiest) times.  As such, it is highly recommended to either have a dummy Channel database group (Which is not associated to any distribution schedule job) to assign to the newly generated terminals or a special **Offline profile** where the configuration **Pause offline synchronization** is set to value **Yes**.  This will allow data generation to occur when required and when the system is most available to do so (Potentially pausing multiple times, as required).



## Resources
| Links |
|---------------------------------------------------|
| [Commerce Data Exchange troubleshooting](dev-itpro/CDX-Troubleshooting.md) |
| [Commerce Data Exchange implementation guidance](implementation-considerations-CDX.md) |
| [Commerce Architecture](commerce-architecture.md) |
| [Select an in-store topology](retail-in-store-topology.md) |
| [Device management implementation guidance](implementation-considerations-devices.md) |
| Link to MPOS installer* LINK PENDING |
| Link to CSU (Self-hosted) installer* LINK PENDING |
