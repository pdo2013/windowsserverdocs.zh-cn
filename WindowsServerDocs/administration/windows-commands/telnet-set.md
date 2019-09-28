---
title: telnet 集
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e39e2812edc9cd5f169a046def26beebda1d007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383618"
---
# <a name="telnet-set"></a>telnet：设置

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

设置选项。   
## <a name="syntax"></a>语法  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>Parameters  

|                    参数                     |                                                                                                                                              描述                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 发送**Backspace**作为**删除**。                                                                                                                                  |
|                       crlf                       |                                                                                                        按下**Enter**键时，发送 CR & LF （0x0D，0x 0A）。 称为 "新行模式"。                                                                                                        |
|                     delasbs                      |                                                                                                                                 以**Backspace**形式发送**删除**。                                                                                                                                  |
|                转义 <Character>                | 设置用于输入 telnet 客户端提示符的转义符。 转义符可以是单个字符，也可以是**CTRL**键加字符的组合。 若要设置控件键组合，请在键入要分配的字符时按住**CTRL**键。 |
|                    localecho                     |                                                                                                                                         启用本地回显。                                                                                                                                          |
|                日志文件 <FileName>                |                                                                                               将当前的 telnet 会话记录到本地文件。 设置此选项后，会自动开始日志记录。                                                                                               |
|                     logging                      |                                                                                                                  启用日志记录。 如果未设置日志文件，将显示一条错误消息。                                                                                                                   |
|           模式 {控制台&#124;屏幕}           |                                                                                                                                       设置操作模式。                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     启用 NTLM 身份验证。                                                                                                                                     |
| 字词 {ansi &#124; vt100 &#124; vt52 &#124; vtnt} |                                                                                                                                        设置终端类型。                                                                                                                                        |
|                        ?                         |                                                                                                                                    显示此命令的帮助。                                                                                                                                    |

## <a name="remarks"></a>备注  
1. 您可以使用 "取消**设置**" 命令关闭先前设置的选项。  
2. 在非英语版本的 telnet 上， **codeset** <option> 可用。 **Codeset** <option> 将设置的当前代码设置为一个选项，该选项可以是以下任意一项： **shift JIS**、**日语 EUC**、jis **78** **日文** 应在远程计算机上设置相同的代码集。  
   ## <a name="BKMK_Examples"></a>示例  
   设置日志文件并开始记录到本地文件 tnlog  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>其他参考  
3. [命令行语法项](command-line-syntax-key.md)  
