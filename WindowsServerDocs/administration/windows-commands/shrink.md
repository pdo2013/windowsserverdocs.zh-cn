---
title: shrink
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0e5caa0103018c94671d7441a6d2349ab734be6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370899"
---
# <a name="shrink"></a>shrink

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

Diskpart shrink 命令按指定的数量减小所选卷的大小。 此命令可在卷末尾的未使用空间提供可用磁盘空间。

## <a name="syntax"></a>语法
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>Parameters

|  参数  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired = <n> |                                                     指定所需的空间量（以兆字节（MB）为单位）以减小卷的大小。                                                     |
| 最小值 = <n> |                                                           指定减小卷大小的最小空间量（以 MB 为单位）。                                                           |
|  querymax   |                       返回可减少卷的最大空间量（以 MB 为单位）。 如果应用程序当前正在访问该卷，此值可能会更改。                        |
|   nowait    |                                                       强制该命令在收缩进程仍在进行的同时立即返回。                                                        |
|    noerr    | 仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。 |

## <a name="remarks"></a>备注
- 仅当卷使用 NTFS 文件系统进行格式化，或者它没有文件系统时，才能减小卷的大小。
- 此命令适用于基本卷和简单卷或跨区动态卷。
- 如果未指定所需的数量，则将按最小数量（如果已指定）减少卷。
- 如果未指定最小数量，则卷将按所需量（如果已指定）减少。
- 如果未指定最小数量和所需数量，则将尽可能减少该卷。
- 如果指定了最小数量但可用空间不足，则该命令将失败。
- 若要成功执行此操作，必须选择卷。 使用 "**选择音量**" 命令选择卷并将焦点移动到该卷。
- 此命令不适用于原始设备制造商（OEM）分区、可扩展固件接口（EFI）系统分区或恢复分区。
  ## <a name="BKMK_examples"></a>示例
  若要将所选卷的大小减小到250到 500 mb 之间的最大可能值，请键入：
  ```
  shrink desired=500 minimum=250
  ```
  若要显示可减少的卷的最大 MB 数，请键入：
  ```
  shrink querymax
  ```

[调整大小-分区](https://technet.microsoft.com/library/hh848680.aspx)
