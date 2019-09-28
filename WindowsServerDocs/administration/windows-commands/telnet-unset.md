---
title: 未设置 telnet
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6f0eb98c4168d2f664780dad42ca1aea5463d24
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383480"
---
# <a name="telnet-unset"></a>telnet： unset

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

关闭先前设置的选项。   
## <a name="syntax"></a>语法  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|bsasdel|以**backspace**形式发送**backspace** 。|  
|crlf|以 CR 形式发送**Enter**键。 也称为 "换行符" 模式。|  
|delasbs|将**删除**作为**删除**发送。|  
|Esc|删除转义符设置。|  
|localecho|关闭 localecho。|  
|logging|关闭日志记录。|  
|ntlm|关闭 NTLM 身份验证。|  
|?|显示此命令的帮助。|  
## <a name="BKMK_Examples"></a>示例  
关闭日志记录。  
```  
u logging  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
