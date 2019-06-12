---
title: assign
description: Windows 命令主题**分配**-为带焦点的卷分配驱动器号或装入点。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e7f680fe93e846f5b916cf3210a7ca61f190674
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435301"
---
# <a name="assign"></a>assign

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

为带焦点的卷分配驱动器号或装入点。

## <a name="syntax"></a>语法
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parameters

|  参数   |                                                                                                                                 描述                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  letter=<d>  |                                                                                                             你想要分配给卷驱动器号。                                                                                                              |
| mount=<path> | 你想要分配给卷装入点路径。<br /><br />有关如何使用此命令的说明，请参阅[向驱动器分配装入点文件夹路径](https://go.microsoft.com/fwlink/?LinkId=207059)(<https://go.microsoft.com/fwlink/?LinkId=207059>)。 |
|    noerr     |                                    仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。                                     |

## <a name="remarks"></a>备注
- 如果指定没有驱动器号或装入点，则，分配下一个可用的驱动器号。 如果正在使用的驱动器号或装入点，会生成错误。
- 通过使用分配命令，可以更改与可移动驱动器相关联的驱动器号。
- 不能将驱动器号分配给系统卷、 启动卷或包含页面文件的卷。 此外，不能将驱动器号分配给原始设备制造商 (OEM) 分区或除基本数据分区以外的任何 GUID 分区表 (gpt) 分区。
- 成功执行此操作，必须选择一个卷。 使用**选择卷**命令以选择一个卷并将焦点移到它。
  ## <a name="BKMK_examples"></a>示例
  若要将字母 E 分配到具有焦点的卷，请键入：
  ```
  assign letter=e
  ```

