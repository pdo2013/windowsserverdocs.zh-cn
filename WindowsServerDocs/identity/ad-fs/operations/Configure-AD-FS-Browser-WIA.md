---
title: 通过 AD FS 将浏览器配置为使用 Windows 集成身份验证（WIA）
description: 本文档介绍如何将浏览器配置为将 WIA 与 AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c1413e4fa9d86b3c2204b9ed7337389437b93952
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865878"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>通过 AD FS 将浏览器配置为使用 Windows 集成身份验证（WIA）

默认情况下，在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务（AD FS）中启用 Windows 集成身份验证（WIA），以便在组织的内部网络（intranet）内发生身份验证请求，这些请求使用用于身份验证的浏览器。

AD FS 2016 现在提供了一个改进的默认设置，使边缘浏览器可以执行 WIA，同时也不会错误地捕获 Windows Phone：

    =~Windows\s*NT.*Edge

上述说明，你不再需要配置单独的用户代理字符串来支持常见边缘方案，即使它们很频繁地进行更新。

对于其他浏览器，请配置 AD FS 属性**WiaSupportedUserAgents** ，以根据所使用的浏览器添加所需的值。  你可以使用以下过程。



### <a name="view-wiasupporteduseragent-settings"></a>查看 WIASupportedUserAgent 设置
**WIASupportedUserAgents**定义了支持 WIA 的用户代理。 当在浏览器或浏览器控件中执行登录名时，AD FS 会分析用户代理字符串。

你可以使用以下 PowerShell 示例查看当前设置：

```powershell
    Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![WIA 支持](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>更改 WIASupportedUserAgent 设置
默认情况下，新的 AD FS 安装会创建一组用户代理字符串匹配项。 但是，这些更改可能基于对浏览器和设备的更改而过期。 尤其是，Windows 设备具有类似的用户代理字符串，这些字符串在令牌中具有较小的变化。 以下 Windows PowerShell 示例为当前在市场上支持无缝 WIA 的一组设备提供了最佳指导：

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

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
