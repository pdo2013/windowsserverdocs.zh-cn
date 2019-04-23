---
title: telnet 集
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18a49f2e4629b1410a1cec40fe77077c2988dce2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836118"
---
# <a name="telnet-set"></a>telnet： 设置

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

设置选项。   
## <a name="syntax"></a>语法  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|bsasdel|将发送**退格符**作为**删除**。|  
|crlf|将发送 CR 和 LF (0x0D，0x0a) 时**Enter**按下键。 名为新行模式。|  
|delasbs|将发送**删除**作为**退格符**。|  
|转义 <Character>|设置用于输入 telnet 客户端提示符的转义字符。 转义字符可以是单个字符，也可以是一系列**CTRL**键以及一个字符。 若要设置的控制键组合，按下**CTRL**键的同时键入你想要分配的字符。|  
|localecho|打开本地回显。|  
|日志文件 <FileName>|将当前 telnet 会话记录到本地文件。 当设置此选项，自动开始记录。|  
|logging|启用日志记录。 如果不设置了任何日志文件，将显示错误消息。|  
|模式 {控制台&#124;屏幕}|将操作模式设置。|  
|ntlm|打开 NTLM 身份验证。|  
|术语 {ansi &#124; vt100 &#124; vt52 &#124; vtnt}|设置的终端类型。|  
|?|显示此命令的帮助。|  
## <a name="remarks"></a>备注  
1.  可以使用**取消设置**命令关闭以前设置的选项。  
2.  在非英语版本的 telnet**代码集**<option>可用。 **代码集**<option>设置设置为一个选项，可以是以下任何项的当前代码： **shift JIS**，**日语 EUC**， **JIS 日文汉字**，**JIS 日文汉字 (78)**， **DEC 日文汉字**， **NEC 日文汉字**。 应设置在远程计算机上设置的相同代码。  
## <a name="BKMK_Examples"></a>示例  
设置日志文件并开始记录到本地文件 tnlog.txt  
```  
set logfile tnlog.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
