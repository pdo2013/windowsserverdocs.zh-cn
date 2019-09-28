---
title: ftp put
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15c1734322d3642ebc85891b71c6ad68100d514d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376072"
---
# <a name="ftp-put"></a>ftp： put

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用当前文件传输类型将本地文件复制到远程计算机。   
## <a name="syntax"></a>语法  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parameters  

|   参数    |                    描述                    |
|----------------|---------------------------------------------------|
|  <LocalFile>   |         指定要复制的本地文件。         |
| [<remoteFile>] | 指定要在远程计算机上使用的名称。 |

## <a name="remarks"></a>备注  
- **Put**命令与**send**命令完全相同。  
- 如果未指定*remoteFile* ，则会为该文件提供*LocalFile*名称。  
  ## <a name="BKMK_Examples"></a>示例  
  复制本地文件**test.txt** ，并在远程计算机上将其命名为**test1。**  
  ```  
  put test.txt test1.txt  
  ```  
  将本地文件**setup.exe**复制到远程计算机。  
  ```  
  put program.exe  
  ```  
  ## <a name="additional-references"></a>其他参考  
- [ftp： ascii](ftp-ascii.md)  
- [ftp：二进制](ftp-binary.md)  
- [命令行语法项](command-line-syntax-key.md)  
