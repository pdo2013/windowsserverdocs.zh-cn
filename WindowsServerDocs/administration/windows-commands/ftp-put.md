---
title: ftp put
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3307ba71e7b3c8b4113f9ed29ab06660dafa5f6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868738"
---
# <a name="ftp-put"></a>ftp: put

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将本地文件复制到远程计算机使用当前的文件传输类型。   
## <a name="syntax"></a>语法  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<LocalFile>|指定要复制的本地文件。|  
|[<remoteFile>]|指定要在远程计算机上使用的名称。|  
## <a name="remarks"></a>备注  
-   **放**命令等同于**发送**命令。  
-   如果*remoteFile*未指定，则指定文件*LocalFile*名称。  
## <a name="BKMK_Examples"></a>示例  
将本地文件复制**test.txt**并将其命名**test1.txt**远程计算机上。  
```  
put test.txt test1.txt  
```  
将本地文件复制**program.exe**到远程计算机。  
```  
put program.exe  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令行语法解答](command-line-syntax-key.md)  
