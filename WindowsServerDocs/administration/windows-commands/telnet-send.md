---
title: telnet 发送
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 958e0e507e5a0ae836da98de8d677a116dbb38bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383631"
---
# <a name="telnet-send"></a>telnet：发送

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

向 telnet 服务器发送 telnet 命令。   
## <a name="syntax"></a>语法  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parameters  

| 参数 |                     描述                      |
|-----------|------------------------------------------------------|
|    ao     |       发送 telnet 命令中止输出。        |
|    ayt    |       发送 telnet 命令。       |
|    brk    |            发送 telnet 命令 brk。            |
|    Ecs    |      发送当前的 telnet 转义符。      |
|    lip     |     发送 telnet 命令中断进程。     |
|   保持   |           发送 telnet 命令同步。           |
| <string>  | 将键入的任何字符串发送到 telnet 服务器。 |
|     ?     |     显示与此命令相关联的帮助。      |

## <a name="BKMK_Examples"></a>示例  
发送到 telnet 服务器。  
```  
sen ayt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
