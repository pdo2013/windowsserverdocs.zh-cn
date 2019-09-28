---
title: DirectAccess 疑难解答
description: 本主题提供有关在 Windows Server 2016 中对 DirectAccess 部署进行故障排除的信息。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ee1eb6f9855174357242d2689567b394b75aed1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394474"
---
# <a name="troubleshooting-directaccess"></a>DirectAccess 疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

请按照以下步骤解决远程访问（DirectAccess）问题。  
  
|||  
|-|-|  
|**问题**|**解决方法**|  
|远程访问管理控制台无法显示 DirectAccess 配置|**还原缺少的配置信息**<br />-如果对多站点部署进行故障排除，请确保最接近入口点的域控制器可用。<br />-使用**set-daentrypointdc** cmdlet 检索离入口点最近的域控制器的名称。 如果域控制器未运行，请使用**set-daentrypointdc** cmdlet 指向另一域控制器。<br />-从服务器上提升的命令提示符运行**gpresult** ，以确保服务器正在获取 DirectAccess 组策略对象。<br />-启用用户界面（UI）日志记录。<br />-使用以下命令启动 Windows PowerShell 日志记录：<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>-关闭然后重新打开用户界面。<br />-禁用 Windows Powershell 日志记录。 收集事件跟踪日志文件。 同时，收集 **% windir%/tracing**文件夹中的所有日志。|  
|应用 DirectAccess 配置失败|**刷新 DirectAccess 配置**<br />-如果对多站点部署进行故障排除，请确保最接近入口点的域控制器可用。<br />-使用**set-daentrypointdc** cmdlet 检索离入口点最近的域控制器的名称。 如果域控制器未运行，请使用**set-daentrypointdc** cmdlet 指向另一域控制器。<br />-使用以下命令启动 Windows Powershell 日志记录：<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-单击 "**应用**"。<br />-发生故障后，禁用 Windows Powershell 日志记录，并收集事件跟踪日志。|  
|DirectAccess 已配置，但客户端无法连接到内部资源|**排查客户端连接问题**<br />-单击远程访问管理控制台中的 "**操作状态**" 选项卡，并确保所有组件都显示一个绿色图标。 如果不是，请检查错误详细信息，然后按照解决方法步骤操作。<br />-运行远程访问服务器最佳做法分析器（BPA）。 如果出现任何警告或错误，请按照解决方法步骤来解决此问题。|  
|遇到与多站点配置相关的问题（例如，启用多站点、添加入口点或为入口点设置域控制器）|按照[对多站点部署进行疑难解答](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx)中的步骤操作。|  
|仪表板上的 "配置状态" 磁贴显示警告或错误|按照[监视远程访问服务器的配置分发状态](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx)中的步骤进行操作。|  
|遇到与配置负载平衡相关的问题（例如，启用负载平衡时配置失败，或者当你在群集中添加或删除服务器时出现问题）|如果你启用了负载平衡或添加节点，并且在你单击 "**应用**" 时刷新了配置，但群集未在服务器上正确地进行窗体，请运行以下命令： **cmd.exe/c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters/f/v DebugFlag/t REG_DWORD/d "" 0xffffffff "** " "，收集新服务器上的用户界面日志。|  
|"操作状态" 显示在执行以下步骤后出现的错误或警告，以更正此问题|如果操作状态显示不正确的信息（例如错误，即使在修复这些错误之后）：<br /><br />-启用注册表项**cmd.exe/c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters/f/V EnableTracing/T REG_DWORD/d" "5"** ""。<br />-刷新操作状态并从 **% windir%/tracing**收集日志。|  
|Windows 8 及更高版本的 DirectAccess 客户端计算机将报告 "无 Internet" 作为 DirectAccess 连接的状态，并且网络连接状态指示器（NCSI）报告限制的连接。|如果在 DirectAccess 配置中启用了强制隧道，则会发生这种情况，因此，只使用 IPHTTPS。 若要解决此问题，你可以创建和配置代理服务器。 然后，NCSI 使用代理服务器执行 Internet 连接检查。 建议使用以下过程将静态代理添加到名称解析策略表（NRPT）。<br /><br />在运行此过程中的命令之前，请确保将所有域名、计算机名称和其他 Windows PowerShell 命令变量替换为适用于你的部署的值。<br /><br />**为 NRPT 规则配置静态代理**<br />1.显示 "."NRPT 规则： `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2.记下 "." 的名称（GUID）。NRPT 规则。 名称（GUID）应以**DA-{...}** 开头<br />3.为 "." 设置代理。NRPT 规则到**proxy.corp.example.com:8080**： `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4.显示 "."NRPT 规则通过运行 `Get-DnsClientNrptRule`，并验证**ProxyFQDN： port**现在是否已正确配置。<br />5.刷新组策略，方法是在客户端内部连接时，在 DirectAccess 客户端上运行 `gpupdate /force`，然后使用 `Get-DnsClientNrptPolicy` 显示 NRPT，并验证 "." 规则是否显示**ProxyFQDN： port**。|  
  


