---
title: ftp 接收
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ec35a2044945e3d39a2a78d39923de3a56eb18d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376125"
---
# <a name="ftp-recv"></a>ftp：接收

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用当前文件传输类型将远程文件复制到本地计算机。   
## <a name="syntax"></a>语法  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parameters  

|   参数   |                   描述                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        指定要复制的远程文件。        |
| [<LocalFile>] | 指定要在本地计算机上使用的名称。 |

## <a name="remarks"></a>备注  
- **接收**命令与**get**命令完全相同。  
- 如果未指定*LocalFile* ，则会为该文件提供*remoteFile*名称。  
  ## <a name="BKMK_Examples"></a>示例  
  使用当前文件传输类型将**test.txt**复制到本地计算机。  
  ```  
  recv test.txt  
  ```  
  使用当前文件传输类型将**test.txt**复制到本地**计算机。**  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>其他参考  
- [ftp： ascii](ftp-ascii.md)  
- [ftp：二进制](ftp-binary.md)  
- [ftp： get](ftp-get.md)  
- [命令行语法项](command-line-syntax-key.md)  
