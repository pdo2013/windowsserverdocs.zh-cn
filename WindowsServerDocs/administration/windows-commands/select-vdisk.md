---
title: 选择的虚拟磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a71a5c15c05a1e969d0720bc8e67e669d553f649
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852568"
---
# <a name="select-vdisk"></a>选择的虚拟磁盘

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

选择指定虚拟硬盘\(VHD\)和将焦点转移给它。  
  
> [!NOTE]  
> 此命令当前仅适用于 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>语法  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|file\=<full path>|指定现有的 VHD 文件的完整路径和文件名。|  
|noerr|用于仅编写脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="BKMK_examples"></a>示例  
若要将焦点移到名为 Test.vhd VHD 中，键入：  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>其他参考  
  
-   [命令行语法解答](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [压缩的虚拟磁盘](compact-vdisk.md)  
  
  
  
-   [分离的虚拟磁盘](detach-vdisk.md)  
  
-   [detail vdisk](detail-vdisk.md)  
  
-   [展开的虚拟磁盘](expand-vdisk.md)  
  
-   [合并的虚拟磁盘](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

