---
title: 合并的虚拟磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 159e307912cb06bc3ea5e452f735e786c0cf9965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847008"
---
# <a name="merge-vdisk"></a>合并的虚拟磁盘

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

合并差异虚拟硬盘 (VHD) 使用其相应的父 VHD。 将修改父 VHD，以包括从差异 VHD 的修改。
> [!NOTE]
> 此命令当前仅适用于 Windows 7 和 Windows Server 2008 R2。
## <a name="syntax"></a>语法
```
merge vdisk depth=<n>
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|depth=<n>|指示父 VHD 文件以将合并在一起的数字。 例如，**深度 = 1**指示将使用一个级别的差异链合并差异虚拟硬盘。|
## <a name="remarks"></a>备注
-   必须选择并为此操作才能成功分离 VHD。 使用**选择的虚拟磁盘**命令选择 VHD，并将焦点移到它。
-   此参数将修改父 VHD。 因此，依赖于父其他差异 Vhd 将不再有效。
## <a name="BKMK_Examples"></a>示例
若要合并其父 VHD 的差异 VHD，请键入：
```
merge vdisk depth=1
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)
-   [压缩的虚拟磁盘](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [分离的虚拟磁盘](detach-vdisk.md)
-   [展开的虚拟磁盘](expand-vdisk.md)
-   [选择的虚拟磁盘](select-vdisk.md)
-   [list_1](list_1.md)
