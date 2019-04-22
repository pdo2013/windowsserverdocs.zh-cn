---
title: ftp 二进制
description: Ftp 的二进制文件的的 Windows 命令主题
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cadd59bff3bd2acf5c6d700caef66ca5c871b523
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821918"
---
# <a name="ftp-binary"></a>ftp： 二进制

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将文件传输类型设置为二进制。   
## <a name="syntax"></a>语法  
```  
binary  
```  
### <a name="parameters"></a>Parameters  
无  
## <a name="remarks-optional-section"></a>备注 <optional section>  
**ftp**支持 ASCII 和二进制图像文件的传输类型。 传输可执行文件时，请使用二进制文件。 在二进制模式下，文件传输单字节为单位。 有关 ASCII 文件传输的详细信息，请参阅**ftp: ascii**中其他引用。  
## <a name="BKMK_Examples"></a>示例  
文件传输类型设置为二进制文件。  
```  
binary  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: ascii](ftp-ascii.md)  
-   [命令行语法解答](command-line-syntax-key.md)  
