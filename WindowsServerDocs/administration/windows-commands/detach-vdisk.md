---
title: 分离 vdisk
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4850f9f17218178f210820dd4c6ca96fd918accc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378687"
---
# <a name="detach-vdisk"></a>分离 vdisk

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

停止所选的虚拟硬盘 \(VHD @ no__t，将其显示为主计算机上的本地硬盘驱动器。 分离 VHD 后，可以将其复制到其他位置。  
  
> [!NOTE]  
> 此命令仅适用于 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>语法  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   必须选择并分离 VHD，此操作才能成功。 使用 "**选择 vdisk** " 命令选择 VHD 并将焦点移动到该 VHD。  
  
## <a name="BKMK_Examples"></a>示例  
要分离所选 VHD，请键入：  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
-   [附加 vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [详细信息 vdisk](detail-vdisk.md)  
  
-   [展开 vdisk](expand-vdisk.md)  
  
-   [Merge vdisk](merge-vdisk.md)  
  
-   [选择 vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

