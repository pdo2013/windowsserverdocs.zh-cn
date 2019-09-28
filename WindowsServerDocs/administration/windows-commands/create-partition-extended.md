---
title: 创建分区扩展
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 21620da46be0e1375f320172e7ccfe2edc338114
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378910"
---
# <a name="create-partition-extended"></a>创建分区扩展

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在具有焦点的磁盘上创建扩展分区。 只能在主启动记录上使用此命令 \(MBR @ no__t 磁盘。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                                                             描述                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size @ no__t-0 @ no__t-1  |                                                  以 mb 为单位指定分区的大小 @no__t-配置包括 @ no__t-1。 如果没有给定大小，则分区会一直继续，直到扩展分区中没有可用空间为止。                                                  |
| offset @ no__t-0 @ no__t-1 |                     指定在其中创建分区的偏移量（kb \(KB @ no__t）。 如果未给出偏移量，分区将从磁盘上可用空间的开始处开始，该空间足以容纳新的分区。                      |
| align @ no__t-0 @ no__t-1  | 将所有分区区区对齐到最接近的对齐边界。 通常与硬件 RAID 逻辑单元号一起使用 \(LUN @ no__t 数组来提高性能。 <n> 是从磁盘开头到最接近的对齐边界的千字节数（kb） \(KB @ no__t。 |
|    noerr    |                                 仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。                                 |
  
## <a name="remarks"></a>备注  
  
-   创建分区之后，焦点会自动转移到新分区。  
  
-   每个磁盘只能创建一个扩展分区。  
  
-   如果尝试在另一个扩展分区中创建扩展分区，则此命令将失败。  
  
-   必须先创建扩展分区，然后才能创建逻辑驱动器。  
  
-   必须选择基本 MBR 磁盘，此操作才能成功。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。  
  
## <a name="BKMK_examples"></a>示例  
若要创建大小为 1000 mb 的扩展分区，请键入：  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

