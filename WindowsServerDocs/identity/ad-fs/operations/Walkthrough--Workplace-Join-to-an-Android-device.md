---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: "演练-到 Android 设备的工作区加入"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Android 设备到演练： 工作区加入

>适用于：Windows Server 2016，Windows Server 2012 R2


## <a name="join-your-device-with-workplace-join"></a>通过加入工作区加入你的设备

> [!NOTE]
> Android 工作区加入需要 Azure Active Directory 设备注册服务。 为了加强条件的设备的策略本地，必须启用设备对象回写选项部署目录同步工具 （目录同步）。 目前，设备写回 Active Directory 从 Azure Active Directory 可能需要达 3 小时。 用户正因为如此，必须等待访问本地 web 应用程序，创建一个帐户，工作后的 3 小时。 有关部署 Azure Active Directory 设备注册的详细信息服务、 查看， [Azure Active Directory 设备注册服务概述](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>创建加入你的设备的工作区加入工作帐户

1.  你将需要在你的设备创建工作帐户，通过加入工作区加入你的设备上安装 Azure 验证器应用程序。 以下 URL 具有如何 Android 设备上安装 Azure 验证器应用和添加工作帐户的说明进行操作。 工作帐户插入受信任的设备使你 Android 设备，并提供在设备上的应用程序单一登录 (SSO)。 按照你的 IT 管理员的建议，你可以使用访问 web 应用程序和现代业务线应用程序受信任的设备。 有关详细信息，请参阅[Azure Authenticator 于 Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to)。

## <a name="see-also"></a>请参阅
[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[设置本地条件访问使用 Azure Active Directory 设备注册服务](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


