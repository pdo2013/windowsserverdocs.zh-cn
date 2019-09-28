---
title: ftp mdelete_1
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ffbb06cff9e177316ea60e281c25d7640a7f981
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375920"
---
# <a name="ftp-mdelete_1"></a>ftp： mdelete_1

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

删除远程计算机上的文件。   
## <a name="syntax"></a>语法  
```  
mdelete <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parameters  

|  参数   |             描述              |
|--------------|--------------------------------------|
| <remoteFile> | 指定要删除的远程文件。 |

## <a name="BKMK_Examples"></a>示例  
删除远程文件 **.exe**和 **.exe**。  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
