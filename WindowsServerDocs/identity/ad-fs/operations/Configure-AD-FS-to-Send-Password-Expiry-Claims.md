---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: 配置 AD FS 以发送密码过期声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3be14b824038e9424b86c40bfd657dd988fa99e9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189867"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>配置 AD FS 以发送密码过期声明


可以配置 Active Directory 联合身份验证服务 (AD FS) 将发送到受 ADFS 信赖方信任 （应用程序） 的密码过期声明。 如何使用这些声明取决于应用程序。 例如，与 Office 365 作为信赖方，更新了对 Exchange 和 Outlook 以通知他们即将-到--已过期的密码的联合的用户。

若要配置 AD FS 以发送密码到期声明与信赖方信任，你必须添加以下声明规则到此信赖方信任：

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> 密码过期声明才可用于用户名和密码与 Microsoft Passport 的工作身份验证类型。  如果用户进行身份验证使用 Windows 集成身份验证和 Passport 未配置，声明将不可用并且用户不会看到密码过期通知。

> [!NOTE]
> 存在是 14 天窗口，因此如果密码即将在 14 天内到期，才会填充已发送的声明。

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
