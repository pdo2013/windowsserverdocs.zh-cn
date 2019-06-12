---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 演练-使用 iOS 设备加入工作区
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 979802469737066612bc6242f942fd3acd077479
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444790"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>操作实例：使用 iOS 设备加入工作区


> [!IMPORTANT] 
> 此方法非常适用于仅完全在本地客户。 混合或仅限云的客户必须使用此方法来注册其 iOS 设备。 并在本地客户决定移动到云时，此方法不兼容。 必须取消注册设备，并将其注册到云中。 

本主题在 iOS 设备上演示“工作区加入”。 必须完成中的步骤[为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)部分之前可以尝试此操作实例。 可以使用设备访问相同的公司 web 应用程序中访问[演练：使用 Windows 设备加入工作区](Walkthrough--Workplace-Join-with-a-Windows-Device.md)。


## <a name="join-an-ios-device-with-workplace-join"></a>使用“工作区加入”加入 iOS 设备

> [!IMPORTANT]
> 配置本地 DRS 时，iOS 设备必须信任用于配置 Active Directory 联合身份验证服务 (AD FS) 的安全套接字层 (SSL) 证书在[步骤 2:使用设备注册服务配置联合身份验证服务器 (ADFS1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)，才能成功执行工作区加入。
> 
> -   如果 AD FS SSL 证书由测试证书颁发机构 (CA) 颁发，则必须在 iOS 设备上安装证书颁发机构证书。
> -   如果你的证书颁发机构证书已发布在网站上，可以从你的 iOS 设备浏览到该网站并安装该证书。

在此演示中，你将设备加入到工作区。

#### <a name="to-join-an-ios-device-to-a-workplace"></a>将 iOS 设备加入到工作区

1. -   **如果已配置的 DRS 是 Azure Active Directory 设备注册服务：** 打开 Apple Safari 并导航到 iOS 设备的 Azure Active Directory 设备注册服务无线配置文件终结点 <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` >，<`yourdomainname`> 是你使用 Azure Active Directory 配置的域名。 例如，如果你的域名为 contoso.com，则 URL 将为：`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **如果已配置的 DRS 是本地 DRS**：打开 Apple Safari 并导航到 iOS 设备的设备注册服务 (DRS) 无线配置文件终结点 `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   将此 URL 传播给用户的方式有很多。 其中一种建议的方式是在 AD FS 中的自定义应用程序访问被拒消息中发布此 URL。 后面的部分中介绍了此方式：[创建应用程序访问策略和自定义访问被拒消息](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. 登录到网页使用公司的域帐户： <strong>roberth@contoso.com</strong>和密码： <strong>P@ssword</strong>。

3. 系统会提示你安装配置文件。 在“安装配置文件”  屏幕上，单击“安装”  。

4. 当系统提示确认安装配置文件时，请单击“立即安装”  。

5. 如果你的设备需要一个 PIN 来解锁，系统会提示你输入 PIN。

6. 当你看到“配置文件已安装”  屏幕时，表示配置文件安装已完成。 单击 **“完成”** 。

   返回到 Safari。 显示一条消息，通知你可以关闭或退出 Safari。

> [!TIP]
> 若要查看或删除“工作区加入”配置文件，请浏览到“设置”  ，单击“常规”  ，然后单击 iOS 设备上的“配置文件”  。

## <a name="see-also"></a>请参阅


- [工作区从任何设备加入实现 SSO 和无缝第二重身份验证跨公司应用程序](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [为 Windows Server 2012 R2 中的 AD FS 设置实验室环境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [演练：使用 Windows 设备加入工作区](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



