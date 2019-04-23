---
title: 创建简单卷
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d754a6b5788656132459a0b4a42954e9084f26bb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836498"
---
# <a name="create-volume-simple"></a>创建简单卷

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在指定的动态磁盘上创建简单卷。  
  
> [!IMPORTANT]  
> 对于 Windows Vista，此 DiskPart 命令才可用的 Windows Vista Ultimate、 Windows Vista Enterprise 和 Windows Vista Business 版本中。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|size\=<n>|以兆字节表示卷的大小\(MB\)。 如果没有给定大小，新建卷将占用磁盘上剩余的可用空间。|  
|disk\=<n>|在其创建卷的动态磁盘。 如果指定了不到磁盘，则使用当前磁盘。|  
|align\=<n>|对齐到最接近的对齐边界的所有卷扩展盘区。 通常与硬件 RAID 逻辑单元号一起使用\(LUN\)数组以提高性能。 *n*是千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。|  
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   创建卷后，焦点就自动转移到新卷。  
  
## <a name="BKMK_examples"></a>示例  
若要创建 1000 兆字节的卷的大小，磁盘 1 上键入：  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  

