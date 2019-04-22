---
title: 分离的虚拟磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b6a1ecd3d787506c89f120bed204cc30e6d68d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822728"
---
# <a name="detach-vdisk"></a>分离的虚拟磁盘

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

停止所选的虚拟硬盘\(VHD\)使其不显示作为主计算机上的本地硬盘驱动器。 当分离 VHD 时，可以将其复制到其他位置。  
  
> [!NOTE]  
> 此命令当前仅适用于 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>语法  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|noerr|用于仅编写脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   必须选择并为此操作才能成功分离 VHD。 使用**选择的虚拟磁盘**命令选择 VHD，并将焦点移到它。  
  
## <a name="BKMK_Examples"></a>示例  
若要分离所选的 VHD，请键入：  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法解答](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [压缩的虚拟磁盘](compact-vdisk.md)  
  
  
  
-   [detail vdisk](detail-vdisk.md)  
  
-   [展开的虚拟磁盘](expand-vdisk.md)  
  
-   [合并的虚拟磁盘](merge-vdisk.md)  
  
-   [选择的虚拟磁盘](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

