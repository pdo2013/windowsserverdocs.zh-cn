---
title: 创建分区逻辑
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b347581773a086d525bb005edeca2efa31e1848
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886048"
---
# <a name="create-partition-logical"></a>创建分区逻辑

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在现有的扩展分区中创建的逻辑分区。 在主启动记录上只能使用此命令\(MBR\)磁盘。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|size\=<n>|指定的逻辑分区的大小以兆字节为单位\(MB\)，它必须是小于扩展分区。 如果没有给定大小，则分区将继续，直到扩展分区中没有更多可用空间。|  
|offset\=<n>|千字节为单位指定的偏移量\(KB\)，在创建分区。 偏移量将向上舍入以完全填充使用柱面大小。 如果没有给定偏移量，然后分区放置在大小足以容纳它的第一个磁盘区域。 分区是不少于以字节为单位指定的数量作为**大小\=<n>**。 如果指定的逻辑分区的大小，它必须小于扩展分区。|  
|align\=<n>|将所有卷或分区范围为最接近的对齐边界都对齐。 通常与硬件 RAID 逻辑单元号一起使用\(LUN\)数组以提高性能。  <n> 是的千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。|  
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   如果**大小**并**偏移量**未指定参数，在扩展分区中可用的最大磁盘区域中创建逻辑分区。  
  
-   创建分区后，焦点就自动转移到新的逻辑分区。  
  
-   若要成功执行此操作，必须选择的基本 MBR 磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。  
  
## <a name="BKMK_examples"></a>示例  
若要创建 1000 兆字节为单位的逻辑分区的大小，在所选磁盘的扩展分区中键入：  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  

