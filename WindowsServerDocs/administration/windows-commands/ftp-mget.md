---
title: ftp mget
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666025c92b6fb1a612cbe7b83833557a8a7d5017
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376296"
---
# <a name="ftp-mget"></a>ftp： mget

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用当前文件传输类型将远程文件复制到本地计算机。   
## <a name="syntax"></a>语法  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parameters  

|  参数   |                        描述                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | 指定要复制到本地计算机的远程文件。 |

## <a name="BKMK_Examples"></a>示例  
使用当前文件传输类型将远程文件 **.exe**和 **.exe**复制到本地计算机。  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp： ascii](ftp-ascii.md)  
-   [ftp：二进制](ftp-binary.md)  
-   [命令行语法项](command-line-syntax-key.md)  
