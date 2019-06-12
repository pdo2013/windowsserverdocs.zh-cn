---
title: ftp mdir
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c4ec445c3e367a46dc40d10a37c0b3b8e53a10e3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438328"
---
# <a name="ftp-mdir"></a>ftp: mdir

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

远程目录中显示文件和子目录的目录列表。   
## <a name="syntax"></a>语法  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parameters  

|  参数   |                               描述                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   指定你想要查看列表的目录或文件。   |
| <LocalFile>  | 指定要应用商店一览的本地文件。 此参数是必需的。 |

## <a name="remarks"></a>备注  
- 可以使用**mdir**若要指定多个文件。  
- 指定*remoteFile*  
  键入一个连字符 ( **-** ) 若要在远程计算机上使用当前工作目录。  
- 指定*本地文件*  
  键入一个连字符 ( **-** ) 以在屏幕上显示该列表。  
  ## <a name="BKMK_Examples"></a>示例  
  显示的目录列表**dir1**并**dir2**在屏幕上  
  ```  
  mdir dir1 dir2 -  
  ```  
  保存的组合的目录列表**dir1**并**dir2**本地文件中名为**dirlist.txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>其他参考  
- [命令行语法项](command-line-syntax-key.md)  
