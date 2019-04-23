---
title: ftp ascii
description: 'Windows 命令主题 ftp ascii '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b7bcca3f29cec8ff5c30256dfd123acc7fbb804d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849188"
---
# <a name="ftp-ascii"></a>ftp: ascii

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将文件传输类型设置为 ASCII。   
## <a name="syntax"></a>语法  
```  
ascii  
```  
### <a name="parameters"></a>Parameters  
无  
## <a name="remarks"></a>备注  
-   默认文件传输类型为 ASCII。  
-   在 ASCII 模式下，执行之间的网络标准字符集的字符转换。 例如，行尾字符必要时进行转换，根据目标操作系统。  
-   **ftp**支持 ASCII 和二进制图像文件的传输类型。 传输文本文件时，请使用 ASCII。 有关二进制文件传输的详细信息，请参阅**ftp： 二进制**中其他引用。  
## <a name="BKMK_Examples"></a>示例  
文件传输类型设置为 ASCII。  
```  
ascii  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: binary](ftp-binary.md)  
-   [命令行语法解答](command-line-syntax-key.md)  
