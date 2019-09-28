---
title: 磁盘管理概述
description: 磁盘管理是 Windows 中的一个系统实用程序，使你能够执行高级存储任务，例如初始化新驱动器、扩展卷、收缩磁盘分区和更改驱动器号。
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 46ed1256ed9039311939f9de12ea46416443be9c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402142"
---
# <a name="overview-of-disk-management"></a>磁盘管理概述

> **适用于：** Windows 10、Windows 8.1、Windows 7、Windows Server（半年频道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

磁盘管理是 Windows 中的一个系统实用程序，使你能够执行高级存储任务。 以下是可从磁盘管理获益的一些事项：

- 要设置新的驱动器，请参阅[初始化新驱动器](initialize-new-disks.md)。
- 要将卷扩展到同一驱动器上尚未成为卷的一部分的空间，请参阅[扩展基本卷](extend-a-basic-volume.md)。
- 要收缩分区，通常可以扩展相邻分区，请参阅[收缩基本卷](shrink-a-basic-volume.md)。
- 要更改驱动器号或分配新的驱动器号，请参阅[更改驱动器号](change-a-drive-letter.md)。

![磁盘管理显示具有三个分区的典型驱动器 - 一个 499 MB 系统分区、一个用于 Windows 的较大 C 驱动器以及另一个用于恢复的 499 MB 分区](media/disk-management.png)

> [!TIP]
>  如果在按照这些步骤操作时出现错误或无法工作，请查看[磁盘管理疑难解答](troubleshooting-disk-management.md)主题。 如果没有用的话，也不要惊慌！ 在 [Microsoft 社区](https://answers.microsoft.com/en-us/windows)网站上有大量信息，试着搜索[文件、文件夹和存储](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639)部分，如果仍然需要帮助，请在那里发布问题，Microsoft 或其他社区成员将尝试提供帮助。 如果你有关于如何改进这些主题的反馈，我们很乐意听取你的意见！ 只需回答“此页面是否有用？”  提示，并在此处或本主题底部的公共评论会话中留下任何评论。

以下是你可能想要执行但在 Windows 中使用其他工具的一些常见任务：

- 要释放磁盘空间，请参阅[释放 Windows 10 中的驱动器空间](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)。
- 要对驱动器进行碎片整理，请参阅[对 Windows 10 电脑进行碎片整理](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc)。
- 要采用多个硬盘驱动器并将它们组合在一起，类似于 RAID，请参阅[存储空间](https://support.microsoft.com/help/12438/windows-10-storage-spaces)。

## <a name="about-those-extra-recovery-partitions"></a>关于这些额外的恢复分区

如果你感到好奇（我们已经阅读了你的评论！），Windows 通常在主驱动器上包含三个分区（通常是 C:\ drive）：

![磁盘 0 显示三个分区 - EFI 系统分区、Windows 分区和恢复分区](media/windows-partitions.png)

- **EFI 系统分区** - 新式电脑用它来启动（引导）你的电脑和操作系统。
- **Windows 操作系统驱动器 (C:)** - 这是安装 Windows 的位置，通常是你放置剩余应用和文件的位置。
- **恢复分区** - 这是存储特殊工具以帮助你在启动出错或遇到其他严重问题时恢复 Windows 的地方。

尽管磁盘管理可能会将 EFI 系统分区和恢复分区显示为 100% 空闲，但事实上不是。 这些分区通常非常满，其中存储着你的电脑正常运行所需的重要文件。 最好让它们独自完成它们的工作，启动你的电脑，帮助你从问题中恢复过来。

## <a name="see-also"></a>另请参阅

- [管理磁盘](manage-disks.md)
- [管理基本卷](manage-basic-volumes.md)
- [磁盘管理疑难解答](troubleshooting-disk-management.md)
- [Windows 10 中的恢复选项](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [查找更新到 Windows 10 后丢失的文件](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [备份和还原文件](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [创建一个恢复驱动器](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [创建系统还原点](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [查找我的 BitLocker 恢复密钥](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
