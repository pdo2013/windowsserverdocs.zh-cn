---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: 为不支持 WIA 的设备配置基于 intranet 窗体的身份验证
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9b74c57059346e87c5091c83d648b034f4cd049e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358077"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>为不支持 WIA 的设备配置基于 intranet 窗体的身份验证


默认情况下，在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务（AD FS）中启用 Windows 集成身份验证（WIA），以便在组织的内部网络（intranet）内发生身份验证请求，这些请求使用用于身份验证的浏览器。 例如，这些应用程序可以是基于浏览器的应用程序，它们使用 WS 联合身份验证或 SAML 协议以及使用 OAuth 协议的丰富应用程序。 WIA 向最终用户提供无缝登录到应用程序，而无需手动输入其凭据。 但是，某些设备和浏览器不能支持 WIA，因此这些设备发出的身份验证请求会失败。 此外，在某些与 NTLM 协商的浏览器上的体验并不理想。 推荐的方法是回退到此类设备和浏览器的基于窗体的身份验证。

Windows Server 2016 和 Windows Server 2012 R2 中的 AD FS 使管理员能够配置支持回退到基于窗体的身份验证的用户代理列表。 可以通过两个配置进行回退：


- `Set-ADFSProperties` Commandlet 的**WIASupportedUserAgentStrings**属性
- `Set-AdfsGlobalAuthenticationPolicy` Commandlet 的**WindowsIntegratedFallbackEnabled**属性

**WIASupportedUserAgentStrings**定义了支持 WIA 的用户代理。 当在浏览器或浏览器控件中执行登录名时，AD FS 会分析用户代理字符串。 如果用户代理字符串的组件与在**WIASupportedUserAgentStrings**属性中配置的用户代理字符串的任何组件都不匹配，AD FS 将回退到提供基于窗体的身份验证，前提是**WindowsIntegratedFallbackEnabled**标志设置为 True。

默认情况下，新的 AD FS 安装会创建一组用户代理字符串匹配项。 但是，这些更改可能基于对浏览器和设备的更改而过期。 尤其是，Windows 设备具有类似的用户代理字符串，这些字符串在令牌中具有较小的变化。 以下 Windows PowerShell 示例为当前在市场上支持无缝 WIA 的一组设备提供了最佳指导：

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

上述命令将确保 AD FS 仅涵盖 WIA 的以下用例：

用户代理|用例|
-----|-----|
MSIE 6。0|IE 6。0|
MSIE 7.0;Windows NT|IE 7、IE 在 intranet 区域中。 桌面操作系统发送 "Windows NT" 片段。|
MSIE 8。0|IE 8.0 （无设备发送此信息，因此需要更具体的信息）|
MSIE 9。0|IE 9.0 （无设备发送此信息，因此无需再进行此操作）|
MSIE 10.0;Windows NT 6|适用于 Windows XP 和更高版本的桌面操作系统的 IE 10。0</br></br>将排除 Windows Phone 8.0 设备（将首选项设置为 "移动"），因为它们发送</br></br>用户代理：Mozilla/5.0 （兼容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;IEMobile/10.0;单臂接触NOKIALumia 920）|
Windows NT 6.3;Trident/7。0</br></br>Windows NT 6.3;Win6464Trident/7。0</br></br>Windows NT 6.3;WOW64Trident/7。0| Windows 8.1 桌面操作系统，不同的平台|
Windows NT 6.2;Trident/7。0</br></br>Windows NT 6.2;Win6464Trident/7。0</br></br>Windows NT 6.2;WOW64Trident/7。0|Windows 8 桌面操作系统，不同平台|
Windows NT 6.1;Trident/7。0</br></br>Windows NT 6.1;Win6464Trident/7。0</br></br>Windows NT 6.1;WOW64Trident/7。0|Windows 7 桌面操作系统，不同平台|
“MSIPC”| Microsoft Information Protection and Control 客户端|
Windows Rights Management 客户端|Windows Rights Management 客户端|

若要为用户代理启用回退到基于窗体的身份验证，而不是在 WIASupportedUserAgents 字符串中提到的身份验证，请将 WindowsIntegratedFallbackEnabled 标志设置为 true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

还要确保为 intranet 启用了基于窗体的身份验证。

## <a name="configuring-wia-for-chrome"></a>为 Chrome 配置 WIA
你可以将 Chrome 或其他用户代理添加到支持 WIA 的 AD FS 配置。 这样就可以无缝登录到应用程序，而无需在访问受 AD FS 保护的资源时手动输入凭据。 按照以下步骤在 Chrome 上启用 WIA：

在 AD FS 配置中，在基于 Windows 的平台上添加 Chrome 的用户代理字符串：

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Windows NT")

同样，在 Apple macOS 上，将以下用户代理字符串添加到 AD FS 配置中：

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Macintosh; Intel Mac OS X")

确认 "Chrome" 的用户代理字符串现在已在 "AD FS 属性" 中设置：

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

（此处需要一个新屏幕快照）![配置身份验证](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> 当新浏览器和设备发布时，建议您协调这些用户代理的功能，并相应地更新 AD FS 配置，以优化用户使用的浏览器和设备时的身份验证体验。 更具体地说，建议您在将新设备或浏览器类型添加到 WIA 的支持矩阵时，重新评估 AD FS 中的**WIASupportedUserAgents**设置。


