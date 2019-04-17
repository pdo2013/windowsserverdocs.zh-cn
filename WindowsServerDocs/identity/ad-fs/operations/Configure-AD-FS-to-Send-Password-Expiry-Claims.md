---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: "配置广告 FS 发送密码到期索赔"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 386a5ac921ba609c371121b8657351667628951b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>配置广告 FS 发送密码到期索赔

>适用于：Windows Server 2016，Windows Server 2012 R2

您可以配置了 Active Directory 联合身份验证服务 (AD FS) 发送到信赖的方信任（应用）中受 ADFS 密码到期索赔。 这些声明的使用方式取决于应用程序。 例如，与作为你依赖方 Office 365、更新已经实现了对通知的其很快到-会-已过期密码联合的用户的 Exchange and Outlook。

配置广告 FS 发送密码到期声明与信赖的方信任，你必须添加以下索赔规则此信赖的方信任：

```
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
=> issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> 密码到期索赔它们仅适用于用户名、密码和 Microsoft Passport 的工作身份验证类型。  如果用户可验证使用 Windows 的集成身份验证和 Passport 未配置索赔将不可用，用户不会看到密码到期通知。

> [!NOTE]
> 没有 14 天后窗口将仅填充发送的索赔，如果密码将在 14 天内过期，因此。

## <a name="see-also"></a>请参阅
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
