---
title: shrink
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 09cc9ce7beebc04a0a45cd93c2b0da25c5a1a49e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441267"
---
# <a name="shrink"></a>shrink

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Diskpart 收缩命令指定的数量减少所选卷的大小。 此命令使可从该卷末尾的未使用空间的可用磁盘空间。

## <a name="syntax"></a>语法
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>Parameters

|  参数  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired=<n> |                                                     通过指定所需的兆字节 (MB)，以减少卷的大小的空间量。                                                     |
| minimum=<n> |                                                           以 mb 为单位来减小卷的大小指定最小空间量。                                                           |
|  querymax   |                       以 mb 为单位该卷可以减少返回的最大空间量。 如果应用程序当前正在访问该卷，可能会更改此值。                        |
|   nowait    |                                                       强制命令在收缩过程仍正在进行时立即返回。                                                        |
|    noerr    | 仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。 |

## <a name="remarks"></a>备注
- 仅当使用 NTFS 文件系统格式化或不具有文件系统，可以减少卷的大小。
- 此命令可以在基本卷或卷或跨区动态卷上运行。
- 如果未指定所需的量，该卷将减少的最小数量 （如果指定）。
- 如果未指定最小的量，该卷将减少所需的量 （如果指定）。
- 如果最小数量和所需的量均未指定，该卷将减少通过尽可能多地。
- 如果指定最小数量，但没有足够可用空间，则该命令将失败。
- 成功执行此操作，必须选择一个卷。 使用**选择卷**命令以选择一个卷并将焦点移到它。
- 此命令不会运行原始设备制造商 (OEM) 分区、 可扩展固件接口 (EFI) 系统分区或恢复分区上。
  ## <a name="BKMK_examples"></a>示例
  若要减少之间 250 到 500 兆字节为单位的最大可能数量的所选卷的大小，请键入：
  ```
  shrink desired=500 minimum=250
  ```
  若要显示的最大卷可以通过减少的 MB 数，请键入：
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
