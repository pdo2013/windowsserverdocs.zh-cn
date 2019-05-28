---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 演练-Android 设备的工作区加入
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73dbe4d62d460f9487467c7d4198d62b3b6af539
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188927"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>操作实例：Android 设备的工作区加入



## <a name="join-your-device-with-workplace-join"></a>使用“工作区加入”加入你的设备

> [!NOTE]
> Android 工作区加入要求 Azure Active Directory 设备注册服务。 若要强制实施条件性策略本地设备，必须启用设备对象回写式选项部署目录同步工具 (DirSync)。 在存在时，设备回写到 Active Directory 从 Azure Active Directory 可能需要最多为 3 个小时。 在这种情况下，用户必须等待 3 小时才能在创建工作帐户后访问的本地 web 应用程序。 有关部署 Azure Active Directory 设备注册服务，请参阅， [Azure Active Directory 设备注册服务概述](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>创建联接 workplace Join 的设备的工作帐户

1.  您需要创建与工作区加入加入你的设备的工作帐户在设备上安装 Azure 验证器应用程序。 以下 URL 中说明了如何在 Android 设备上安装 Azure 验证器应用并添加工作帐户。 工作帐户使你的 Android 设备变成受信任的设备，并向设备上的应用程序提供单一登录 (SSO)。 按照你的 IT 管理员的建议，可以使用受信任的设备访问 web 应用程序和现代业务线应用程序。 有关详细信息，请参阅[适用于 Android 的 Azure 验证器](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to)。

## <a name="see-also"></a>请参阅
[加入工作区以从任一设备实现 SSO 和无缝第二个身份 Authentication Across Company Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[设置本地条件性访问使用 Azure Active Directory 设备注册服务](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


