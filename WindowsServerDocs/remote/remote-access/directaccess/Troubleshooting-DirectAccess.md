---
title: DirectAccess 疑难解答
description: 本主题提供有关 Windows Server 2016 中的 DirectAccess 部署故障排除信息。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f49d9ab0e28e84cbb46015d50778653b35f5ea85
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281939"
---
# <a name="troubleshooting-directaccess"></a>DirectAccess 疑难解答

>适用于：Windows 服务器 （半年频道），Windows Server 2016

按照以下步骤来解决远程访问 (DirectAccess) 问题。  
  
|||  
|-|-|  
|**问题**|**解决方法**|  
|远程访问管理控制台不能显示 DirectAccess 配置|**若要还原缺少的配置信息**<br />-如果您多站点部署进行故障排除，请确保入口点最接近的域控制器是否可用。<br />-使用**Get DAEntrypointDC** cmdlet 来检索到的入口点最接近的域控制器的名称。 如果未运行的域控制器，使用**Set-daentrypointdc** cmdlet 为指向另一个域控制器。<br />-运行**gpresult**从提升的命令提示符以确保服务器是否正在使用 DirectAccess 组策略对象在服务器上。<br />-启用用户界面 (UI) 日志记录。<br />-使用以下命令以启动 Windows PowerShell 日志记录：<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>-关闭并重新打开用户界面。<br />-禁用 Windows Powershell 日志记录。 收集事件跟踪日志文件。 此外，收集所有日志 **%windir%/tracing**文件夹。|  
|在应用 DirectAccess 配置失败|**若要刷新的 DirectAccess 配置**<br />-如果您多站点部署进行故障排除，请确保入口点最接近的域控制器是否可用。<br />-使用**Get DAEntrypointDC** cmdlet 来检索到的入口点最接近的域控制器的名称。 如果未运行的域控制器，使用**Set-daentrypointdc** cmdlet 为指向另一个域控制器。<br />-使用以下命令以启动 Windows Powershell 日志记录：<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-单击**应用**。<br />-在故障发生后，禁用 Windows Powershell 日志记录，并收集事件跟踪日志。|  
|配置 DirectAccess，但客户端不能连接到内部资源|**若要对客户端连接问题进行故障排除**<br />-单击**操作状态**在远程访问管理控制台中，选项卡，并确保所有组件，都显示绿色图标。 如果没有，请查看错误详细信息，并按照的解决方法步骤。<br />-运行远程访问服务器最佳做法分析器 (BPA)。 如果有任何警告或错误，请按照来解决此问题的解决方法步骤。|  
|遇到与多站点配置 （例如，启用多站点、 添加入口点，或设置一个入口点域控制器） 相关的问题|按照中的步骤[多站点部署故障排除](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx)。|  
|配置状态磁贴的仪表板上显示的警告或错误|按照中的步骤[监视远程访问服务器的配置分发状态](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx)。|  
|遇到与配置负载平衡 （例如，配置会在启用负载平衡，或有问题时添加或从群集中删除服务器时失败） 相关的问题|如果已启用负载平衡或添加节点，并在配置刷新时单击了**Apply**，但群集未正确构成的服务器上，运行以下命令： **cmd.exe /c"reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v DebugFlag /t REG_DWORD /d""0xffffffff"""** 收集用户界面日志在新服务器上。|  
|以下步骤解决此问题后，操作状态将显示错误或警告|如果操作状态 （例如，错误，甚至在您修复这些） 显示不正确的信息：<br /><br />-   Enable the registry key **cmd.exe /c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v EnableTracing /t REG_DWORD /d ""5"" "** .<br />-刷新的操作状态和收集日志 **%windir%/tracing**。|  
|Windows 8 和更高版本的 DirectAccess 客户端计算机与 DirectAccess 连接的状态报告"无 Internet"和网络连接状态指示器 (NCSI) 报告有限的连接。|这可以在 DirectAccess 配置中启用强制隧道，因此，使用仅 IPHTTPS 发生。 若要解决此问题，可以创建和配置代理服务器。 NCSI 然后使用代理服务器进行 Internet 连接性检查。 它被建议您添加静态代理到名称解析策略表 (NRPT) 通过使用以下过程。<br /><br />在此过程中运行命令之前，请确保使用适合你的部署的值替换所有域名、 计算机名称和其他 Windows PowerShell 命令变量。<br /><br />**配置 NRPT 规则的静态代理**<br />1.显示"。"NRPT 规则： `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2.请记下名称 (GUID) 的"。"NRPT 规则。 名称 (GUID) 应以开头**DA-{。}**<br />3.设置的代理"。"NRPT 规则应用于**proxy.corp.example.com:8080**:  `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4.显示"。"通过再次运行的 NRPT 规则`Get-DnsClientNrptRule`，并确认**ProxyFQDN:port**现已正确配置。<br />5.通过运行刷新组策略`gpupdate /force`DirectAccess 客户端上当客户端连接时在内部，然后显示使用 NRPT `Get-DnsClientNrptPolicy` ，并验证"。"规则显示**ProxyFQDN:port**。|  
  


