---
title: 配置浏览器与 AD FS 配合使用 Windows 集成身份验证 (WIA)
description: 本文档介绍如何配置为使用 AD FS WIA 的浏览器
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f71680bb721635bd37197dca9d3ae4726099525f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845478"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>配置浏览器与 AD FS 配合使用 Windows 集成身份验证 (WIA)

默认情况下，Active Directory 联合身份验证服务 (AD FS) 在 Windows Server 2012 R2 中的组织的内部网络 (intranet) 内发生的任何应用程序使用的身份验证请求中启用 Windows 集成身份验证 (WIA)其身份验证的浏览器。

AD FS 2016 现在具有允许 Edge 浏览器也不还 （错误地） 捕获的 Windows Phone 的同时执行 WIA 的改进了的默认设置：

    =~Windows\s*NT.*Edge

上面的意思，不再需要配置单独的用户代理字符串以支持常见的边缘情况下，即使它们经常更新。

对于其他浏览器，配置 AD FS 属性**WiaSupportedUserAgents**添加所需的值基于正在使用的浏览器。  可以使用以下过程。



### <a name="view-wiasupporteduseragent-settings"></a>查看 WIASupportedUserAgent 设置
**WIASupportedUserAgents**定义支持 WIA 的用户代理。 在浏览器或浏览器控件中执行的登录名时，AD FS 将分析用户代理字符串。

您可以查看当前设置使用以下 PowerShell 示例：

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![WIA 支持](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>更改 WIASupportedUserAgent 设置
默认情况下，安装新的 AD FS 具有一系列创建的用户代理字符串匹配项。 但是，这些可能是过期的基于浏览器和设备的更改。 具体来说，是 Windows 设备的令牌中具有类似用户代理字符串与次要变体。 以下 Windows PowerShell 示例提供了针对当前集是当今市场支持无缝 WIA 的设备的最佳指导：

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

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
