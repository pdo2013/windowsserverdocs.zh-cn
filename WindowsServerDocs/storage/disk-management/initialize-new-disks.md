---
title: 初始化新磁盘
description: 如何初始化新磁盘使用磁盘管理，让用户可供使用。 此外包括指向故障排除问题。
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 61c8eb2321ee2a282345aba01c6d04a128f0448c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819388"
---
# <a name="initialize-new-disks"></a>初始化新磁盘

> **适用于：** Windows 10、 Windows 8.1、 Windows 7、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

如果将新磁盘添加到您的 PC，并且它不会显示在文件资源管理器，您可能需要[添加驱动器号](change-a-drive-letter.md)，或对其进行初始化，然后再使用它。 仅可以初始化尚不支持格式的驱动器。 初始化磁盘会删除包含的所有信息，使其准备好以供 Windows 之后, 可以将其格式化，然后将其上的文件存储。

> [!WARNING]
> 如果已在磁盘上它你关注的信息的文件，不将其初始化-将丢失所有文件。 相反，我们建议进行故障排除该磁盘要了解是否可以读取文件-请参阅[磁盘的状态为未初始化或磁盘丢失完全](troubleshooting-disk-management.md#disk-not-initialized)。

## <a name="to-initialize-new-disks"></a>初始化新磁盘

下面介绍了如何初始化新的磁盘使用磁盘管理。 如果想要使用 PowerShell，使用[初始化磁盘](https://docs.microsoft.com/powershell/module/storage/initialize-disk)cmdlet 相反。

1. 使用管理员权限打开磁盘管理。 <br>为此，请在任务栏上的搜索框中，键入**磁盘管理**、 选择和保存 （或右键单击）**磁盘管理**，然后选择**以管理员身份运行** > **是**。 如果您不能以管理员身份打开它，请键入**计算机管理**相反，然后转到**存储** > **磁盘管理**。
1. 在磁盘管理，右键单击的磁盘需要进行初始化，然后依次**初始化磁盘**（如下所示）。 如果磁盘被列为*脱机*，首先右键单击它并选择**联机**。<br>请注意，某些 USB 驱动器没有要初始化的选项，它们只是获取格式和一个[驱动器号](change-a-drive-letter.md)。

    ![使用显示的初始化磁盘快捷方式菜单显示无格式的磁盘的磁盘管理](media\uninitialized-disk.PNG)
2. 在中**初始化磁盘**（如下所示） 的对话框中，检查以确保选择正确的磁盘，然后单击**确定**以接受默认分区形式。 如果你需要更改分区样式 （GPT 或 MBR），请参阅[有关分区形式的 GPT 和 MBR](#about-partition-styles-GPT-and-MBR)。<br>磁盘状态简要变为**正在初始化**然后**联机**状态。 如果出于某种原因初始化失败，请参阅[磁盘的状态为未初始化或磁盘丢失完全](troubleshooting-disk-management.md#disk-not-initialized)。

    ![与所选的 GPT 分区形式初始化磁盘对话框](media\initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>有关分区样式的 GPT 和 MBR

磁盘可以划分为多调用分区的多个块区。 每个分区中-即使只有一个-必须具有分区形式的 GPT 或 MBR。 Windows 使用的分区形式来了解如何访问磁盘上的数据。

因为这可能不是为着迷，底线是，如今，你通常不必担心如何分区形式-Windows 将自动使用相应的磁盘类型。

大多数 Pc 硬盘驱动器和 Ssd 中用于 GUID 分区表 (GPT) 磁盘类型。 GPT 更加可靠和允许大于 2 TB 的卷。 32 位电脑、 旧 Pc 和可移动驱动器，比如内存卡使用较旧的主启动记录 (MBR) 磁盘类型。

若要将磁盘从 MBR 为 GPT，反之亦然，首先需要从磁盘中删除所有卷擦除磁盘上的所有内容。 有关详细信息，请参阅[MBR 磁盘转换为 GPT 磁盘](change-an-mbr-disk-into-a-gpt-disk.md)，或[GPT 磁盘转换为 MBR 磁盘](change-a-gpt-disk-into-an-mbr-disk.md)。