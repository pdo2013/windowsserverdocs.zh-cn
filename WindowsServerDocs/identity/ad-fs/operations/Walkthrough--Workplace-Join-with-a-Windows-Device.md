---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: 演练-使用 Windows 设备加入工作区
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b3b2934e7aa177e873e19d77530b2d796ccd521
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188901"
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>操作实例：使用 Windows 设备加入工作区

本主题演示如何使用“工作区加入”将你的 Windows 设备与工作区连接以及如何通过使用单一登录访问 Web 应用程序。 必须完成中的步骤[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)部分之前可以尝试此操作实例。

## <a name="access-the-web-application-before-device-registration"></a>在设备注册前访问 Web 应用程序
在本操作实例中，你在将设备加入工作区前访问公司 Web 应用程序。 该网页显示你的安全令牌中所包括的声明。 请注意声明列表不包括有关你的设备的任何信息。 你还可能会发现你不具有单一登录。

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>在你的设备上使用“工作区加入”前访问 Web 应用程序

1.  请使用你的 Microsoft 帐户登录到 Client1。

2.  打开 Internet Explorer 并浏览到你的通用声明应用**https://webserv1.contoso.com/claimapp**。

3.  登录到网页使用公司的域帐户： **roberth@contoso.com**，密码： **P@ssword**。

4.  该网页列出你的安全令牌中的所有声明。 在你的安全令牌中仅存在用户声明。

5.  关闭 Internet Explorer。

6.  打开 Internet Explorer 并导航到相同的声明应用**https://webserv1.contoso.com/claimapp**。

7.  请注意，系统会提示再次输入你的凭据。 你无法从使用“工作区加入”的设备连接到工作区，因此不具备单一登录。

## <a name="join-your-device-with-workplace-join"></a>使用“工作区加入”加入你的设备

> [!IMPORTANT]
> 若要成功执行工作区加入，客户端计算机 (Client1) 必须信任用于配置 Active Directory 联合身份验证服务 (AD FS) 的 SSL 证书中[步骤 2:使用 Device Registration Service (ADFS1 上) 配置联合身份验证服务器](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)。 它还必须能够验证该证书的吊销信息。 如果在执行工作区加入时遇到任何问题，可以查看 Client1 上的事件日志。
> 
> 若要查看事件日志，请打开事件查看器，依次展开“应用程序和服务日志”  、“Microsoft”  和“Windows”  ，然后单击“工作区加入”  。

#### <a name="to-join-your-device-with-workplace-join"></a>使用“工作区加入”加入你的设备

1.  请使用你的 Microsoft 帐户登录到 Client1。

2.  在“开始”  屏幕上，打开“超级按钮”  栏，然后选择“设置”  超级按钮。 选择“更改电脑设置”  。

3.  在“电脑设置”  页面上，选择“网络”  ，然后单击“工作区”  。

4.  在中**输入用户 Id 以获取工作区访问权限或打开设备管理**框中，键入**roberth@contoso.com**，然后单击**加入**。

5.  系统提示输入凭据，键入**roberth@contoso.com**，和密码： **P@ssword**。 单击 **“确定”** 。

6.  现在，你应看到如下消息：“此设备已加入工作区网络。”

### <a name="access-the-web-application-after-joining-the-workplace"></a>加入工作区后访问 Web 应用程序
在此部分的演示中，从与“工作区加入”连接的设备访问公司 Web 应用程序。 该网页显示你的安全令牌中所包括的声明。 请注意声明列表同时包括设备和用户信息。 你还可能会发现你现在具有单一登录。

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>在加入工作区后访问 Web 应用程序

1.  请使用你的 Microsoft 帐户登录到“Client1”  。

2.  打开 Internet Explorer 并浏览到你的通用声明应用**https://webserv1.contoso.com/claimapp**。

3.  登录到网页使用公司的域帐户： **roberth@contoso.com**，密码： **P@ssword**。

4.  该网页列出你的安全令牌中的声明。 你的令牌同时包含用户和设备声明。

5.  关闭 Internet Explorer。

6.  打开 Internet Explorer 并导航到相同的声明应用**https://webserv1.contoso.com/claimapp**。

7.  请注意，系统 **不会** 提示再次输入你的凭据。 你从使用“工作区加入”的设备连接，因此具有单一登录。

## <a name="see-also"></a>请参阅
[加入工作区以从任一设备实现 SSO 和无缝第二个身份 Authentication Across Company Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
 [演练：使用 iOS 设备加入工作区](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



