---
title: tlntadmn
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b4423e35d0c26819188001dea8d3d8497add7f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440952"
---
# <a name="tlntadmn"></a>tlntadmn

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

管理正在运行的 telnet 服务器服务在本地或远程计算机。   
## <a name="syntax"></a>语法  
```  
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]  
```  
### <a name="parameters"></a>Parameters  

|                   参数                    |                                                                                                                                                       描述                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                \<computerName>                 |                                                                                                                    指定要连接到的服务器的名称。 默认值为本地计算机。                                                                                                                    |
|         -u \<UserName> -p \<Password>          |                                                指定用于你想要管理的远程服务器管理凭据。 如果你想要管理远程服务器，您都不登录到使用管理凭据，此参数是必需的。                                                |
|                     start                      |                                                                                                                                            启动 telnet 服务器服务。                                                                                                                                             |
|                      stop                      |                                                                                                                                             停止 telnet 服务器服务                                                                                                                                              |
|                     pause                      |                                                                                                                          暂停的 telnet 服务器服务。 将不接受任何新连接。                                                                                                                          |
|                    继续                    |                                                                                                                                            恢复 telnet 服务器服务。                                                                                                                                            |
|          -s {\<SessionID >&#124;所有}          |                                                                                                                                             显示活动 telnet 会话。                                                                                                                                             |
|          -k {\<SessionID> &#124; all}          |                                                                                                        结束 telnet 会话。 键入要结束特定会话的会话 ID 或键入所有要结束所有会话。                                                                                                         |
|    -m {\<SessionID> &#124; all}  <Message>     |                                                   将消息发送到一个或多个会话。 键入要将消息发送到特定的会话，会话 ID 或键入所有要将消息发送到所有会话。 键入你想要在引号内发送的消息。                                                   |
|             配置 dom =\<域 >             |                                                                                                                                      配置服务器的默认域。                                                                                                                                       |
|      config ctrlakeymap = {yes &#124; no}      |                                                                                     指定您需要的 telnet 服务器解释为 ALT CTRL + A。 类型**是**到映射的快捷键或类型**没有**防止映射。                                                                                     |
|       配置超时 = \<hh >:\<mm >:\<ss >       |                                                                                                                                 在小时、 分钟和秒为单位设置超时期限。                                                                                                                                 |
|     config timeoutactive = {yes &#124; no      |                                                                                                                                            允许空闲会话超时值。                                                                                                                                             |
|          config maxfail = \<attempts>          |                                                                                                                          设置最大断开连接之前的失败的登录尝试次数。                                                                                                                          |
|        配置 maxconn =\<连接 >         |                                                                                                                                         设置最大连接数。                                                                                                                                          |
|            config port = <\Number>             |                                                                                                                    设置 telnet 端口。 您必须指定端口小于 1024年的整数。                                                                                                                    |
| config sec {+ &#124; -}NTLM {+ &#124; -}passwd | 指定是否想要使用 NTLM、 一个密码，或这两者进行身份验证的登录尝试次数。 若要使用特定类型的身份验证，请键入加号 ( **+** ) 之前该类型的身份验证。 若要防止使用特定类型的身份验证，请键入减号 ( **-** ) 之前该类型的身份验证。 |
|     config mode = {console &#124; stream}      |                                                                                                                                             指定操作的模式。                                                                                                                                             |
|                       -?                       |                                                                                                                                           在命令提示符下显示帮助。                                                                                                                                           |

## <a name="remarks"></a>备注  
-   若要显示的服务器设置，请键入**tlntadmn**不带任何参数。  
-   若要使用**tlntadmn**命令时，您必须登录到本地计算机上具有管理凭据。 若要管理远程计算机，您还必须为远程计算机提供管理凭据。 可以通过登录到本地计算机具有本地计算机和远程计算机的管理凭据的帐户执行操作。 如果您不能使用此方法，则可以使用 **-u**并 **-p**参数为远程计算机提供管理凭据。  

## <a name="BKMK_Examples"></a>示例  
配置空闲会话超时为 30 分钟。  
```  
tlntadmn config timeout=0:30:0  
```  
显示活动 telnet 会话。  
```  
tlntadmn -s  
```  

## <a name="additional-references"></a>其他参考  
-   [telnet 操作指南](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)  
-   [命令行语法项](command-line-syntax-key.md)  
