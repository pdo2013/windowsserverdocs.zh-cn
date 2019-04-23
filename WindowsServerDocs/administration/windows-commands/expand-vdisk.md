---
title: 展开的虚拟磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf8fd0b05ca5baeeee4fadd670adb3169130d04e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837398"
---
# <a name="expand-vdisk"></a>展开的虚拟磁盘

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将虚拟硬盘 (VHD) 扩展到您指定的大小。
> [!NOTE]
> 此命令当前仅适用于 Windows 7 和 Windows Server 2008 R2。
## <a name="syntax"></a>语法
```
expand vdisk maximum=<n>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|maximum=<n>|指定兆字节 (MB) 中的 VHD 的新大小。|
## <a name="remarks"></a>备注
-   必须选择并为此操作才能成功分离 VHD。 使用**选择的虚拟磁盘**命令以选择一个卷并将焦点移到它。
## <a name="BKMK_Examples"></a>示例
若要为 20 GB 展开所选的 VHD，请键入：
```
expand vdisk maximum=20000
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)
-   [压缩的虚拟磁盘](compact-vdisk.md)

-   [分离的虚拟磁盘](detach-vdisk.md)
-   [detail vdisk](detail-vdisk.md)
-   [合并的虚拟磁盘](merge-vdisk.md)
-   [选择的虚拟磁盘](select-vdisk.md)
-   [list_1](list_1.md)
