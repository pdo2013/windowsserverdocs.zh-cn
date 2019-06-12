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
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812407"
---
# <a name="manage-disks"></a>管理磁盘

> **适用于：** Windows 10、 Windows 8.1、 Windows Server （半年频道）、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主题和及其子主题讨论使用磁盘管理来管理的计算机中的磁盘，并包含有关初始化新磁盘，不同的分区样式，以及 Windows 如何处理新磁盘的联机状态之间转换磁盘的信息。

## <a name="online-and-offline-status"></a>联机和脱机状态

磁盘管理显示磁盘是联机 （可用），或脱机。

在 Windows 中，默认情况下，所有新发现的磁盘均已联机并具有读写访问权限。 在 Windows Server 中，除非新发现的磁盘在共享总线（如 SCSI、iSCSI、串行附加 SCSI 或光纤通道）上，默认情况下，新发现的磁盘已联机并具有读写访问权限。 共享总线上的磁盘处于脱机状态第一次检测到它们。

如果磁盘处于脱机状态，则你必须将其联机，然后才能将其初始化或在其上创建卷。

若要使磁盘联机或使其脱机，右键单击磁盘名称，然后选择相应的措施。

## <a name="see-also"></a>请参阅

-   [初始化新磁盘](initialize-new-disks.md)
-   [将磁盘移动到另一台计算机](move-disks-to-another-computer.md)
-   [将动态磁盘更改回基本磁盘](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [转换为 GUID 分区表磁盘更改为主启动记录磁盘](change-an-mbr-disk-into-a-gpt-disk.md)
-   [GUID 分区表磁盘更改为主启动记录磁盘](change-a-gpt-disk-into-an-mbr-disk.md)
-   [管理虚拟硬盘](manage-virtual-hard-disks.md)