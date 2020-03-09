---
title: Access blob storage snapshots in Azure HPC Cache
description: Explains the express snapshots feature and how to access recent backups of Azure Blob storage targets
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: rohogue
---

# View snapshots for blob storage targets

Azure HPC Cache automatically saves storage snapshots for Azure Blob storage targets. Snapshots provide a quick reference point for the contents of the back-end storage container. Snapshots are not a replacement for data backups, and they don't include any information about the state of cached data.

This feature is available for Azure Blob storage targets only, and its configuration can't be changed.

Snapshots are taken every eight hours, at UTC 0:00, 08:00, and 16:00.

Azure HPC Cache stores daily, weekly, and monthly snapshots until they are replaced by new ones. The limits are:

* up to 20 daily snapshots
* up to 8 weekly snapshots
* up to 3 monthly snapshots

Access the snapshots from the `.snapshot` directory in your blob storage target's namespace.
