---
title: Common questions about the Azure Migrate appliance
description: Get answers to common questions about the Azure Migrate appliance
ms.topic: conceptual
ms.date: 11/21/2019
---

# Azure Migrate appliance: Common questions

This article answers common questions about the Azure Migrate appliance. If you have further queries after reading this article, post them on the [Azure Migrate forum](https://aka.ms/AzureMigrateForum). If you have other questions, review these articles:

- [General questions](resources-faq.md) about Azure Migrate.
- [Questions](common-questions-discovery-assessment.md) about the discovery, assessment, and dependency visualization.


## What is the Azure Migrate appliance?

The Azure Migrate appliance is a lightweight appliance used by the Azure Migrate: Server Assessment tool to discover and assess on-premises servers, and used by the Azure Migrate: Server Migration tool for agentless migration of on-premises VMware VMs. 

The appliance is deployed on-premises as a VM or physical machine. The appliance discovers on-premises machines, and continually sends machine metadata and performance data to Azure Migrate. Appliance discovery is agentless. Nothing needs to be installed on discovered machines. [Learn more](migrate-appliance.md) about the appliance.

## How does the appliance connect to Azure?

The connection can be over the internet, or use Azure ExpressRoute with public/Microsoft peering.

## Does appliance analysis impact performance?

The Azure Migrate appliance profiles on-premises machines continuously to measure VM performance data. This profiling has almost no performance impact on the Hyper-V/ESXi hosts or on VMware vCenter Server.

### Can I harden the appliance VM?

When you create the appliance VM using the downloaded template, you can add components (for example an antivirus) to the template, as long as you leave the communication and firewall rules required for the Azure Migrate appliance as is.


## What network connectivity is needed?

Review the following:
- Appliance VMware assessment: [URL](migrate-support-matrix-vmware.md#assessment-url-access-requirements) and [port](migrate-support-matrix-vmware.md#assessment-port-requirements) access requirements.
- Appliance agentless VMware migration: [URL](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) and [port](migrate-support-matrix-vmware.md#agentless-migration-port-requirements) access requirements.
- Appliance Hyper-V assessment: [URL](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) and [port](migrate-support-matrix-hyper-v.md#assessment-port-requirements) access requirements.


## What data does the appliance collect?

Review the collected data:

- VMware VM [performance data](migrate-appliance.md#collected-performance-data-vmware) and [metadata](migrate-appliance.md#collected-metadata-vmware).
- Hyper-V VM [performance data](migrate-appliance.md#collected-performance-data-hyper-v) and [metadata](migrate-appliance.md#collected-metadata-hyper-v).


## How is data stored?

Data collected by the Azure Migrate appliance is stored in the Azure location that you choose when you created the migration project. 

- The data is securely stored in a Microsoft subscription, and is deleted when you delete the Azure Migrate project.
- If you use [dependency visualization](concepts-dependency-visualization.md), the data collected is stored in the United States, in a Log Analytics workspace created in the Azure subscription. This data is deleted when you delete the Log Analytics workspace in your subscription.

## How much data is uploaded in continuous profiling?

The volume of data sent to Azure Migrate depends on a number of parameters. To give you a sense of the volume, an Azure Migrate project with 10 machines (each with one disk and one NIC) sends around 50 MB per day. This value is approximate, and changes based on the number of data points for the NICs and disks. The increase in data sent is non-linear if the number of machines, NICs, or disks increases.

## Is data encrypted at-rest/in-transit?

Yes, for both.

- Metadata is securely sent to the Azure Migrate service over the internet, via HTTPS.
- Metadata is stored in an [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) database, and in [Azure Blob storage](../storage/common/storage-service-encryption.md), in a Microsoft subscription. The metadata is encrypted at-rest.
- The data for dependency analysis is also encrypted in-transit (secure HTTPS). It's stored in a Log Analytics workspace in your subscription. It's also encrypted at-rest.

## How does the appliance connect to vCenter Server?

1. The appliance connects to vCenter Server (port 443), using the credentials you provided when you set up the appliance.
2. The appliance uses VMware PowerCLI to query vCenter Server, to collect metadata about the VMs managed by vCenter Server.
3. The appliance collects configuration data about VMs (cores, memory, disks, NICs) and the performance history of each VM for the past month.
4. The collected metadata is ent to Azure Migrate: Server Assessment (over the internet via HTTPS) for assessment.

## Can I connect the appliance to multiple vCenter Servers?

No. There's a one-to-one mapping between an appliance and vCenter Server. To discover VMs on multiple vCenter Server instances, you need to deploy multiple appliances.

### How many VMs can I discover with an appliance?

You can discover up to 10,000 VMware VMs and up to 5,000 Hyper-V VMs with a single appliance. If you have more machines in your on-premises environment, read about scaling [Hyper-V](scale-hyper-v-assessment.md) and [VMware](scale-vmware-assessment.md) assessment.

### Can I delete an appliance?

Currently deletion of appliance from the project isn't supported.

- The only way to delete the appliance is to delete the resource group that contains the Azure Migrate project associated with the appliance.
- However, deleting the resource group will also delete other registered appliances, the discovered inventory, assessments and all other Azure components associated with the project in the resource group.


## Next steps
Read the [Azure Migrate overview](migrate-services-overview.md).
