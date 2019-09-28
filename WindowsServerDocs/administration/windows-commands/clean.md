---
title: clean
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f871ad1d13e06bf0cbb886ba64a52e7a55a9a797
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379321"
---
# <a name="clean"></a>clean

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

Diskpart clean 命令从具有焦点的磁盘中删除所有分区或卷格式。
## <a name="syntax"></a>语法
```
clean [all]
```
## <a name="parameters"></a>Parameters

| 参数 |                                                        描述                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    全部    | 指定磁盘上的每个扇区都设置为零，这会完全删除磁盘上包含的所有数据。 |

## <a name="remarks"></a>备注
- 在主启动记录（MBR）磁盘上，仅覆盖 MBR 分区信息和隐藏扇区信息。
- 在 GUID 分区表（gpt）磁盘上，将覆盖 gpt 分区信息（包括保护 MBR）。 没有隐藏扇区信息。
- 若要成功执行此操作，必须选择磁盘。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。
  ## <a name="BKMK_examples"></a>示例
  若要从所选磁盘中删除所有格式设置，请键入：
  ```
  clean
  ```

[清除磁盘](https://technet.microsoft.com/library/hh848661.aspx)
