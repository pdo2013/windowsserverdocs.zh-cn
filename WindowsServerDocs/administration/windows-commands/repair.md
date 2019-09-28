---
title: 修正
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88293422519488405d94e32596c81dbe4a697dee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371527"
---
# <a name="repair"></a>修正

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

通过将出现故障的磁盘区域替换为指定的动态磁盘，修复带有焦点的 RAID @ no__t-05 卷。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
| 参数  |                                                                                             描述                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disk @ no__t-0 @ no__t-1  |                                                                 指定将替换出现故障的磁盘区域的动态磁盘。                                                                 |
| align @ no__t-0 @ no__t-1 |          将所有卷或分区区与最接近的对齐边界对齐。 *n*是从磁盘开头到最接近的对齐边界的千字节数 \(kb @ no__t-2。           |
|   noerr    | 仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。 |
  
## <a name="remarks"></a>备注  
  
-   指定的动态磁盘的可用空间必须大于或等于 RAID @ no__t-05 卷中发生故障的磁盘区域的总大小。  
  
-   必须选择 RAID @ no__t-05 阵列中的卷，此操作才能成功。 使用 "**选择音量**" 命令选择卷并将焦点移动到该卷。  
  
## <a name="BKMK_examples"></a>示例  
若要通过将其替换为动态磁盘4来替换具有焦点的卷，请键入：  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

