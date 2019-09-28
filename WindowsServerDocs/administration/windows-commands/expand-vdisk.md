---
title: 展开 vdisk
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8fb5d7b58b7aa4bc9557086c73020820cfa04284
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377291"
---
# <a name="expand-vdisk"></a>展开 vdisk

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将虚拟硬盘（VHD）扩展到指定的大小。
> [!NOTE]
> 此命令仅适用于 Windows 7 和 Windows Server 2008 R2。
> ## <a name="syntax"></a>语法
> ```
> expand vdisk maximum=<n>
> ```
> ## <a name="parameters"></a>Parameters
> 
> |  参数  |                      描述                      |
> |-------------|-------------------------------------------------------|
> | 最大值 = <n> | 指定 VHD 的新大小（以兆字节（MB）为单位）。 |
> 
> ## <a name="remarks"></a>备注
> - 必须选择并分离 VHD，此操作才能成功。 使用 "**选择 vdisk** " 命令选择卷并将焦点移动到该卷。
>   ## <a name="BKMK_Examples"></a>示例
>   若要将所选 VHD 扩展到 20 GB，请键入：
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>其他参考
> - [命令行语法项](command-line-syntax-key.md)
> - [附加 vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [分离 vdisk](detach-vdisk.md)
-   [详细信息 vdisk](detail-vdisk.md)
-   [Merge vdisk](merge-vdisk.md)
-   [选择 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
