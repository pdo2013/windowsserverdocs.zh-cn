---
title: ftp ascii
description: 'Ftp ascii 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5ae0064f9c1679bb8b386271f042d589b158c73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376618"
---
# <a name="ftp-ascii"></a>ftp： ascii

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将文件传输类型设置为 ASCII。   
## <a name="syntax"></a>语法  
```  
ascii  
```  
### <a name="parameters"></a>Parameters  
无  
## <a name="remarks"></a>备注  
- 默认文件传输类型为 ASCII。  
- 在 ASCII 模式下，执行与网络标准字符集之间的字符转换。 例如，根据目标操作系统，将根据需要转换行尾字符。  
- **ftp**支持 ASCII 和二进制图像文件传输类型。 在传输文本文件时使用 ASCII。 有关二进制文件传输的详细信息，请参阅其他引用中的**ftp： binary** 。  
  ## <a name="BKMK_Examples"></a>示例  
  将 "文件传输类型" 设置为 "ASCII"。  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>其他参考  
- [ftp：二进制](ftp-binary.md)  
- [命令行语法项](command-line-syntax-key.md)  
