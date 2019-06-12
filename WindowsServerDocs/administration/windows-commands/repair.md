---
title: 修复
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1e4b9cde10e11558aaa95edda94921144dac1f86
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441803"
---
# <a name="repair"></a>修复

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

修复 RAID\-5 通过用指定的动态磁盘替换故障的磁盘区域来选中卷。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
| 参数  |                                                                                             描述                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disk\=<n>  |                                                                 指定将替换出现故障的磁盘区域的动态磁盘。                                                                 |
| align\=<n> |          将所有卷或分区范围为最接近的对齐边界都对齐。 *n*是千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。           |
|   noerr    | 仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。 |
  
## <a name="remarks"></a>备注  
  
-   指定的动态磁盘可用空间大于或等于在 RAID 中故障的磁盘区域的总大小必须\-5 个卷。  
  
-   在 RAID 卷\-5 数组必须选择此操作才会成功。 使用**选择卷**命令以选择一个卷并将焦点移到它。  
  
## <a name="BKMK_examples"></a>示例  
若要通过它替换为 4 的动态磁盘替换具有焦点的卷，请键入：  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

