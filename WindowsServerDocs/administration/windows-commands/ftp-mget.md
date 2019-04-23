---
title: ftp mget
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1160ec742dde318141da720bd35b7d60ab805bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888418"
---
# <a name="ftp-mget"></a>ftp: mget

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将远程文件复制到本地计算机使用当前的文件传输类型。   
## <a name="syntax"></a>语法  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<remoteFile>|指定要复制到本地计算机的远程文件。|  
## <a name="BKMK_Examples"></a>示例  
将远程文件复制**a.exe**并**b.exe**到本地计算机使用当前的文件传输类型。  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令行语法解答](command-line-syntax-key.md)  
