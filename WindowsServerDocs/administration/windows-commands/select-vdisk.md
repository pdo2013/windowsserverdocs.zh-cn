---
title: 选择 vdisk
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bfa6450d1704cde1e5ff2a50a8e3b61a30d0766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384193"
---
# <a name="select-vdisk"></a>选择 vdisk

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

选择指定的虚拟硬盘 \(VHD @ no__t，并将焦点移到该磁盘。  
  
> [!NOTE]  
> 此命令仅适用于 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>语法  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|file @ no__t-0 @ no__t-1|指定现有 VHD 文件的完整路径和文件名。|  
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|  
  
## <a name="BKMK_examples"></a>示例  
若要将焦点转移到名为 "Test" 的 VHD，请键入：  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
-   [附加 vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [分离 vdisk](detach-vdisk.md)  
  
-   [详细信息 vdisk](detail-vdisk.md)  
  
-   [展开 vdisk](expand-vdisk.md)  
  
-   [Merge vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

