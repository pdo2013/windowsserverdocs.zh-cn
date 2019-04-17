---
title: "配置使用与广告 FS Windows 集成身份验证 (WIA) 中的浏览器"
description: "本文介绍了如何配置浏览器与广告 FS 使用 WIA"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7d43931d1fe4958a539ff1b728e4cc154d06248
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>配置使用与广告 FS Windows 集成身份验证 (WIA) 中的浏览器

默认情况下，Windows 集成身份验证 (WIA) 中 Active Directory 联合身份验证服务 (广告 FS) 在 Windows Server 2012 R2 为启用组织的内部网络 (intranet) 的使用浏览器其身份验证的任何应用程序中发生的身份验证请求。

广告 FS 2016 现在已改进的默认设置，使在 Edge 浏览器，若要执行操作时不还 WIA（错误地）以及捕捉 Windows Phone:

    =~Windows\s*NT.*Edge

上述意味着你无需配置单个用户专员字符串支持常见 Edge 方案中，即使他们会经常更新也是如此。

对于其他浏览器，将配置广告 FS 属性**WiaSupportedUserAgents**添加所需的值基于你所使用的浏览器。  你可以使用下面的过程。



### <a name="view-wiasupporteduseragent-settings"></a>查看 WIASupportedUserAgent 设置
**WIASupportedUserAgents**定义都支持 WIA 用户代理。 广告 FS 分析用户代理字符串中的浏览器或浏览器控件执行登录时。

你可以查看使用下面的示例 PowerShell 的当前设置：

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![WIA 支持](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>更改 WIASupportedUserAgent 设置
默认情况下，新的广告 FS 安装有用户专员字符串匹配创建一的组。 但是，这些可能已过期根据浏览器与设备所做的更改。 特别是，Windows 设备具有次要变化与相似用户代理字符串在这些标记。 下面的 Windows PowerShell 示例提供支持的设备上的市场当今的无缝 WIA 套当前的最佳指南：

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

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
Windows NT 6.1;Trident/7.0</br></br>Windows NT 6.1;Win64;x64;Trident/7.0</br></br>Windows NT 6.1;WOW64;Trident/7.0|Windows 7 桌面操作系统，不同 platoforms|
MSIPC| Microsoft 信息保护和控制客户端|
Windows 权限管理的客户端|Windows 权限管理的客户端|
