---
title: ftp 二进制文件
description: Ftp 二进制文件的 Windows 命令主题
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48579523f44232dec3357a20e8082050cc5175e6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376579"
---
# <a name="ftp-binary"></a>ftp：二进制

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将文件传输类型设置为 binary。   
## <a name="syntax"></a>语法  
```  
binary  
```  
### <a name="parameters"></a>Parameters  
无  
## <a name="remarks-optional-section"></a>备注 <optional section>  
**ftp**支持 ASCII 和二进制图像文件传输类型。 传输可执行文件时，请使用二进制。 在二进制模式下，文件以单字节单位传输。 有关 ASCII 文件传输的详细信息，请参阅其他引用中的**ftp： ASCII** 。  
## <a name="BKMK_Examples"></a>示例  
将 "文件传输类型" 设置为 "二进制"。  
```  
binary  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp： ascii](ftp-ascii.md)  
-   [命令行语法项](command-line-syntax-key.md)  
