---
title: 一般问题疑难解答
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2b8d7decad482ca8756aa4d82baa35abf16f5fe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404446"
---
# <a name="troubleshooting-general-issues"></a>一般问题疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题包含与远程访问相关的一般问题的疑难解答信息。  
  
## <a name="gpo-retrieval-error"></a>GPO 检索错误  
**接收到错误**。 无法检索 DirectAccess 服务器 GPO 设置。 确保你拥有 GPO 的编辑权限。  
  
远程访问管理控制台在收到此错误后无响应。  
  
**原因**  
  
DirectAccess 无法访问部署中某个入口点的 GPO，因此无法加载配置。  
  
**解决方案**  
  
请确保部署中的每个入口点在其域控制器上都有相应的 GPO，并验证登录用户对在远程访问部署中配置的所有 Gpo 是否拥有读取和写入权限。  
  
解决方法是使用配置 cmdlet，而不是使用远程访问管理控制台;例如，使用 @no__t 0 和 @no__t。  
  
> [!NOTE]  
> 当前入口点的服务器 GPO 不可用时，不会出现这种情况。  
  
你可以使用 `Get-DAEntryPointDC` cmdlet 列出存储服务器 Gpo 的所有域控制器，并将 `Get-DAMultiSite` 与 @no__t 2 一起使用，以便在部署中检索服务器 Gpo 的完整列表。 例如：  
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Windows 7 到 Windows 8 或10客户端升级  
**症状**。 Windows 7 客户端在多站点部署中升级到 Windows 10 或 Windows 8 后，DirectAccess 连接在网络列表中不可见。  
  
**原因**  
  
多站点部署中的 Windows 7 Gpo 不包含 Windows 8 网络连接助手配置。  
  
 Windows 7 客户端应使用 DirectAccess 连接助手来监视其 DirectAccess 连接状态，这需要在 Windows 7 客户端 Gpo 中进行单独的手动配置。 当 Windows 7 客户端升级到 Windows 10 或 Windows 8 时，如果仍应用 Windows 7 客户端 GPO，则网络连接助手将无法正常运行。  
  
**解决方案**  
  
如果在 Windows 7 Gpo 中配置了 DirectAccess 连接助手设置，则可以在升级客户端计算机之前使用以下 PowerShell cmdlet 来解决此问题：  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
如果客户端已升级或未配置 DCA，请将客户端计算机移动到 Windows 10 或 Windows 8 安全组。  
  
## <a name="general-cmdlet-errors"></a>常规 cmdlet 错误  
  
-   **问题1**  
  
    **接收到错误**。 对于 < server_name 或 entry_point_name >，无法访问域控制器 < domain_controller >。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 当管理入口点服务器 GPO 的域控制器不可用时，无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    按照 [2.4 中所述的过程 "更改管理服务器 Gpo 的域控制器"。配置 Gpo @ no__t。  
  
-   **问题2**  
  
    **接收到错误**。 无法访问域 < domain_name > 中的主域控制器。  
  
    **原因**  
  
    要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 客户端 GPO 由主域控制器进行管理。 如果主域控制器不可用，则无法读取或修改远程访问配置设置。  
  
    **解决方案**  
  
    按照 [2.4 中所述的 "传输 PDC 仿真器角色" 过程进行操作。配置 Gpo @ no__t。  
  


