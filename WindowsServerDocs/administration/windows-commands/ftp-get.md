---
title: ftp get
description: 获取 ftp 的 Windows 命令主题
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28961ccf0ae04b52586728f9c68a9b2ca3e69b1d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438777"
---
# <a name="ftp-get"></a>ftp： 获取

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将远程文件复制到本地计算机使用当前的文件传输类型。   
## <a name="syntax"></a>语法  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parameters  

|   参数   |                                                              描述                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   指定要复制的远程文件。                                                   |
| [<LocalFile>] | 指定要在本地计算机上使用的文件的名称。 如果*LocalFile*未指定，则指定文件*remoteFile*名称。 |

## <a name="remarks"></a>备注  
**获取**命令等同于**收到**命令。  
## <a name="BKMK_Examples"></a>示例  
复制**test.txt**到本地计算机使用当前的文件传输类型。  
```  
get test.txt  
```  
复制**test.txt**到本地计算机作为**test1.txt**使用当前的文件传输类型。  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令行语法项](command-line-syntax-key.md)  
