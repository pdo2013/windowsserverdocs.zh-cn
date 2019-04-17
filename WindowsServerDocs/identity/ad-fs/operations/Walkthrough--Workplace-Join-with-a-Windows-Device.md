---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: "演练-与 Windows 设备的工作区加入"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fd222eb47982591e051594e8a572443b65c0357f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>演练：与 Windows 设备的工作区加入

>适用于：Windows Server 2016，Windows Server 2012 R2

本主题演示如何用于工作区加入连接你的 Windows 设备与你的工作区以及如何访问 web 应用程序通过单一登录。 你必须完成中的步骤[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)之前，你可以试用此演练部分。

## <a name="access-the-web-application-before-device-registration"></a>注册设备之前的 web 应用程序访问
在此演练，然后再将你的设备连接到工作区访问公司 web 应用程序。 网页显示安全令牌中包含的索赔。 注意到的索赔列表中不包括你的设备有关的任何信息。 你可能还会观察您不具有单一登录。

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>使用之前工作区加入你的设备上访问 web 应用程序

1.  使用你的 Microsoft 帐户登录到客户端 1。

2.  打开 Internet Explorer 并浏览到你的索赔通用应用**https://webserv1.contoso.com/claimapp**。

3.  登录到使用公司域帐户网页： ** roberth@contoso.com **，密码： ** P@ssword **。

4.  网页列出安全令牌中的所有声明。 仅用户索赔均存在安全令牌中。

5.  关闭 Internet Explorer。

6.  打开 Internet Explorer，然后导航到同一索赔应用， **https://webserv1.contoso.com/claimapp**。

7.  请注意，提示你重新输入凭据。 你未从通过加入工作区设备连接到工作区，因此不将单一登录。

## <a name="join-your-device-with-workplace-join"></a>通过加入工作区加入你的设备

> [!IMPORTANT]
> 对于工作区加入成功的客户端计算机 (客户端 1) 必须信任用来配置了 Active Directory 联合身份验证服务 (广告 FS) SSL 证书中[第 2 步： 与设备注册服务 (ADFS1) 配置联合服务器](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)。 它还必须无法验证的证书撤销信息。 如果你有任何问题，通过加入工作区，你可以在客户端 1 查看事件日志。
> 
> 若要查看事件日志，请打开事件查看器，展开**应用程序和服务日志**，展开**Microsoft**，展开**Windows**，然后单击**加入工作区**。

#### <a name="to-join-your-device-with-workplace-join"></a>若要加入你的设备，通过加入工作区

1.  使用你的 Microsoft 帐户登录到客户端 1。

2.  在**开始**屏幕上，打开**超级按钮**栏，然后依次选择**设置**超级按钮。 选择**更改电脑设置**。

3.  在**电脑设置**页上，选择**网络**，然后单击**工作区**。

4.  在**输入你的用户 Id，以获取工作区访问或打开设备管理**框中，键入** roberth@contoso.com **，然后单击**加入**。

5.  当系统提示您输入凭据时，键入** roberth@contoso.com **，和密码： ** P@ssword **。 单击**确定**。

6.  你现在应该会看到一条消息:"此设备已加入你的工作区网络"。

### <a name="access-the-web-application-after-joining-the-workplace"></a>访问加入工作区后的 web 应用程序
在此部分中的演示，您访问来自你加入工作区与已连接的设备公司 web 应用程序。 网页显示安全令牌中包含的索赔。 注意到列表中索赔，包括设备和用户的信息。 你可能还会观察现在有单一登录。

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>若要访问后加入工作区的 web 应用程序

1.  登录到**客户端 1**与你的 Microsoft 帐户。

2.  打开 Internet Explorer 并浏览到你的索赔通用应用**https://webserv1.contoso.com/claimapp**。

3.  登录到使用公司域帐户网页： ** roberth@contoso.com **，密码： ** P@ssword **。

4.  网页列出安全令牌中的索赔。 你令牌包含用户和设备的索赔。

5.  关闭 Internet Explorer。

6.  打开 Internet Explorer，然后导航到同一索赔应用， **https://webserv1.contoso.com/claimapp**。

7.  请注意，你均**不**提示你重新输入凭据。 你从通过加入工作区设备连接，，因此将单一登录。

## <a name="see-also"></a>请参阅
[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
[演练： iOS 设备通过加入工作区](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



