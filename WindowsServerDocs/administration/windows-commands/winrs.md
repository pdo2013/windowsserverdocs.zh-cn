---
title: winrs
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6ee44cb530614485f0dbd58a9ec13a4788370fe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361938"
---
# <a name="winrs"></a>winrs

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

Windows 远程管理允许您远程管理和执行程序。   
## <a name="syntax"></a>语法  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>Parameters  

|           参数            |                                                                                                                                                                                    描述                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /remote： \<endpoint >       |                                                                                          使用 NetBIOS 名称或标准连接指定目标端点：<br /><br />-    @ no__t： [\<transport >：//] \<target > [： \<port >]<br /><br />如果未指定，则使用 **/r： localhost** 。                                                                                          |
|          /unencrypted          | 指定不加密到远程 shell 的消息。 这对于故障排除或已使用**ipsec**加密网络流量或强制实施物理安全性很有用。<br /><br />默认情况下，使用 Kerberos 或 NTLM 密钥对消息进行加密。<br /><br />选择 HTTPS 传输时，将忽略此命令行选项。 |
|     /username： \<username >      |                                                                                在命令行上指定用户名。<br /><br />如果未指定，则该工具将使用协商身份验证或提示输入名称。<br /><br />如果指定了 **/username** ，则还必须指定 **/password** 。                                                                                 |
|     /password： \<password >      |                                                                           指定命令行上的密码。<br /><br />如果未指定 **/password**但 **/username**为，则该工具将提示输入密码。<br /><br />如果指定了 **/password** ，还必须指定 **/username** 。                                                                            |
|      /timeout： \<seconds >       |                                                                                                                                                                             不推荐使用此选项。                                                                                                                                                                             |
|       /目录： \<path >       |                                                                                            指定远程 shell 的起始目录。<br /><br />如果未指定，将在由环境变量 **% USERPROFILE%** 定义的用户的主目录中启动远程 shell。                                                                                             |
| /environment： \<string > =<value> |                                                                          指定在 shell 开始时要设置的单个环境变量，该变量允许更改 shell 的默认环境。<br /><br />此开关的多次出现必须用于指定多个环境变量。                                                                          |
|            /noecho             |                                                                                                    指定应禁用 echo。 这可能是为了确保不会在本地显示用户对远程提示的答案。<br /><br />默认情况下，echo 为 "on"。                                                                                                    |
|           /noprofile           |                                              指定不应加载用户的配置文件。<br /><br />默认情况下，服务器将尝试加载用户配置文件。<br /><br />如果远程用户不是目标系统上的本地管理员，则需要此选项（默认值将导致错误）。                                               |
|         /allowdelegate         |                                                                                                                  指定用户的凭据可用于访问远程共享，例如，在与目标终结点不同的计算机上找到。                                                                                                                   |
|          /compression          |                                                                           启用压缩。  远程计算机上的较旧安装可能不支持压缩，因此默认情况下处于关闭状态。<br /><br />默认设置为 off，因为远程计算机上的较旧安装可能不支持压缩。                                                                           |
|            /usessl             |                                                                                                               使用远程终结点时使用 SSL 连接。  指定它而不是传输**https：** 将使用默认的**WinRM**默认端口。                                                                                                                |
|               /?               |                                                                                                                                                                        在命令提示符下显示帮助。                                                                                                                                                                        |

## <a name="remarks"></a>备注  
-   所有命令行选项都接受短格式或长格式。 例如， **/r**和 **/remote**都是有效的。  
-   若要终止 **/remote**命令，用户可以键入**ctrl + C**或**ctrl + break**，这将发送到远程 shell。 第二个**ctrl-c**将强制终止**winrs.exe**。  
-   若要管理活动远程 shell 或 winrs 配置，请使用 WinRM 工具。  用于管理活动 shell 的 URI 别名为**shell/cmd**。  Winrs 配置的 URI 别名为**winrm/config/winrs**。  

## <a name="BKMK_Examples"></a>示例  
```  
winrs /r:https://contoso.com command  
```  
```  
winrs /r:contoso.com /usessl command  
```  
```  
winrs /r:myserver command  
```  
```  
winrs /r:http://127.0.0.1 command  
```  
```  
winrs /r:http://169.51.2.101:80 /unencrypted command  
```  
```  
winrs /r:https://[::FFFF:129.144.52.38] command  
```  
```  
winrs /r:http://[1080:0:0:0:8:800:200C:417A]:80 command  
```  
```  
winrs /r:https://contoso.com /t:600 /u:administrator /p:$%fgh7 ipconfig  
```  
```  
winrs /r:myserver /env:path=^%path^%;c:\tools /env:TEMP=d:\temp config.cmd  
```  
```  
winrs /r:myserver netdom join myserver /domain:testdomain /userd:johns /passwordd:$%fgh789  
```  
```  
winrs /r:myserver /ad /u:administrator /p:$%fgh7 dir \\anotherserver\share  
```  

## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

