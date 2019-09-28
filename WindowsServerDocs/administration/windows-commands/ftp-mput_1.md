---
title: ftp mput_1
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6308f7b47d58bc25d964944f96fbc83a26350962
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376215"
---
# <a name="ftp-mput_1"></a>ftp： mput_1

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用当前文件传输类型将本地文件复制到远程计算机。   
## <a name="syntax"></a>语法  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parameters  

|  参数  |                       描述                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | 指定要复制到远程计算机的本地文件。 |

## <a name="BKMK_Examples"></a>示例  
使用当前文件传输类型将**Program1**和**program2.c**复制到远程计算机。  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp： ascii](ftp-ascii.md)  
-   [ftp：二进制](ftp-binary.md)  
-   [命令行语法项](command-line-syntax-key.md)  
