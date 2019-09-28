---
title: 创建分区 efi
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76d97129fd67345f23eee2fc7b300493a1632cc6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379006"
---
# <a name="create-partition-efi"></a>创建分区 efi

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在 Itanium @ no__t-0based 计算机上，在 GUID 分区表 \(gpt @ no__t-4 磁盘上创建 \(EFI @ no__t 系统分区的可扩展固件接口。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size @ no__t-0 @ no__t-1  |                         分区的大小（mb） @no__t-配置包括 @ no__t-1。 如果没有给定大小，则分区会一直继续，直至当前区域中没有可用空间为止。                         |
| offset @ no__t-0 @ no__t-1 |             在其中创建分区的偏移量（kb） \(KB @ no__t-1。 如果未给出偏移量，则会将该分区置于足够大的第一个磁盘范围中以容纳该分区。              |
|    noerr    | 仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。 |
  
## <a name="remarks"></a>备注  
  
-   创建分区后，会将焦点提供给新的分区。  
  
-   若要成功执行此操作，必须选择 gpt 磁盘。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。  
  
## <a name="BKMK_examples"></a>示例  
若要在所选磁盘上创建 1000 mb 的 EFI 分区，请键入：  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  

