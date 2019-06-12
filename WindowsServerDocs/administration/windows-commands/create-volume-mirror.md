---
title: 创建卷镜像
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72d80fdf6eca1262a858cbe2a98ed8c9c421bff6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434078"
---
# <a name="create-volume-mirror"></a>创建卷镜像

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用两个指定的动态磁盘来创建卷镜像。  
  
> [!NOTE]  
> 此命令才在 Windows 7 和 Windows Server 2008 R2 中可用。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|         参数         |                                                                                                                                     描述                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         size\=<n>         |                 指定的磁盘空间量，以兆字节为单位\(MB\)，该卷将占用的每个磁盘上。 如果没有给定大小，新建卷将占用的最小磁盘以及每个后续的磁盘上相同大小的空间上的剩余可用空间。                 |
| disk\=<n>,<n>\[,<n>,...\] |                       指定在其创建镜像卷的动态磁盘。 需要两个动态磁盘来创建镜像卷。 使用指定的大小相等的空间量**大小**参数分配每个磁盘上。                        |
|        align\=<n>         | 对齐到最接近的对齐边界的所有卷扩展盘区。 此参数通常用于硬件 RAID 逻辑单元号\(LUN\)数组以提高性能。 *n*是千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。 |
|           noerr           |                                        用于仅编写脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出并出现错误。                                         |
  
## <a name="remarks"></a>备注  
  
-   创建卷后，焦点就自动转移到新卷。  
  
## <a name="BKMK_examples"></a>示例  
若要创建 1000 兆字节为单位的镜像的卷的大小，磁盘 1 和 2 中，键入：  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

