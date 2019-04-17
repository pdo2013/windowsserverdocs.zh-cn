---
title: "管理磁盘"
description: "本文介绍了如何管理磁盘"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ae96071733b961fbe65551120894c21c633db83e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="manage-disks"></a>管理磁盘

> **适用于：**Windows 10、Windows 8.1、Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题及其子主题讨论如何使用磁盘管理来管理计算机中的磁盘，并包括有关在不同分区样式之间转换磁盘以及 Windows 如何处理新磁盘的联机状态的信息。

## <a name="converting-disk-types"></a>转换磁盘类型

虽然磁盘管理允许你在各种类型和分区样式之间更改磁盘，但是一些操作是不可逆的（除非你重新格式化驱动器）。 你应该仔细考虑最适合你的应用程序的磁盘类型和分区样式。

下表显示了用于在各种分区样式之间转换磁盘的选项：

| 磁盘类型 | 转换为 MBR  | 转换为 GPT| 转换为动态 |
| ---- | --- | --- |--- |
| <p>主启动记录 (MBR)</p> | <p>--</p> | <p>允许（如果磁盘上没有卷）。</p> | <p>允许，但磁盘可能会变得无法启动。 请参阅“注意”。</p> |
| <p>GUID 分区表 (GPT)</p> | <p>允许（如果磁盘上没有卷）。</p> | <p>--</p>  | <p>允许，但磁盘可能会变得无法启动。 请参阅“注意”。</p> |


> [!NOTE]
> 在多启动方案中，如果已经启动到一个操作系统，并且将包含备用操作系统的基本 MBR 磁盘转换为动态磁盘，则将无法再启动到备用操作系统。

## <a name="online-and-offline-status"></a>联机和脱机状态

磁盘管理会显示磁盘的联机和脱机状态。 

在 Windows 中，默认情况下，所有新发现的磁盘均已联机并具有读写访问权限。 在 Windows Server 中，除非新发现的磁盘在共享总线（如 SCSI、iSCSI、串行附加 SCSI 或光纤通道）上，默认情况下，新发现的磁盘已联机并具有读写访问权限。 首次检测到共享总线上的磁盘时，它们将处于脱机状态。

如果磁盘处于脱机状态，则你必须将其联机，然后才能将其初始化或在其上创建卷。 

-  右键单击磁盘名称，然后选择适当的操作，即可使磁盘联机或脱机。

## <a name="see-also"></a>另请参阅

-   [初始化新磁盘](initialize-new-disks.md)
-   [将磁盘移到另一台计算机](move-disks-to-another-computer.md)
-   [将动态磁盘更改回基本磁盘](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [将主启动记录磁盘更改为 GUID 分区表磁盘](change-an-mbr-disk-into-a-gpt-disk.md)
-   [将 GUID 分区表磁盘更改为主启动记录磁盘](change-a-gpt-disk-into-an-mbr-disk.md)
-   [管理虚拟硬盘](manage-virtual-hard-disks.md)