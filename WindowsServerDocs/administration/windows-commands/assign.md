---
title: assign
description: 用于**分配**的 Windows 命令主题-将驱动器号或装入点分配给具有焦点的卷。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3e07edcd4ac4ddf5eca1e57da17df441043d15f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382684"
---
# <a name="assign"></a>assign

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

为具有焦点的卷分配驱动器号或装入点。

## <a name="syntax"></a>语法
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parameters

|  参数   |                                                                                                                                 描述                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  字母 = <d>  |                                                                                                             要分配给卷的驱动器号。                                                                                                              |
| 装载 = <path> | 要分配给卷的装入点路径。<br /><br />有关如何使用此命令的说明，请参阅[向驱动器分配装入点文件夹路径](https://go.microsoft.com/fwlink/?LinkId=207059)（<https://go.microsoft.com/fwlink/?LinkId=207059>）。 |
|    noerr     |                                    仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。                                     |

## <a name="remarks"></a>备注
- 如果未指定驱动器号或装入点，则分配下一个可用的驱动器号。 如果驱动器号或装入点已在使用，则会生成错误。
- 通过使用 assign 命令，可以更改与可移动驱动器关联的驱动器号。
- 不能将驱动器号分配给系统卷、启动卷或包含页面文件的卷。 此外，不能将驱动器号分配给原始设备制造商（OEM）分区或除基本数据分区以外的任何 GUID 分区表（gpt）分区。
- 若要成功执行此操作，必须选择卷。 使用 "**选择音量**" 命令选择卷并将焦点移动到该卷。
  ## <a name="BKMK_examples"></a>示例
  若要将字母 E 分配给焦点的卷，请键入：
  ```
  assign letter=e
  ```

