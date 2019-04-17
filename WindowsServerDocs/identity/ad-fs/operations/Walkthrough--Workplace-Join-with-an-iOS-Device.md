---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 演练-iOS 设备的工作区加入
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a8643bab913dfec07c6bbea0c068e1240f16c5b
ms.sourcegitcommit: a2260c96b0e49519d180c7382b921ce8ddb053fe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>工作区参与 iOS 设备演练：

>适用于： Windows Server 2012 R2

本主题演示加入工作区 iOS 设备上。 你必须完成中的步骤[设置适用于在 Windows Server 2012 R2 的广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)之前，你可以试用此演练部分。 你可以使用设备来访问你访问在同一公司 web 应用程序[演练： 与 Windows 设备的工作区加入](Walkthrough--Workplace-Join-with-a-Windows-Device.md)。

## <a name="join-an-ios-device-with-workplace-join"></a>通过加入工作区加入 iOS 设备

> [!IMPORTANT]
> IOS 设备上本地 DRS 配置时，必须在信任用来配置了 Active Directory 联合身份验证服务 (广告 FS) 的安全套接字层 (SSL) 证书[第 2 步： 与设备注册服务配置联合服务器 (ADFS1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)的工作区加入成功。
> 
> -   如果广告 FS SSL 证书颁发从测试证书颁发机构），你必须在 iOS 设备上安装证书颁发机构证书。
> -   如果网站上发布你的证书颁发机构证书后，你可以从 iOS 设备浏览到该网站并安装证书。

在此演示，您将设备加入到工作区。

#### <a name="to-join-an-ios-device-to-a-workplace"></a>若要加入工作区 iOS 设备

1.  -   **Azure Active Directory 设备注册服务后配置的 DRS:**打开 Apple Safari，然后导航到用于 iOS 设备 Azure Active Directory 设备注册服务无线个人资料端点 <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > 了 <`yourdomainname`> 是使用 Azure Active Directory 配置你的域名。 例如，如果你的域名 contoso.com，URL 会进行以下操作： `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **当本地 DRS 是配置的 DRS**： 打开 Apple Safari，然后导航到的设备注册服务 (DRS) 无线个人资料端点 iOS 设备`https://adf1s.contoso.com/enrollmentserver/otaprofile`

    有多种方法通信此为你的用户的 URL。 推荐的一种方法是在自定义应用程序访问发布此 URL 拒绝广告 FS 中的消息。 这水域被即将推出的部分：[创建一个应用程序访问策略和自定义拒绝访问消息](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  登录到使用公司域帐户网页： ** roberth@contoso.com **和密码： ** P@ssword **。

3.  系统将提示你安装的个人资料。 在**安装的个人资料**屏幕上，单击**安装**。

4.  当提示你确认已安装的配置文件，请单击**立即安装**。

5.  如果你的设备需要 PIN 解锁设备，系统将提示你输入你的 pin 码。

6.  当你查看个人资料安装完成**个人资料安装**屏幕。 单击**完成**。

    返回到 Safari。 你可以关闭或离开 Safari 一条消息通知你。

> [!TIP]
> 若要查看或删除加入工作区的个人资料，浏览到**设置**，单击**常规**，然后单击**配置文件**iOS 设备上。

## <a name="see-also"></a>请参阅


- [加入工作区从任何设备 SSO 和无缝第二个跨公司应用程序因素身份验证](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [设置适用于 Windows Server 2012 R2 中广告 FS 实验环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [演练：与 Windows 设备的工作区加入](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



