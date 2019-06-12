---
title: 创建卷的 raid
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca75dd9af441446081cb10743329eb8e42166c0c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434071"
---
# <a name="create-volume-raid"></a>创建卷的 raid

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建 RAID\-5 个卷使用三个或多个指定动态磁盘。  
  
> [!IMPORTANT]  
> 此 DiskPart 命令不可在任何版本的 Windows Vista 中可用。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|           参数           |                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           size\=<n>           | 磁盘空间量，以兆字节为单位\(MB\)，该卷将占用的每个磁盘上。 如果没有给定大小，最大可能 RAID\-将创建 5 个卷。 具有最小可用连续空间的磁盘大小确定为 raid 附加\-从每个磁盘分配 5 个卷和占用的空间相同。 实际的 RAID 中的可用磁盘空间量\-5 卷是磁盘空间总量小于因为某些磁盘空间是需要用于奇偶校验。 |
| disk\=<n>,<n>,<n>\[,<n>,...\] |                                                                                                                                               动态磁盘在其上创建 RAID\-5 个卷。 若要创建 RAID 需要至少三个动态磁盘\-5 个卷。 空间等于**大小\=<n>** 上每个磁盘分配。                                                                                                                                                |
|          align\=<n>           |                                                                                                                   对齐到最接近的对齐边界的所有卷扩展盘区。 通常与硬件 RAID 逻辑单元号一起使用\(LUN\)数组以提高性能。 *n*是千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。                                                                                                                   |
|             noerr             |                                                                                                                                                 仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。                                                                                                                                                  |
  
## <a name="remarks"></a>备注  
  
-   创建卷后，焦点就自动转移到新卷。  
  
## <a name="BKMK_examples"></a>示例  
若要创建的 RAID\-1000 兆字节为单位的大小，使用磁盘 1、 2 和 3，类型的 5 个卷：  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

