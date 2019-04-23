---
title: 压缩的虚拟磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba04bdc8c469ef80853b6092b8745defa1f3f19e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877298"
---
# <a name="compact-vdisk"></a>压缩的虚拟磁盘

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

减少了动态扩充虚拟硬盘 (VHD) 文件的物理大小。 此参数很有用，因为动态扩展 Vhd 大小增加，添加文件，但它们不会自动降低了大小时删除文件。
> [!NOTE]
> 此命令当前仅适用于 Windows 7 和 Windows Server 2008 R2。
## <a name="syntax"></a>语法
```
compact vdisk
```
## <a name="remarks"></a>备注
-   若要成功执行此操作，必须选择动态扩展 VHD。 使用**选择的虚拟磁盘**命令选择 VHD，并将焦点移到它。
-   您可以只是压缩动态扩展 Vhd 分离或附加为只读的。
## <a name="BKMK_Examples"></a>示例
若要压缩动态扩展 VHD，请键入：
```
compact vdisk
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [分离的虚拟磁盘](detach-vdisk.md)
-   [展开的虚拟磁盘](expand-vdisk.md)
-   [合并的虚拟磁盘](merge-vdisk.md)
-   [选择的虚拟磁盘](select-vdisk.md)
-   [list_1](list_1.md)
