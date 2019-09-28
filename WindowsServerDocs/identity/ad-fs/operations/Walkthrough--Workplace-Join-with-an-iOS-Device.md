---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 演练-使用 iOS 设备 Workplace Join
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5214165c2843461a2da8b574ad28f92b0e0bc64d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407499"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>演练：使用 iOS 设备加入工作区


> [!IMPORTANT] 
> 此方法仅适用于完全本地的客户。 混合或仅限云的客户不得使用此方法来注册其 iOS 设备。 当本地客户决定迁移到云时，此方法不兼容。 设备必须注册并注册到云中。 

本主题在 iOS 设备上演示“工作区加入”。 你必须完成在[Windows Server 2012 R2 中设置 AD FS 实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)部分中的步骤，然后才能尝试此演练。 你可以使用该设备来访问在 @no__t 中访问的同一公司 web 应用程序0Walkthrough：使用 Windows 设备 @ no__t Workplace Join。


## <a name="join-an-ios-device-with-workplace-join"></a>使用“工作区加入”加入 iOS 设备

> [!IMPORTANT]
> 在配置本地 DRS 时，iOS 设备必须信任用于配置的安全套接字层（SSL）证书，该证书用于配置 [Step 2 中的 Active Directory 联合身份验证服务（AD FS）：配置联合服务器（ADFS1）与设备注册服务 @ no__t-0，以便 Workplace Join 成功。
> 
> -   如果 AD FS SSL 证书由测试证书颁发机构 (CA) 颁发，则必须在 iOS 设备上安装证书颁发机构证书。
> -   如果你的证书颁发机构证书已发布在网站上，可以从你的 iOS 设备浏览到该网站并安装该证书。

在此演示中，你将设备加入到工作区。

#### <a name="to-join-an-ios-device-to-a-workplace"></a>将 iOS 设备加入到工作区

1. -   **如果已配置的 DRS 是 Azure Active Directory 设备注册服务：** 打开 Apple Safari 并导航到 iOS 设备的 Azure Active Directory 设备注册服务无线配置文件终结点，< `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` >，其中 < @no__t > 为你使用 Azure Active Directory 配置的域名。 例如，如果你的域名为 contoso.com，则 URL 将为：`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **如果已配置的 DRS 是本地 DRS**：打开 Apple Safari 并导航到 iOS 设备的设备注册服务（DRS）无线配置文件终结点，`https://adf1s.contoso.com/enrollmentserver/otaprofile`

   将此 URL 传播给用户的方式有很多。 其中一种建议的方式是在 AD FS 中的自定义应用程序访问被拒消息中发布此 URL。 后面的部分中介绍了此方式：[创建应用程序访问策略和自定义访问被拒消息](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. 使用公司域帐户登录到网页： <strong>roberth@contoso.com</strong>和密码： <strong>P@ssword</strong>。

3. 系统会提示你安装配置文件。 在“安装配置文件” 屏幕上，单击“安装”。

4. 当系统提示确认安装配置文件时，请单击“立即安装”。

5. 如果你的设备需要一个 PIN 来解锁，系统会提示你输入 PIN。

6. 当你看到“配置文件已安装” 屏幕时，表示配置文件安装已完成。 单击 **“完成”** 。

   返回到 Safari。 显示一条消息，通知你可以关闭或退出 Safari。

> [!TIP]
> 若要查看或删除“工作区加入”配置文件，请浏览到“设置”，单击“常规”，然后单击 iOS 设备上的“配置文件”。

## <a name="see-also"></a>请参阅


- [跨公司应用程序从任一设备加入工作区以实现 SSO 和无缝第二重身份验证](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [演练：使用 Windows 设备加入工作区](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



