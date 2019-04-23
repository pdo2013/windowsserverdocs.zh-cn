---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: 配置 intranet 基于窗体的身份验证不支持 WIA 的设备
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cddc5d890114dec7e0053b16701db6f03c3cbbdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889848"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>配置 intranet 基于窗体的身份验证不支持 WIA 的设备

>适用于：Windows Server 2016, Windows Server 2012 R2

默认情况下，Active Directory 联合身份验证服务 (AD FS) 在 Windows Server 2012 R2 中的组织的内部网络 (intranet) 内发生的任何应用程序使用的身份验证请求中启用 Windows 集成身份验证 (WIA)其身份验证的浏览器。 例如，这些可以是基于浏览器的应用程序使用 WS 联合身份验证或 SAML 协议和丰富的应用程序使用 OAuth 协议。 WIA 为最终用户提供无缝登录到应用程序，而不必手动输入其凭据。 但是，某些设备和浏览器不能够支持 WIA，因此从这些设备的身份验证请求失败。 此外，不应在协商到 NTLM 某些浏览器上的体验。 建议的方法是回退到此类设备和浏览器的基于窗体的身份验证。

Windows Server 2016 和 Windows Server 2012 R2 中的 AD FS 为管理员提供配置支持回退到基于窗体的身份验证的用户代理列表中的功能。 回退可通过两种配置：


- **WIASupportedUserAgentStrings**属性的`Set-ADFSProperties`commandlet
- **WindowsIntegratedFallbackEnabled**属性的`Set-AdfsGlobalAuthenticationPolicy`commandlet

**WIASupportedUserAgentStrings**定义支持 WIA 的用户代理。 在浏览器或浏览器控件中执行的登录名时，AD FS 将分析用户代理字符串。 如果用户代理字符串的组件不匹配任何配置中的用户代理字符串组成部分**WIASupportedUserAgentStrings**属性，AD FS 将回退到提供基于表单的身份验证，前提**WindowsIntegratedFallbackEnabled**标志设置为 True。

默认情况下，安装新的 AD FS 具有一系列创建的用户代理字符串匹配项。 但是，这些可能是过期的基于浏览器和设备的更改。 具体来说，是 Windows 设备的令牌中具有类似用户代理字符串与次要变体。 以下 Windows PowerShell 示例提供了针对当前集是当今市场支持无缝 WIA 的设备的最佳指导：

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

上面的命令将确保 AD FS 仅对于 WIA 涉及以下用例：

用户代理|用例|
-----|-----|
MSIE 6.0|IE 6.0|
MSIE 7.0; Windows NT|IE 7，IE intranet 区域中。 "Windows NT"片段桌面操作系统，即可发送。|
MSIE 8.0|IE 8.0 （没有设备发送这，因此需要进行更具体）|
MSIE 9.0|IE 9.0 （没有设备发送，因此不需要将此更具体）|
MSIE 10.0; Windows NT 6|Windows XP 和更高版本的桌面操作系统的 IE 10.0</br></br>排除 Windows Phone 8.0 设备 （具有设置到移动设备的首选项），因为它们会将发送</br></br>用户代理：Mozilla/5.0 （兼容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;IEMobile/10.0;ARM;接触;NOKIA;Lumia 920)|
Windows NT 6.3;Trident/7.0</br></br>Windows NT 6.3;Win64;x64;Trident/7.0</br></br>Windows NT 6.3;WOW64;Trident/7.0| Windows 8.1 桌面操作系统，不同的平台|
Windows NT 6.2;Trident/7.0</br></br>Windows NT 6.2;Win64;x64;Trident/7.0</br></br>Windows NT 6.2;WOW64;Trident/7.0|Windows 8 桌面操作系统，不同的平台|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1;Win64;x64;Trident/7.0</br></br>Windows NT 6.1;WOW64;Trident/7.0|Windows 7 桌面操作系统，不同的平台|
“MSIPC”| Microsoft 信息保护和控制客户端|
Windows 权限管理客户端|Windows 权限管理客户端|

若要启用回退到 WIASupportedUserAgents 字符串中提及的其他用户代理的基于窗体身份验证，将 WindowsIntegratedFallbackEnabled 标志设置为 true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

此外请确保 intranet 中启用了基于窗体身份验证。

## <a name="configuring-wia-for-chrome"></a>为 Chrome 配置 WIA
可以向支持 WIA 的 AD FS 配置中添加 Chrome 或其他用户代理。 这样，无缝登录到应用程序而无需访问 AD FS 保护的资源时手动输入凭据。 请执行以下步骤来启用 WIA 在 Chrome 上：

在 AD FS 配置中为 Chrome 添加用户代理字符串

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
确认现在中 AD FS 属性设置为 Chrome 用户代理字符串

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![配置身份验证](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> 发布新的浏览器和设备，建议您协调这些用户代理的功能，并相应地更新 AD FS 配置时使用浏览器和设备所说优化用户的身份验证体验。 具体而言，建议重新评估**WIASupportedUserAgents**为 WIA 到您的支持矩阵中添加新设备或浏览器类型时，在 AD FS 中设置。


