---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: 加入工作区从任何设备 SSO 和无缝第二个跨公司应用程序因素身份验证
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4926eb32a0bbffb092ec02ca2508fe97d52d1466
ms.sourcegitcommit: 46439194e5deb0fa5f338b428f95dd6b5b799337
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2018
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>加入工作区从任何设备 SSO 和无缝第二个跨公司应用程序因素身份验证

>适用于： Windows Server 2012 R2

消费者设备和普遍信息访问的数字快速增加正在发生变化人脉感知其技术的方式。 全天，轻松访问的信息，以及信息技术常使用模糊工作和生活家庭之间的传统限制。 这些转移限制附有相信技术选中和自定义为贴合用户的个性、活动和日程安排该个人-应延伸到工作区。 为达到可以连接到的企业网络个人消费者设备日益要求，我们引入以下值主张：

-   管理员可以控制哪些人有权根据应用、 用户、 设备和位置的公司资源。

-   员工可以访问在任何设备上的应用程序和 everywhere，数据。 员工可以使用单一登录，在浏览器应用程序或企业版的应用程序。

## <a name="key-concepts-introduced-in-the-solution"></a>主要的概念解决方案中引入了

### <a name="workplace-join"></a>加入工作区
通过加入工作区信息员工都可以加入他们的个人设备与他们的公司的工作区计算机公司资源和服务的访问。 当你到工作区加入你的个人设备时，它变得已知的设备，并提供无缝第二个因素身份验证和单一登录到工作区资源和应用程序。 当通过加入工作区加入某台设备时，可以驱动器条件访问对于授权的应用程序的安全令牌颁发目录中检索的设备属性。 通过加入工作区，可以加入 Windows 8.1 和 iOS 6.0 + 以及 Android 4.0 + 设备。

### <a name="BKMK_DRS"></a>Azure Active Directory 注册设备的服务
加入工作区成为可能由 Azure Active Directory 设备注册服务。 当设备加入通过加入工作区后时，该服务规定设备在 Azure Active Directory 对象，然后用于表示设备身份的本地设备上设置一个键。 然后，此设备身份可用于访问控制规则的云和本地在中托管的应用程序。

有关启用 Azure Active Directory 设备注册服务的详细信息，请参阅[Azure Active Directory 设备注册服务概述](https://msdn.microsoft.com/6a14cb1f-a058-4453-8ede-d9f4a66a7073.aspx)。

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>作为无缝第二个的工作区加入因素身份验证
公司都可以管理与信息访问和驱动器管辖并时授予消费者设备访问公司资源合规性风险。 在设备上的工作区加入与管理员提供了以下功能：

-   设备身份识别已知的设备。 管理员可以使用此信息驾车到资源的条件访问和控制访问权限。

-   提供更加无缝登录体验的用户，若要从受信任的设备上访问公司资源。

### <a name="single-sign-on"></a>单一登录
单一登录 (SSO) 中的此项 scenario 上下文是减少的最终用户已从已知设备访问公司资源输入的密码提示的功能。 此功能意味着提示用户访问公司应用程序和资源此设备从生存期 SSO 期间的一次。 如果设备使用加入工作区，注册了使用此设备的用户将获取永久性 SSO，默认情况下进行七天。 此用户都能无缝登录体验同一会话中或在新的会议。

## <a name="solution-overview"></a>解决方案概述
作为此解决方法的一部分，您将学会如何在受支持的设备上使用工作区加入和单一登录到公司资源的体验。

> [!NOTE]
> Windows 8.1、iOS 6.0 + 和 Android 4.0 + 设备支持，你必须配置回写的设备对象以及 Azure Active Directory 设备注册，请参阅[本地条件访问使用 Azure Active Directory 设备注册服务的分步指南](https://msdn.microsoft.com/library/azure/dn788908.aspx)

此解决方法才能将指导你通过演练以下步骤：

1.  [演练：与 Windows 设备的工作区加入](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [工作区参与 iOS 设备演练：](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [使用 Android 设备演练：工作区加入](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>请参阅
[与设备注册服务配置联盟服务器](../deployment/configure-a-federation-server-with-device-registration-service.md)



