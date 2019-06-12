---
title: ftp 类型
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a261382da47501b416fa83c6d2497deae5711bb1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438355"
---
# <a name="ftp-type"></a>ftp： 类型

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

设置或显示文件传输类型。   
## <a name="syntax"></a>语法  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>Parameters  

|  参数   |            描述            |
|--------------|-----------------------------------|
| [<typeName>] | 指定的文件传输类型。 |

## <a name="remarks"></a>备注  
- 如果*typeName*未指定，则显示当前的类型。  
- **ftp**支持两个文件的传输类型、 ASCII 和二进制文件。  
  默认文件传输类型为 ASCII。  **Ascii**传输文本文件时，应使用命令。 在 ASCII 模式下，执行之间的网络标准字符集的字符转换。 例如，行尾字符都转换为所需的、 基于目标操作系统上。  
  **二进制**传输可执行文件时，应使用命令。 在二进制模式下，该文件移动以单字节为单位。  
  ## <a name="BKMK_Examples"></a>示例  
  文件传输类型设置为 ASCII。  
  ```  
  type ascii  
  ```  
  将传输文件类型设置为二进制文件。  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>其他参考  
- [命令行语法项](command-line-syntax-key.md)  
