---
title: 扩展基本卷
description: 本文介绍如何在主驱动器和逻辑驱动器上添加空间来扩展基本卷
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a98bd3553c3223716d70ed4329bd7e265e697b73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402099"
---
# <a name="extend-a-basic-volume"></a>扩展基本卷

> **适用于：** Windows 10、Windows 8.1、Windows Server（半年频道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

通过将现有主分区和逻辑驱动器扩展到相同磁盘上的相邻未分配空间，可以向现有主分区和逻辑驱动器添加更多空间。 若要扩展基本卷，它必须是原始卷（未使用文件系统进行格式化）或使用 NTFS 文件系统进行了格式化。 可以在包含逻辑驱动器的扩展分区中的连续可用空间内扩展该逻辑驱动器。 如果将逻辑驱动器扩展到扩展分区中提供的可用空间之外，则扩展分区将会扩大以包含该逻辑驱动器。

对于逻辑驱动器和引导卷或系统卷，只能将卷扩展到连续空间中，并且仅当磁盘可以升级到动态磁盘时才能扩展。 对于其他卷，可以将卷扩展到非连续空间中，但是系统会提示你将磁盘转换为动态磁盘。

## <a name="extending-a-basic-volume"></a>扩展基本卷

#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>使用 Windows 界面扩展基本卷

1. 在磁盘管理器中，右键单击想要扩展的基本卷。

2. 单击“扩展卷”  。

3. 按照屏幕上的说明进行操作。

#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>使用命令行扩展基本卷

1. 打开命令提示符并键入 `diskpart`。

2. 在 **DISKPART** 提示符下，键入 `list volume`。 记下你要扩展的基本卷。

3. 在 **DISKPART** 提示符下，键入 `select volume <volumenumber>`。 这会选择你想要扩展到同一磁盘上的连续空白空间中的基本卷 *volumenumber*。

4. 在 **DISKPART** 提示符下，键入 `extend [size=<size>]`。 这会以兆字节 (MB) 为单位将选择的卷以指定*大小*进行扩展。

| 值 | 描述 |
| --- | --- |
| **list volume** | 显示所有磁盘上的基本卷和动态卷的列表。 |
| **select volume** | 选择指定的卷（其中 <em>volumenumber</em> 是卷编号），并赋予其焦点。 如果未指定卷，则 **select** 命令会列出具有焦点的当前卷。 可以通过编号、驱动器号或装入点路径指定卷。 在基本磁盘上，如果选择卷，则还会赋予相应的分区焦点。 |
| **extend** | <ul><li>将具有焦点的卷扩展到下一个连续的未分配空间。 对于基本卷，未分配的空间必须与具有焦点的分区在同一个磁盘上，并且必须在该分区之后（扇区偏移量高于该分区）。 动态简单卷或跨区卷可以扩展到任何动态磁盘上的任何空白空间。 可以使用以下命令将现有的卷扩展到新创建的空间中。</li ><li>如果以前使用 NTFS 文件系统格式化了分区，则会自动扩展文件系统以占用较大的分区。 不会丢失任何数据。 如果以前使用除 NTFS 以外的任何其他文件系统格式对分区进行了格式化，则此命令将失效并且不会对分区进行任何更改。</li></ul> |
| **size=** <em>大小</em> | 要添加到当前分区中的兆字节 (MB) 空间量。 如果未指定大小，则磁盘会扩展，以占用所有未分配的连续空间。 |

## <a name="additional-considerations"></a>其他注意事项

-   如果磁盘不包含启动分区或系统分区，则可以将卷扩展到其他非启动盘或非系统磁盘，但磁盘将转换为动态磁盘（如果磁盘可升级）。

## <a name="see-also"></a>另请参阅

-   [命令行语法表示法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)
