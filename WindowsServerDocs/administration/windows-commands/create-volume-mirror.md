---
title: 创建卷镜像
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 72ecc4e0ede163857c47c5b7013aacdd49719ac8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378867"
---
# <a name="create-volume-mirror"></a>创建卷镜像

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用两个指定的动态磁盘创建卷镜像。  
  
> [!NOTE]  
> 此命令仅适用于 Windows 7 和 Windows Server 2008 R2。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|         参数         |                                                                                                                                     描述                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         size @ no__t-0 @ no__t-1         |                 指定卷在每个磁盘上占用的磁盘空间量（以 mb 为单位 @no__t-配置包括 @ no__t）。 如果没有给定大小，则新卷将占用最小磁盘上的剩余可用空间，并在每个后续磁盘上占用相同的空间量。                 |
| disk @ no__t-0 @ no__t，<n> @ no__t，<n>,... \] |                       指定在其上创建镜像卷的动态磁盘。 需要两个动态磁盘来创建镜像卷。 在每个磁盘上分配的空间量等于使用**size**参数指定的大小。                        |
|        align @ no__t-0 @ no__t-1         | 将所有卷区与最接近的对齐边界对齐。 此参数通常与硬件 RAID 逻辑单元号 \(LUN @ no__t 数组结合使用，以提高性能。 *n*是从磁盘开头到最接近的对齐边界的千字节数 \(kb @ no__t-2。 |
|           noerr           |                                        仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误。                                         |
  
## <a name="remarks"></a>备注  
  
-   创建卷后，焦点会自动转移到新卷。  
  
## <a name="BKMK_examples"></a>示例  
若要创建大小为 1000 mb 的镜像卷，请在磁盘1和2上键入：  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

