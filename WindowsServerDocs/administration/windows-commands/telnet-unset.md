---
title: telnet 取消设置
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37c4d84d1664fdc13ea7ffec60bf981b264dba00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853888"
---
# <a name="telnet-unset"></a>telnet: unset

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

关闭以前的 set 选项。   
## <a name="syntax"></a>语法  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|bsasdel|将发送**退格符**作为**退格符**。|  
|crlf|将发送**Enter**密钥为 CR。 也称为换行模式。|  
|delasbs|将发送**删除**作为**删除**。|  
|转义|删除转义字符设置。|  
|localecho|关闭 localecho。|  
|logging|关闭日志记录功能。|  
|ntlm|关闭 NTLM 身份验证。|  
|?|显示此命令的帮助。|  
## <a name="BKMK_Examples"></a>示例  
关闭日志记录。  
```  
u logging  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
