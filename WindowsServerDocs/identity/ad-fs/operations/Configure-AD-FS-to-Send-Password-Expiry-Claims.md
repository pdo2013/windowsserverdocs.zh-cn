---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: 配置 AD FS 以发送密码过期声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 29760dcc0dffe9fe29289f20f1abca4cfd8325b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407690"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>配置 AD FS 以发送密码过期声明


可以配置 Active Directory 联合身份验证服务（AD FS），将密码过期声明发送到受 ADFS 保护的信赖方信任（应用程序）。 如何使用这些声明取决于应用程序。 例如，使用 Office 365 作为你的信赖方时，已将更新部署到 Exchange 和 Outlook，以通知联合用户即将过期的密码。

若要配置 AD FS 以便向信赖方信任发送密码过期声明，你必须将以下声明规则添加到此信赖方信任：

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> 密码到期声明仅可用于用户名和密码，以及 Microsoft Passport for Work 身份验证类型。  如果用户使用 Windows 集成身份验证进行身份验证，并且未配置 Passport，则声明将不可用，用户将看不到密码过期通知。

> [!NOTE]
> 存在14天的窗口，因此仅当密码在14天内到期时才会填充已发送的声明。

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
