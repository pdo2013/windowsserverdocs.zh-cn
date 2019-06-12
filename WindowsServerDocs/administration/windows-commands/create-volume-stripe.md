---
title: 创建带区卷
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ed731df4613e215fb4d0954a5b8424035b1166
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434007"
---
# <a name="create-volume-stripe"></a>创建带区卷

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建使用两个或多个指定的动态磁盘的条带化的卷。  
  
> [!IMPORTANT]  
> 对于 Windows Vista，此 DiskPart 命令才可用的 Windows Vista Ultimate、 Windows Vista Enterprise 和 Windows Vista Business 版本中。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|         参数         |                                                                                                                            描述                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         size\=<n>         |             磁盘空间量，以兆字节为单位\(MB\)，该卷将占用的每个磁盘上。 如果没有给定大小，新建卷将占用的最小磁盘以及每个后续的磁盘上相同大小的空间上的剩余可用空间。             |
| disk\=<n>,<n>\[,<n>,...\] |                                  在其创建条带化的卷的动态磁盘。 需要至少两个动态磁盘来创建带区的卷。 空间等于**大小\=<n>** 上每个磁盘分配。                                   |
|        align\=<n>         | 对齐到最接近的对齐边界的所有卷扩展盘区。 通常与硬件 RAID 逻辑单元号一起使用\(LUN\)数组以提高性能。 *n*是千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。 |
|           noerr           |                               仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。                                |
  
## <a name="remarks"></a>备注  
  
-   创建卷后，焦点就自动转移到新卷。  
  
## <a name="BKMK_examples"></a>示例  
若要创建 1000 兆字节为单位的条带化的卷的大小，磁盘 1 和 2 中，键入：  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

