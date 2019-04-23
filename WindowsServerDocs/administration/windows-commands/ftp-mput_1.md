---
title: ftp mput_1
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 99b938618deb2d1e779fd20c504c01a13a2d3f8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868158"
---
# <a name="ftp-mput1"></a>ftp: mput_1

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将本地文件复制到远程计算机使用当前的文件传输类型。   
## <a name="syntax"></a>语法  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<LocalFile>|指定要复制到远程计算机的本地文件。|  
## <a name="BKMK_Examples"></a>示例  
复制**Program1.exe**并**Program2.exe**到远程计算机使用当前的文件传输类型。  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令行语法解答](command-line-syntax-key.md)  
