---
title: 创建分区逻辑
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4f18048272eda710f7cb53a631ddeda81784a56b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378889"
---
# <a name="create-partition-logical"></a>创建分区逻辑

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在现有扩展分区中创建逻辑分区。 在主启动 @no__t 记录上，只能对 0MBR @ no__t 磁盘使用此命令。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                                                                                                                                                       描述                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size @ no__t-0 @ no__t-1  |                                                                                                              指定逻辑分区的大小（mb） @no__t-配置包括 @ no__t-1，该值必须小于扩展分区。 如果没有给定大小，则分区会一直继续，直到扩展分区中没有可用空间为止。                                                                                                               |
| offset @ no__t-0 @ no__t-1 | 指定在其中创建分区的偏移量（kb \(KB @ no__t）。 偏移量向上舍入，以完全填充所使用的任何柱面大小。 如果未给出偏移量，则会将该分区置于足够大的第一个磁盘范围中以容纳该分区。 分区至少与**size @ no__t-1 @ no__t**指定的数字一样长。 如果为逻辑分区指定大小，则它必须小于扩展分区。 |
| align @ no__t-0 @ no__t-1  |                                                                                     将所有卷或分区区与最接近的对齐边界对齐。 通常与硬件 RAID 逻辑单元号一起使用 \(LUN @ no__t 数组来提高性能。  <n> 是从磁盘开头到最接近的对齐边界的千字节数（kb） \(KB @ no__t。                                                                                      |
|    noerr    |                                                                                                                           仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。                                                                                                                           |
  
## <a name="remarks"></a>备注  
  
-   如果未指定**size**和**offset**参数，则会在扩展分区中可用的最大磁盘区中创建逻辑分区。  
  
-   创建分区之后，焦点会自动转移到新的逻辑分区。  
  
-   必须选择基本 MBR 磁盘，此操作才能成功。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。  
  
## <a name="BKMK_examples"></a>示例  
若要创建大小为 1000 mb 的逻辑分区，请在所选磁盘的扩展分区中键入：  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

