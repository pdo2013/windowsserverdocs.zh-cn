---
title: ftp mdir
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08aa5bb216a3d0155c100c761e476bb963e59311
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375847"
---
# <a name="ftp-mdir"></a>ftp： mdir

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示远程目录中的文件和子目录的目录列表。   
## <a name="syntax"></a>语法  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parameters  

|  参数   |                               描述                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   指定要查看其列表的目录或文件。   |
| <LocalFile>  | 指定用于存储列表的本地文件。 此参数是必需的。 |

## <a name="remarks"></a>备注  
- 您可以使用**mdir**来指定多个文件。  
- 指定*remoteFile*  
  键入连字符（ **-** ）以使用远程计算机上的当前工作目录。  
- 指定*LocalFile*  
  键入连字号（ **-** ）以在屏幕上显示列表。  
  ## <a name="BKMK_Examples"></a>示例  
  在屏幕上显示 " **dir1** " 和 " **dir2** " 的目录列表  
  ```  
  mdir dir1 dir2 -  
  ```  
  将**dir1**和**dir2**的合并目录列表保存在名为**dirlist**的本地文件中  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>其他参考  
- [命令行语法项](command-line-syntax-key.md)  
