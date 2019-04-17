---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: "配置设备不支持 WIA intranet 基于形式的身份的验证"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c78dc702cbff8eab487c6c077dfe57b0f0663342
ms.sourcegitcommit: 5012c078b410f59261edf0cc917393467243a5c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>配置设备不支持 WIA intranet 基于形式的身份的验证

>适用于：Windows Server 2016，Windows Server 2012 R2

默认情况下，Windows 集成身份验证 (WIA) 中 Active Directory 联合身份验证服务 (广告 FS) 在 Windows Server 2012 R2 为启用组织的内部网络 (intranet) 的使用浏览器其身份验证的任何应用程序中发生的身份验证请求。 例如，这些可以使用 WS 联盟基于浏览器的应用程序或 SAML 协议和丰富的应用程序使用的 OAuth 协议。 WIA 无需手动输入凭据为最终用户提供无缝登录应用程序。 但是，某些设备和浏览器都不支持 WIA 和这些设备从身份验证请求失败的结果。 此外，不需要 ntlm 协商某些浏览器上的体验。 建议的方法是回退到的此类设备和浏览器基于形式的身份验证。

在 Windows Server 2016 和 Windows Server 2012 R2 的广告 FS 将管理员提供配置的用户支持回退到基于形式的身份验证的代理列表的能力。 回退可通过两种配置：


- **WIASupportedUserAgentStrings**属性`Set-ADFSProperties`commandlet
- **WindowsIntegratedFallbackEnabled**属性`Set-AdfsGlobalAuthenticationPolicy`commmandlet

**WIASupportedUserAgentStrings**定义都支持 WIA 用户代理。 广告 FS 分析用户代理字符串中的浏览器或浏览器控件执行登录时。 如果用户代理字符串组件不符的任何配置在用户专员字符串组件**WIASupportedUserAgentStrings**属性，广告 FS 将使其回退提供基于形式的身份验证，提供了**WindowsIntegratedFallbackEnabled**标志设置为 True。

默认情况下，新的广告 FS 安装有用户专员字符串匹配创建一的组。 但是，这些可能已过期根据浏览器与设备所做的更改。 特别是，Windows 设备具有次要变化与相似用户代理字符串在这些标记。 下面的 Windows PowerShell 示例提供支持的设备上的市场当今的无缝 WIA 套当前的最佳指南：

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

上述命令将确保广告 FS 仅对于 WIA 涵盖以下的使用情况：

用户代理|使用情况|
-----|-----|
MSIE 6.0|IE 6.0|
MSIE 7.0;Windows NT|第 7 IE、 IE intranet 区域中。 "Windows NT"片段发送的桌面操作系统。|
MSIE 8.0|IE 8.0 （无设备发送此，因此需要进行更多具体）|
MSIE 9.0|IE 9.0 （没有设备发送此，不需要以使其更多具体）|
MSIE 10.0;Windows NT 6|对 Windows XP 和桌面操作系统的较新版本 IE 10.0</br></br>因为他们发送排除 （带有到移动设备设置首选项） 的 Windows Phone 8.0 设备</br></br>用户代理： Mozilla/5.0 （兼容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;IEMobile/10.0;ARM;触摸;NOKIA;Lumia 920)|
Windows NT 6.3;Trident/7.0</br></br>Windows NT 6.3;Win64;x64;Trident/7.0</br></br>Windows NT 6.3;WOW64;Trident/7.0| Windows 8.1 桌面操作系统，不同的平台|
Windows NT 6.2;Trident/7.0</br></br>Windows NT 6.2;Win64;x64;Trident/7.0</br></br>Windows NT 6.2;WOW64;Trident/7.0|Windows 8 桌面操作系统，不同的平台|
Windows NT 6.1;Trident/7.0</br></br>Windows NT 6.1;Win64;x64;Trident/7.0</br></br>Windows NT 6.1;WOW64;Trident/7.0|Windows 7 桌面操作系统，不同的平台|
MSIPC| Microsoft 信息保护和控制客户端|
Windows 权限管理的客户端|Windows 权限管理的客户端|

为了使回退到表单基于以外，述 WIASupportedUserAgents 字符串用户代理身份验证，设置为 true WindowsIntegratedFallbackEnabled 标志

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

此外请确保启用 intranet 了基于表单身份验证。

## <a name="configuring-wia-for-chrome"></a>为 Chrome 配置 WIA
你可以添加 Chrome 或其他用户代理支持 WIA 的广告 FS 配置。 无需手动输入凭据时访问受广告 FS 资源，这样，无缝登录应用程序。 请按照以下步骤来启用 WIA Chrome 上：

Chrome 广告 FS 配置中添加用户代理字符串

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
确认 Chrome 用户代理字符串现已设置中广告 FS 属性

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![配置 auth](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> 当发布新的浏览器和设备时，建议你协调这些用户代理的功能，并更新广告 FS 配置相应地优化用户的身份验证的体验，在使用所说的浏览器和设备。 具体而言，建议你重新评估**WIASupportedUserAgents**为 WIA 到支持列表中添加了新的设备或浏览器类型时，在广告 FS 设置。


