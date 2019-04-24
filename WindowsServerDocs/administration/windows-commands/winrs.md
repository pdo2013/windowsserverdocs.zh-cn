---
title: winrs
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9224e2572d7d5efded149cd113730dabc1624299
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843608"
---
# <a name="winrs"></a>winrs

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Windows 远程管理允许你管理和远程执行程序。   
## <a name="syntax"></a>语法  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[/remote]:\<endpoint>|指定使用 NetBIOS 名称或标准连接的目标终结点：<br /><br />-   <url>: [\<transport>://]\<target>[:\<port>]<br /><br />如果未指定，否则 **/r:localhost**使用。|  
|/unencrypted]|指定发送到远程外壳程序的消息不会加密。 这可用于故障排除或使用已加密的网络流量**ipsec**，或何时强制实施物理安全性。<br /><br />默认情况下，消息是使用 Kerberos 或 NTLM 密钥加密的。<br /><br />选择 HTTPS 传输时，将忽略此命令行选项。|  
|/username]:\<用户名 >|在命令行上指定用户名。<br /><br />如果未指定，则工具将使用协商身份验证或提示的名称。<br /><br />如果 **/username**指定，则 **/password**还必须指定。|  
|/password]:\<password>|在命令行上指定密码。<br /><br />如果 **/password**未指定，但 **/username**是，该工具将提示您输入密码。<br /><br />如果 **/password**指定，则 **/username**还必须指定。|  
|/timeout:\<seconds>|此选项已弃用。|  
|/ 目录：\<路径 >|指定远程外壳程序的开始目录。<br /><br />如果未指定，远程外壳程序将在由环境变量定义的用户的主目录中启动 **%USERPROFILE%**。|  
|/environment:\<string>=<value>|指定命令行程序将启动，这样就能够更改默认环境 shell 时设置的单个环境变量。<br /><br />必须使用此开关的多个匹配项来指定多个环境变量。|  
|/noecho|指定应禁用该 echo。 这可能有必要以确保不会本地显示远程提示用户的答案。<br /><br />默认情况下 echo 为"开"。|  
|/noprofile|指定应加载用户的配置文件。<br /><br />默认情况下，服务器将尝试加载用户配置文件。<br /><br />如果远程用户不是目标系统上的本地管理员，则将需要此选项 （默认值将导致错误）。|  
|/allowdelegate|指定的用户的凭据可用于访问远程共享目标终结点以外的其他计算机上找到。|  
|/compression|启用压缩。  远程计算机上的较旧的安装可能不支持压缩，因此它默认处于关闭状态。<br /><br />默认设置为关闭，因为远程计算机上的较旧的安装可能不支持压缩。|  
|/usessl|使用远程终结点时，请使用 SSL 连接。  指定此而不传输**https:** 将使用默认**WinRM**默认端口。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
-   所有命令行选项接受短格式或长格式。 例如两者 **/r**并 **/远程**有效。  
-   若要终止 **/远程**命令时，用户可以键入**Ctrl + C**或**ctrl + break**，它将发送到远程外壳程序。 第二个**Ctrl + C**将强制终止**winrs.exe**。  
-   若要管理活动的远程外壳程序或 winrs 配置，请使用 WinRM 工具。  URI 别名以管理 active shell **shell/cmd**。  别名 winrs 配置的 URI 是**winrm/config/winrs**。  

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
-   [命令行语法解答](command-line-syntax-key.md)  
  
