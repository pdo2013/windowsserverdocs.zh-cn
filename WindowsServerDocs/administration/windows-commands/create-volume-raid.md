---
title: 创建卷 raid
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5a3c13cb5b78ae3e771b461a35a7130a48e7ec01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378861"
---
# <a name="create-volume-raid"></a>创建卷 raid

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用三个或更多指定的动态磁盘创建一个 RAID @ no__t-05 卷。  
  
> [!IMPORTANT]  
> 在任何版本的 Windows Vista 中，此 DiskPart 命令均不可用。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|           参数           |                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           size @ no__t-0 @ no__t-1           | 卷在每个磁盘上占用的磁盘空间量（以 mb 为单位 @no__t-配置包括 @ no__t）。 如果没有给定大小，则将创建可能的最大 RAID @ no__t-05 卷。 具有最小可用连续可用空间的磁盘确定了 RAID @ no__t-05 卷的大小，并从每个磁盘分配相同的空间量。 RAID @ no__t-05 卷中的实际可用磁盘空间量小于磁盘空间的合并量，因为某些磁盘空间对于奇偶校验是必需的。 |
| disk @ no__t-0 @ no__t，<n>，<n> @ no__t，<n>,... \] |                                                                                                                                               要在其上创建 RAID @ no__t-05 卷的动态磁盘。 需要至少三个动态磁盘才能创建 RAID @ no__t-05 卷。 在每个磁盘上分配**大小为 @ no__t-1 @ no__t-2**的空间。                                                                                                                                                |
|          align @ no__t-0 @ no__t-1           |                                                                                                                   将所有卷区与最接近的对齐边界对齐。 通常与硬件 RAID 逻辑单元号一起使用 \(LUN @ no__t 数组来提高性能。 *n*是从磁盘开头到最接近的对齐边界的千字节数 \(kb @ no__t-2。                                                                                                                   |
|             noerr             |                                                                                                                                                 仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。                                                                                                                                                  |
  
## <a name="remarks"></a>备注  
  
-   创建卷后，焦点会自动转移到新卷。  
  
## <a name="BKMK_examples"></a>示例  
若要创建大小为 1000 mb 的 RAID @ no__t-05，使用磁盘1、2和3，请键入：  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

