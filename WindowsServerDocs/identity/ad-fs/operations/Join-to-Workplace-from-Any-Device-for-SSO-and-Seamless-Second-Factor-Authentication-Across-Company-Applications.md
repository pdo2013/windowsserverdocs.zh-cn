---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: 跨公司应用程序从任一设备加入工作区以实现 SSO 和无缝第二重身份验证
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0ee22afd6fdec96ab69d915e4730bb834d2ab8ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855518"
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>跨公司应用程序从任一设备加入工作区以实现 SSO 和无缝第二重身份验证

>适用于：Windows Server 2012 R2

消费型电子设备和无处不在的信息访问量的快速增长正在改变人们看待技术的方式。 全天对信息技术的不断使用以及容易获取的信息正在模糊工作与家庭生活之间的传统界限。 这些转变的界限都伴随相信该个人技术选择、 自定义以满足用户的个性、 活动和计划-应延伸到工作区。 为了满足不断增长的个人消费型电子设备连接到企业网络的需求，我们引入了以下价值主张：

-   管理员可以控制谁有权访问公司资源，而这些资源基于应用程序、用户、设备和位置。

-   员工可以在任何设备上随处访问应用程序和数据。 员工可以在浏览器应用程序或企业应用程序中使用单一登录。

## <a name="key-concepts-introduced-in-the-solution"></a>在解决方案中引入的主要概念

### <a name="workplace-join"></a>工作区加入
通过使用工作区加入，信息工作者可以使用公司的工作区计算机加入他们的个人设备以访问公司资源和服务。 当你将自己的个人设备加入工作区时，它将成为已知设备并提供工作区资源和应用程序的无缝第二重身份验证和单一登录。 当通过“加入工作区”加入设备后，为了向应用程序授权颁发安全令牌，可从该目录检索设备的属性以推动有条件的访问。 可使用“工作区加入”加入 Windows 8.1 和 iOS 6.0+ 以及 Android 4.0+ 设备。

### <a name="BKMK_DRS"></a>Azure Active Directory 设备注册服务
Azure Active Directory 设备注册服务使“工作区加入”成为可能。 当通过“工作区加入”加入设备时，该服务会在 Active Directory 中配置设备对象，然后在本地设备上设置用于表示设备标识的键。 此设备标识随后可以与访问控制规则一起用于在云中和本地托管的应用程序。

有关更多详细信息，请参阅[Azure Active Directory 中的设备管理简介](https://docs.microsoft.com/azure/active-directory/device-management-introduction)。

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>工作区加入作为无缝第二重身份验证
公司可以管理与信息访问相关的风险并推动治理和合规性的建设，同时授予消费型电子设备访问公司资源的权限。 设备上的工作区加入为管理员提供了以下功能：

-   使用设备身份验证标识已知设备。 管理员可以使用此信息为资源创造条件性访问和控制访问。

-   从受信任的设备访问公司资源时，为用户提供更无缝的登录体验。

### <a name="single-sign-on"></a>单一登录
单一登录 (SSO) 在此情况下的上下文中是指这样的功能：该功能可减少最终用户为了从已知设备访问公司资源而必须输入密码的提示次数。 此功能意味着从该设备访问公司应用程序和资源时，在 SSO 生存期内只对用户进行一次提示。 如果某个设备使用工作区加入，则已注册使用此设备的用户将获取持续的 SSO，默认情况下为七天。 此用户在同一会话或新会话中拥有无缝登录体验。

## <a name="solution-overview"></a>解决方案概述
作为此解决方案的一部分，你将了解如何在支持的设备上使用“工作区加入”，以及体验对公司资源的单一登录。

> [!NOTE]
> 若要支持 Windows 8.1、iOS 6.0+ 和 Android 4.0+ 设备，必须配置 Azure Active Directory 设备注册配置以及设备对象回写，请参阅 [使用 Azure Active Directory 设备注册服务进行本地条件访问的分步指南](https://msdn.microsoft.com/library/azure/dn788908.aspx)

此解决方案指南将指导你完成以下操作实例的步骤：

1.  [演练：使用 Windows 设备加入工作区](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [演练：使用 iOS 设备加入工作区](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [演练：使用 Android 设备加入工作区](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>请参阅
[使用设备注册服务配置联合身份验证服务器](../deployment/configure-a-federation-server-with-device-registration-service.md)



