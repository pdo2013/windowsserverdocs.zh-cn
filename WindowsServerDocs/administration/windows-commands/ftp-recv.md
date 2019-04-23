---
title: ftp 接收
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4979010c13d78c89c9a3e4965b567f7eef1f2ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841098"
---
# <a name="ftp-recv"></a>ftp: recv

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将远程文件复制到本地计算机使用当前的文件传输类型。   
## <a name="syntax"></a>语法  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<remoteFile>|指定要复制的远程文件。|  
|[<LocalFile>]|指定要在本地计算机上使用的名称。|  
## <a name="remarks"></a>备注  
-   **收到**命令等同于**获取**命令。  
-   如果*LocalFile*未指定，则指定文件*remoteFile*名称。  
## <a name="BKMK_Examples"></a>示例  
复制**test.txt**到本地计算机使用当前的文件传输类型。  
```  
recv test.txt  
```  
复制**test.txt**到本地计算机作为**test1.txt**使用当前的文件传输类型。  
```  
recv test.txt test1.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [ftp: get](ftp-get.md)  
-   [命令行语法解答](command-line-syntax-key.md)  
