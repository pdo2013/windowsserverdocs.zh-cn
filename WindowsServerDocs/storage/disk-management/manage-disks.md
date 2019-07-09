---
title: 管理磁盘
description: 本文介绍了如何管理磁盘
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 344dd363e970b195abe20fcb69e741c450fc7a21
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812407"
---
# <a name="manage-disks"></a>管理磁盘

> **适用于：** Windows 10、Windows 8.1、Windows Server（半年频道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题及其子主题讨论如何使用磁盘管理来管理计算机中的磁盘，并包括有关初始化新磁盘、在不同分区样式之间转换磁盘以及 Windows 如何处理新磁盘的联机状态的信息。

## <a name="online-and-offline-status"></a>联机和脱机状态

磁盘管理显示磁盘是联机（可用）还是脱机状态。

在 Windows 中，默认情况下，所有新发现的磁盘均已联机并具有读写访问权限。 在 Windows Server 中，默认情况下，新发现的磁盘已联机并具有读写访问权限，在共享总线（如 SCSI、iSCSI、串行附加 SCSI 或光纤通道）上的除外。 首次检测到共享总线上的磁盘时，它们将处于脱机状态。

如果磁盘处于脱机状态，则必须将其联机，然后才能将其初始化或在其上创建卷。

右键单击磁盘名称，然后选择适当的操作，即可使磁盘联机或脱机。

## <a name="see-also"></a>另请参阅

-   [初始化新磁盘](initialize-new-disks.md)
-   [将磁盘移到另一台计算机](move-disks-to-another-computer.md)
-   [将动态磁盘更改回基本磁盘](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [将主启动记录磁盘更改为 GUID 分区表磁盘](change-an-mbr-disk-into-a-gpt-disk.md)
-   [将 GUID 分区表磁盘更改为主启动记录磁盘](change-a-gpt-disk-into-an-mbr-disk.md)
-   [管理虚拟硬盘](manage-virtual-hard-disks.md)