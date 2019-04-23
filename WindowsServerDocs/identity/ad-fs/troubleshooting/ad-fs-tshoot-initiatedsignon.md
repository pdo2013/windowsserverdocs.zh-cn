---
title: AD FS 故障排除-Idp 发起的登录
description: 本文档介绍如何排查 AD FS 登录页面。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 61e9adc708e95a6ab4a82550280737b2f4bad0a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844938"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS 故障排除-Idp 发起的登录
AD FS 登录页可用于测试使用身份验证。  这可通过导航到页面并登录。  此外，我们可以使用登录页以验证列出了所有 SAML 2.0 信赖方。

## <a name="enable-the-idp-intiated-sign-on-page"></a>启用 Idp 采取登录页面
默认情况下，AD FS Windows 2016 中的没有符号在页上启用。  若要启用它可以使用 PowerShell 命令 Set-adfsproperties。  使用以下过程来启用页：

1.  打开 Windows PowerShell
2.  输入：`Get-AdfsProperties`并按 enter
3.  确认**EnableIdpInitiatedSignonPage**设置为 false ![False](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  在 PowerShell 中，输入：  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  你将看不到一条确认消息因此再次输入 Get-adfsproperties 并确认**EnableIdpInitatedSignonPage**设置为 true。
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>测试身份验证
使用以下过程来测试与 Idp-Initiated 登录页面的 AD FS 身份验证。

1.  打开 web 浏览器并导航到 Idp 登录页。  示例：  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  系统应该会提示登录。  输入你的凭据。
![单一登录](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  如果这是成功，应登录。


## <a name="test-authentication-using-a-seamless-logon-experience"></a>测试身份验证使用无缝登录体验
可以通过确保 AD FS 服务器的 URL 将添加的 internet 选项本地 intranet 区域测试无缝登录体验。  请执行以下步骤：

1.  在 Windows 10 客户端，单击开始并键入 internet 选项，选择 internet 选项。
2.   单击安全选项卡，单击本地 intranet 上，单击站点按钮。
![无缝](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  单击“高级”。
2.  输入你的 url，然后单击添加。  单击关闭。
![添加 url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  单击确定。  单击确定。  这应关闭 internet 选项。
2.  打开 web 浏览器并导航到 Idp 登录页。  示例：  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  单击登录按钮。  您应自动登录，并不会提示输入凭据。
![无缝](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)