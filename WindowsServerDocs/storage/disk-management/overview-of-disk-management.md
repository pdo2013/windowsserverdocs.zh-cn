---
title: 磁盘管理概述
description: 磁盘管理是使您可以执行高级的存储的任务，例如初始化一个新的驱动器、 扩展卷，收缩分区，并更改驱动器号的 Windows 中的系统实用程序。
ms.date: 4/2/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2a467c64a3e0ff38b5165b9e001fc2deb2d92148
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819278"
---
# <a name="overview-of-disk-management"></a>磁盘管理概述

> **适用于：** Windows 10、 Windows 8.1、 Windows 7、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

磁盘管理是使您可以执行的高级的存储任务的 Windows 中的系统实用程序。 下面是一些磁盘管理是适用于的内容：

- 若要设置新的驱动器，请参阅[初始化新的驱动器](initialize-new-disks.md)。
- 若要将卷扩展到已不属于同一个驱动器上的卷的空间，请参阅[扩展基本卷](extend-a-basic-volume.md)。
- 若要收缩分区，通常，以便您可以扩展相邻的分区，请参阅[收缩基本卷的](shrink-a-basic-volume.md)。
- 若要更改驱动器号或分配新的驱动器号，请参阅[更改驱动器号](change-a-drive-letter.md)。

![显示具有三个分区的 499 MB 系统分区、 更大的 C 驱动器的 Windows 和用于恢复的另一个 499 MB 分区的典型驱动器的磁盘管理](media/disk-management.png)

> [!TIP]
>  如果遇到错误或某些操作无法在执行这些过程时，看看[磁盘管理疑难解答](troubleshooting-disk-management.md)主题。 如果不起作用-不要慌 ！ 还有大量的信息存储在[Microsoft 社区](https://answers.microsoft.com/en-us/windows)站点-请尝试搜索[文件、 文件夹和存储](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639)部分下，如果仍需帮助，请将发布存在的问题和 Microsoft 或其他成员社区将尝试帮助。 如果您有关于如何改进这些主题的反馈，我们期待收到你的反馈 ！ 只需回答*是此页面有帮助？* 提示，并存在或在本主题底部的公用注释线程保留的任何注释。

以下是一些你可能想要执行的常见任务，但在 Windows 中使用其他工具：

- 若要释放磁盘空间，请参阅[释放 Windows 10 中的驱动器空间](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)。
- 若要对你的驱动器进行碎片整理，请参阅[碎片整理将 Windows 10 电脑](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc)。
- 若要执行多个硬盘驱动器和池它们组合在一起，类似于 RAID，请参阅[存储空间](https://support.microsoft.com/help/12438/windows-10-storage-spaces)。

## <a name="about-those-extra-recovery-partitions"></a>有关这些额外的恢复分区

如果您感兴趣 （我们读取您的意见 ！），Windows 通常包括主驱动器 (通常为 C:\ 上的三个分区驱动器）：

![磁盘 0 显示三个分区的 EFI 系统分区、 Windows 分区和恢复分区](media/windows-partitions.png)

- **EFI 系统分区**-这使用现代 Pc 启动 （引导） 您的 PC 和您的操作系统。
- **Windows 操作系统驱动器 （c:）** -这是 Windows 的安装位置，通常放置您的应用程序和文件的其余部分。
- **恢复分区**-这是存储特殊工具来帮助您恢复 Windows，如果它已启动时遇到问题或遇到其他严重问题。

尽管磁盘管理可能显示为 100%免费 EFI 系统分区和恢复分区，但它反映真实情况。 这些分区通常是与您的 PC 需要正常运行的真正重要文件相当完整的。 最好是只需将其单独执行的操作启动您的 PC 并能帮助您从问题中恢复其作业。

## <a name="see-also"></a>请参阅

- [管理磁盘](manage-disks.md)
- [管理基本卷](manage-basic-volumes.md)
- [磁盘管理疑难解答](troubleshooting-disk-management.md)
- [Windows 10 中的恢复选项](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [到 Windows 10 更新后查找丢失的文件](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [备份和还原文件](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [创建恢复驱动器](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [创建系统还原点](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [找到我的 BitLocker 恢复密钥](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
