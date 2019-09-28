---
title: 初始化新磁盘
description: 如何使用磁盘管理初始化新磁盘，使其可供使用。 还包括指向解决问题的链接。
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b38fd0b88cea3fcc386959c08af1169302ddaa1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385905"
---
# <a name="initialize-new-disks"></a>初始化新磁盘

> **适用于：** Windows 10、Windows 8.1、Windows 7、Windows Server（半年频道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果向电脑添加新磁盘，但它没有显示在文件资源管理器中，可能需要[添加驱动器号](change-a-drive-letter.md)，或者在使用之前对其进行初始化。 只能初始化尚未格式化的驱动器。 初始化磁盘会擦除磁盘上的所有内容，并为 Windows 使用磁盘做好准备，之后可以对磁盘进行格式化，然后在其中存储文件。

> [!WARNING]
> 如果磁盘上已经有你关注的文件，不要初始化它 - 否则将丢失所有文件。 相反，建议对磁盘进行故障排除以了解是否可以读取文件 - 请参阅[磁盘状态为未初始化或磁盘完全丢失](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing)。

## <a name="to-initialize-new-disks"></a>初始化新磁盘

下面介绍了如何使用磁盘管理初始化新磁盘。 如果想要使用 PowerShell，则改用 [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) cmdlet。

1. 使用管理员权限打开磁盘管理。 
 
    为此，请在任务栏上的搜索框中键入“磁盘管理”  ，选择并按住（或右键单击）“磁盘管理”  ，然后选择“以管理员身份运行”   > “是”  。 如果不能以管理员身份打开它，请键入“计算机管理”  ，然后转到“存储”   > “磁盘管理”  。
1. 在“磁盘管理”中，右键单击想要初始化的磁盘，然后单击“初始化磁盘”  （如下所示）。 如果磁盘被列为“脱机”  ，首先右键单击它并选择“联机”  。

     请注意，某些 USB 驱动器没有初始化选项，它们只是格式化并获取一个[驱动器号](change-a-drive-letter.md)。

    ![显示未格式化磁盘的磁盘管理，并显示“初始化磁盘”快捷菜单](media/uninitialized-disk.PNG)
2. 在“初始化磁盘”  对话框中（如下所示），检查以确保选择了正确磁盘，然后单击“确定”  接受默认分区样式。 如果需要更改分区样式（GPT 或 MBR），请参阅[关于分区样式 - GPT 和 MBR](#about-partition-styles---gpt-and-mbr)。

     磁盘状态暂时变为“正在初始化”  ，然后变为“联机”  状态。 如果出于某种原因初始化失败，请参阅[磁盘状态为未初始化或磁盘完全丢失](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing)。

    ![选择了 GPT 分区样式的“初始化磁盘”对话框](media/initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>关于分区样式 - GPT 和 MBR

磁盘可以划分为多个区块，称为“分区”。 每个分区（即使只有一个分区）都必须具有分区样式 - GPT 或 MBR。 Windows 使用分区样式来了解如何访问磁盘上的数据。

尽管这可能不是很有趣，但重要的是，现在通常不必担心分区样式 - Windows 会自动使用适当的磁盘类型。

大多数电脑对硬盘驱动器和 SSD 使用 GUID 分区表 (GPT) 磁盘类型。 GPT 更可靠，且允许大于 2 TB 的卷。 旧的主启动记录 (MBR) 磁盘类型由 32 位电脑、旧电脑和可移动驱动器（如存储卡）使用。

要将磁盘从 MBR 转换为 GPT，首先必须从磁盘删除所有卷，擦除磁盘上的所有内容，反之亦然。 有关详细信息，请参阅[将 MBR 磁盘转换为 GPT 磁盘](change-an-mbr-disk-into-a-gpt-disk.md)，或[将 GPT 磁盘转换为 MBR 磁盘](change-a-gpt-disk-into-an-mbr-disk.md)。