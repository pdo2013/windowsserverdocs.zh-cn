---
title: 压缩基本卷
description: 本文介绍如何压缩基本卷
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e54632b78fd67a65b51147323565130881d8d81b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885328"
---
# <a name="shrink-a-basic-volume"></a>压缩基本卷

> **适用于：** Windows 10、 Windows 8.1、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

可以通过以下方式减少主分区和逻辑驱动器所使用的空间：将它们压缩到同一磁盘上相邻的连续空间。 例如，如果需要其他分区却没有多余的磁盘，则可以从卷的末尾处压缩现有分区，来创建新的未分配空间，然后可将这部分空间用于新的分区。 压缩操作可能因存在某些文件类型而被阻止。 有关详细信息，请参阅[其他注意事项](#addcon) 

压缩分区时，会在磁盘上自动重新定位任何普通文件，以创建新的未分配空间。 无需重新格式化磁盘便可压缩分区。

> [!CAUTION]
> 如果分区是包含数据（如数据库文件）的原始分区（即没有文件系统的分区），则压缩分区可能会损坏数据。

## <a name="shrinking-a-basic-volume"></a>压缩基本卷

-   [使用 Windows 界面](#BKMK_WINUI)
-   [使用命令行](#BKMK_CMD)

> [!NOTE]
> 你至少必须是**备份操作员**或**管理员**组的成员才能完成这些步骤。

<a id="BKMK_WINUI"></a>
#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>使用 Windows 界面压缩基本卷

1.  在磁盘管理器中，右键单击想要压缩的基本卷。

2.  单击**压缩卷**。

3.  按照屏幕上的说明进行操作。

<br />

> [!NOTE]
> 你只能压缩没有文件系统或使用 NTFS 文件系统的基本卷。

<a id="BKMK_CMD"></a>
#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>使用命令行压缩基本卷

1.  打开命令提示符并键入 `diskpart`。

2.  在 **DISKPART** 提示符下，键入 `list volume`。 记下你想要压缩的简单卷的编号。

3.  在 **DISKPART** 提示符下，键入 `select volume <volumenumber>`。 选择你想要压缩的简单卷的 *volumenumber*。

4.  在 **DISKPART** 提示符下，键入 `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`。 如果可能，将选择的卷压缩为 *desiredsize* 兆字节 (MB)，如果 *desiredsize* 太大，则压缩为 *minimumsize*。

<br />

| 值 | 描述|
|---|---|
| <p>**list volume**</p> | <p>显示所有磁盘上的基本卷和动态卷的列表。</p>|
| <p>**选择卷**</p> | <p>选择指定的卷（其中 <em>volumenumber</em> 是卷编号），并赋予其焦点。 如果未指定卷，则 **select** 命令会列出具有焦点的当前卷。 你可以通过编号、驱动器号或装入点路径来指定卷。 在基本磁盘上，如果选择卷，则还会赋予相应的分区焦点。</p> |
| <p>**shrink**</p> | <p>压缩具有焦点的卷以创建未分配的空间。 不会丢失任何数据。 如果分区包含不可移动的文件（如页面文件或影子副本存储区域），则卷将压缩到不可移动的文件所在位置。 |
| <p>**desired=** <em>desiredsize</em></p> | <p>要恢复到当前分区中的兆字节空间量。</p> |
| <p>**minimum=** <em>minimumsize</em></p> | <p>要恢复到当前分区中的最小兆字节空间量。 如果未指定所需大小或最小大小，则该命令将回收可能的最大空间。</p> 

<a id="addcon"></a>

## <a name="additional-considerations"></a>其他注意事项

-   压缩分区时，无法自动重新定位某些文件（例如，分页文件或影子副本存储区域），并且当分配的空间减小到不可移动的文件所在位置时，你无法再继续减小。 如果压缩操作失败，请检查应用程序日志中是否有事件 259，此事件标识不可移动的文件。 如果你知道与阻止压缩操作的文件相关联的簇，也可以在命令提示符下使用 **fsutil** 命令（请键入 **fsutil volume querycluster /?** 以了解使用情况）。 如果提供 **querycluster** 参数，则命令输出将标识阻止压缩操作成功的不可移动文件。
在某些情况下，你可以暂时重新定位该文件。 例如，如果你需要进一步压缩分区，则可以使用控制面板将分页文件或存储的影子副本移到另一个磁盘、删除存储的影子副本、压缩该卷，然后将分页文件移回该磁盘。 如果通过动态坏簇重新映射检测到的坏簇数量太高，则无法压缩分区。 如果发生这种情况，应该考虑移动数据并更换磁盘。

-  请不要使用块级复制来传输数据。 这种复制也将复制坏扇区表，并且新磁盘会将相同扇区视为坏扇区，即使它们是正常扇区也不例外。

-   你可以压缩主分区以及原始分区（没有文件系统的分区）或 NTFS 文件系统分区上的逻辑驱动器。

## <a name="see-also"></a>请参阅

-   [管理基本卷](manage-basic-volumes.md)