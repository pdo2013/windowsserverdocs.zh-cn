---
title: Merge vdisk
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7023f2a6669ea6f6801e25cbfc87c950ab95a3bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373735"
---
# <a name="merge-vdisk"></a>Merge vdisk

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将差异虚拟硬盘（VHD）与其对应的父 VHD 合并在一起。 父 VHD 将被修改以包括来自差异 VHD 的修改。
> [!NOTE]
> 此命令仅适用于 Windows 7 和 Windows Server 2008 R2。
> ## <a name="syntax"></a>语法
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>Parameters
> 
> | 参数 |                                                                                    描述                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | 深度 = <n> | 指示要合并在一起的父 VHD 文件的数目。 例如， **depth = 1**表示差异 VHD 将与差异链的一个级别合并。 |
> 
> ## <a name="remarks"></a>备注
> - 必须选择并分离 VHD，此操作才能成功。 使用 "**选择 vdisk** " 命令选择 VHD 并将焦点移动到该 VHD。
> - 此参数修改父 VHD。 因此，依赖于父项的其他差异 Vhd 将不再有效。
>   ## <a name="BKMK_Examples"></a>示例
>   若要将差异 VHD 与其父 VHD 合并，请键入：
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>其他参考
> - [命令行语法项](command-line-syntax-key.md)
> - [附加 vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [详细信息 vdisk](detail-vdisk.md)
-   [分离 vdisk](detach-vdisk.md)
-   [展开 vdisk](expand-vdisk.md)
-   [选择 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
