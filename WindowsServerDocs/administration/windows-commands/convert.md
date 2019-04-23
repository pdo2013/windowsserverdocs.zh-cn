---
title: convert
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9fcb7b2190c3359b78145a2ef79a28e30639a89c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873158"
---
# <a name="convert"></a>convert



将文件分配表 (FAT) 和 FAT32 卷为 NTFS 文件系统，使现有文件和目录保持不变。 转换为 NTFS 文件系统的卷不能转换回 FAT 或 FAT32。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<卷 >|指定 （后跟一个冒号） 的驱动器号，装入点或要将转换为 NTFS 的卷名。|
|/fs:ntfs|必需。 将卷转换为 NTFS。|
|/v|运行**转换**在详细模式下，这将显示所有消息在转换过程。|
|/cvtarea:\<FileName>|指定主文件表 (MFT) 和其他 NTFS 元数据文件将写入到现有的、 连续的占位符文件。 此文件必须是要转换的文件系统的根目录中。 利用 **/cvtarea**参数可能会导致不太零碎的文件系统转换后。 为获得最佳结果，此文件的大小应为 1 KB 的文件和目录数的乘积在文件系统中，尽管**转换**实用程序接受任何大小的文件。</br>重要提示：必须使用创建占位符文件**fsutil 文件 createnew**命令在运行前先**转换**。 **将转换**不会为你创建此文件。 **将转换**覆盖此文件具有 NTFS 元数据。 转换后，会释放此文件中的任何未使用的空间。|
|/nosecurity|指定在转换后的文件和目录的安全设置允许所有用户的访问。|
|/x|如有必要之前它将转换, 就会卸载该卷。 针对卷的任何打开句柄将不再有效。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如果**转换**无法锁定驱动器 （例如，驱动器是系统卷或当前驱动器），你可以转换在下次重新启动计算机的驱动器的选项。 如果您无法重新启动计算机后，立即以完成转换，计划的时间来重新启动计算机并留出额外时间才能完成转换过程。
-   为卷从 FAT 或 FAT32 转换为 NTFS:

    由于现有磁盘使用情况，在 MFT 创建在不同的位置，不是在最初使用 NTFS 格式化的卷，因此卷性能可能不是局限于最初使用 NTFS 格式化的卷上。 为了获得最佳性能，请考虑重新创建这些卷，并设置其格式与 NTFS 文件系统。

    从 FAT 或 FAT32 的卷转换为 NTFS 使文件保持不变，但该卷可能缺少某些相比最初使用 NTFS 格式化的卷的性能优势。 例如，可能会变成 MFT 碎片在转换后的卷上。 此外，在转换后的启动卷**转换**适用相同的默认安全性的 Windows 安装期间应用。

## <a name="BKMK_examples"></a>示例

若要将驱动器 E 上的卷转换为 NTFS 和转换过程中显示的所有消息，请键入：
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)