---
title: ftp send_1
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9f658b4fbfa5f6c9fa9a58fb0c524ad53627bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375998"
---
# <a name="ftp-send_1"></a>ftp： send_1

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用当前文件传输类型将本地文件复制到远程计算机。   
## <a name="syntax"></a>语法  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parameters  

|  参数   |                    描述                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         指定要复制的本地文件。         |
| <remoteFile> | 指定要在远程计算机上使用的名称。 |

## <a name="remarks"></a>备注  
- **Send**命令与**put**命令完全相同。  
- 如果未指定*remoteFile* ，则会为该文件提供*LocalFile*名称。  
  ## <a name="BKMK_Examples"></a>示例  
  复制本地文件**test.txt** ，并在远程计算机上将其命名为**test1。**  
  ```  
  send test.txt test1.txt  
  ```  
  将本地文件**setup.exe**复制到远程计算机。  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>其他参考  
- [命令行语法项](command-line-syntax-key.md)  
