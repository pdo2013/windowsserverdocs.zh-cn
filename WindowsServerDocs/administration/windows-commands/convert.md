---
title: convert
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1e22f67768bbe2f37f3627ca69b162cae96f2d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379076"
---
# <a name="convert"></a>convert



将文件分配表（FAT）和 FAT32 卷转换为 NTFS 文件系统，从而使现有文件和目录保持不变。 转换为 NTFS 文件系统的卷无法转换回 FAT 或 FAT32。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Volume >|指定驱动器号（后跟冒号）、装入点或卷名，以将其转换为 NTFS。|
|/fs： ntfs|必需。 将卷转换为 NTFS。|
|/v|在详细模式下运行**转换**，这会在转换过程中显示所有消息。|
|/cvtarea： \<FileName >|指定将主文件表（MFT）和其他 NTFS 元数据文件写入现有的连续占位符文件。 此文件必须位于要转换的文件系统的根目录中。 使用 **/cvtarea**参数会导致转换后的文件系统碎片不大。 为获得最佳结果，此文件的大小应与文件系统中的文件和目录的数量相乘，不过**转换**实用工具接受任意大小的文件。</br>重要提示：在运行 "**转换**" 之前，必须使用 " **fsutil file createnew** " 命令创建占位符文件。 **转换**不会为你创建此文件。 **Convert**用 NTFS 元数据覆盖此文件。 转换后，此文件中的任何未使用的空间都将释放。|
|/nosecurity|指定已转换的文件和目录的安全设置允许所有用户访问。|
|/x|在转换之前，如有必要，请卸载该卷。 针对卷的任何打开句柄将不再有效。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如果**convert**无法锁定驱动器（例如，驱动器是系统卷或当前驱动器），则可以选择在下次重新启动计算机时转换驱动器。 如果您无法立即重新启动计算机以完成转换，请计划一段时间来重新启动计算机，并留出额外的时间来完成转换过程。
-   对于从 FAT 或 FAT32 转换为 NTFS 的卷：

    由于现有的磁盘使用情况，MFT 在不同于最初使用 NTFS 进行格式化的卷上创建，因此，容量性能可能不如最初用 NTFS 格式化的卷上的好。 为了获得最佳性能，请考虑重新创建这些卷，并将其格式化为 NTFS 文件系统。

    从 FAT 或 FAT32 到 NTFS 的卷转换会使文件保持不变，但与最初用 NTFS 格式化的卷相比，卷可能会缺少一些性能优势。 例如，MFT 可能会在转换后的卷上出现碎片。 此外，转换后的启动卷上的**转换**应用 Windows 安装程序期间应用的默认安全设置。

## <a name="BKMK_examples"></a>示例

要在转换过程中将驱动器 E 上的卷转换为 NTFS 并显示所有消息，请键入：
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)