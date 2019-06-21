---
title: 一般问题疑难解答
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87614ac3b83eaacefb4ac5f9fddef238ed500953
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282553"
---
# <a name="troubleshooting-general-issues"></a>一般问题疑难解答

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题包含有关与远程访问相关的常规问题的疑难解答信息。  
  
## <a name="gpo-retrieval-error"></a>GPO 检索错误  
**收到错误**。 无法检索 DirectAccess 服务器 GPO 设置。 请确保拥有 gpo 的编辑权限。  
  
收到此错误后，远程访问管理控制台无响应。  
  
**原因**  
  
DirectAccess 无法访问 GPO 的部署中的入口点之一，因此无法加载配置。  
  
**解决方案**  
  
请确保部署中的每个入口点在其域控制器上具有相应的 GPO，并验证已登录用户具有读取和写入权限的远程访问部署中配置的所有 Gpo。  
  
作为一种解决方法，而不是使用远程访问管理控制台; 使用配置 cmdlet例如，使用`Get-RemoteAccess`和`Get-DAEntryPoint`。  
  
> [!NOTE]  
> 当前的入口点的服务器 GPO 不可用时，这种情况下不会发生。  
  
可以使用`Get-DAEntryPointDC`cmdlet 可列出所有存储服务器 Gpo 的域控制器并`Get-DAMultiSite`结合`Get-RemoteAccess`以检索部署中的服务器 Gpo 的完整列表。 例如：  
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>为 Windows 8 的 Windows 7 或 10 客户端升级  
**症状**。 Windows 7 客户端升级到 Windows 10 或 Windows 8 中的多站点部署后，DirectAccess 连接不是在网络列表中可见的。  
  
**原因**  
  
Windows 7 Gpo 在多站点部署中不包含 Windows 8 网络连接助手配置。  
  
 Windows 7 客户端应使用 DirectAccess 连接助手监视他们需要单独的手动配置 Windows 7 客户端 Gpo 中的 DirectAccess 连接状态。 在 Windows 7 客户端都升级到 Windows 10 或 Windows 8 中，网络连接助手将无法正常如果 Windows 7 客户端 GPO 仍应用。  
  
**解决方案**  
  
如果在 Windows 7 Gpo 中配置 DirectAccess 连接助手的设置，可以通过修改 Windows 7 Gpo 使用以下 PowerShell cmdlet 来升级客户端计算机之前解决此问题：  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
如果已升级客户端或未配置 DCA，将客户端计算机移动到 Windows 10 或 Windows 8 安全组。  
  
## <a name="general-cmdlet-errors"></a>常规 cmdlet 错误  
  
-   **问题 1**  
  
    **收到错误**。 不能为 < 服务器名称或 entry_point_name > 访问域控制器 < 域 _ 控制器 >。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 时管理的入口点的服务器 GPO 的域控制器不可用，无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    请按照"若要更改管理服务器 Gpo 的域控制器"中所述的过程[2.4。配置 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。  
  
-   **问题 2**  
  
    **收到错误**。 无法访问在 < 域名 > 中的主域控制器。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 客户端 GPO 由主域控制器进行管理。 如果主域控制器不可用，则无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    请按照"若要将 PDC 仿真器角色传输"中所述的过程[2.4。配置 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。  
  


