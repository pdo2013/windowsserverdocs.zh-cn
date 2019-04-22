---
title: 创建扩展分区
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa8d556822bc6caf4277812be818a0cf456e75dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818878"
---
# <a name="create-partition-extended"></a>创建扩展分区

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

具有焦点的磁盘上创建扩展的分区。 你可以使用此命令仅在主启动记录上\(MBR\)磁盘。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|size\=<n>|指定分区的大小以兆字节为单位\(MB\)。 如果没有给定大小，则分区将继续，直到扩展分区中没有更多可用空间。|  
|offset\=<n>|千字节为单位指定的偏移量\(KB\)，在创建分区。 如果没有给定偏移量，将是足够大以保存新分区在磁盘上的可用空间的开始处启动分区。|  
|align\=<n>|对齐到最接近的对齐边界的所有分区扩展盘区。 通常与硬件 RAID 逻辑单元号一起使用\(LUN\)数组以提高性能。 <n> 是的千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。|  
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   创建分区后，焦点就自动转移到新分区。  
  
-   每个磁盘，可以创建只有一个扩展的分区。  
  
-   如果您尝试创建另一个扩展分区内的扩展的分区，此命令将失败。  
  
-   可以创建逻辑驱动器之前，必须创建扩展的分区。  
  
-   若要成功执行此操作，必须选择的基本 MBR 磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。  
  
## <a name="BKMK_examples"></a>示例  
若要创建 1000 兆字节为单位的扩展的分区的大小，请键入：  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  

