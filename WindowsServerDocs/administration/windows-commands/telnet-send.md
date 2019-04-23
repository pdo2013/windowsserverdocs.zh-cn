---
title: telnet 发送
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 32345b22395107f4a2c3d88894126d4e5e0875a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842358"
---
# <a name="telnet-send"></a>telnet： 发送

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将 telnet 命令发送到 telnet 服务器。   
## <a name="syntax"></a>语法  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|ao|发送 telnet 命令中止输出。|  
|ayt|将 telnet 命令那里是您发送。|  
|brk|发送 telnet 命令 brk。|  
|esc|将发送当前 telnet 转义符。|  
|ip|发送 telnet 命令中断进程。|  
|同步|发送 telnet 命令同步。|  
|<string>|将你键入的任何字符串发送到 telnet 服务器。|  
|?|使用此命令相关联的显示帮助。|  
## <a name="BKMK_Examples"></a>示例  
发送存在到 telnet 服务器。  
```  
sen ayt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
