---
title: AD FS 分页登录
description: 本文档介绍 AD FS 2019 的新登录体验。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 41938aef1c22f78a49e2817d0764b8110ef30f54
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866148"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS 分页登录


对于 Windows Server 2019 中的 AD FS，我们已重新设计登录 UI。  现在，AD FS 登录与 Azure AD 具有相同的外观和感觉。  这将为用户提供更一致的登录体验，其中包含居中和分页的用户流。

## <a name="whats-changing"></a>变化
在 Windows Server 2012 R2 和2016的 AD FS 中，登录屏幕的外观如下所示：

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

我们正在移开显示位于屏幕右侧的单一窗体。

在 Windows Server 2019 的 AD FS 中，这是你将看到的主要设计变化：


- **居中的 UI**。 之前，登录 UI 位于屏幕的右侧，如上所示。 我们已移动 UI 前端和中心，使体验实现现代化。
- **分页**。 我们引入了一个新的流，它将指导您逐步完成登录体验，而不是向您提供一个用于填写的长格式。 我们的遥测表明，使用这种方法，我们的客户可以获得更多成功的登录。它还为合并各种身份验证方法提供了更大的灵活性，例如美国电话因素身份验证。

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

在第一页上，系统将要求你输入用户名。 你还可以选择 "使我保持登录" 选项，以减少登录提示的频率，并在安全时保持登录。 （默认情况下禁用此选项。）

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

在第二页上，你将看到管理员配置的身份验证选项。 如果允许将外部身份验证作为主要身份验证，则也会将其包含在内。

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

在第三页上，系统将要求您输入密码（假设您选择了 "密码" 作为身份验证选项）。

## <a name="how-to-get-the-new-experience"></a>如何获取新体验

### <a name="new-installation-of-ad-fs"></a>AD FS 的全新安装
如果你是 AD FS 的新客户，则默认情况下会收到新的设计。

### <a name="upgrading-a-farm"></a>升级场
如果你是 AD FS 2012 R2 或2016的现有客户，可通过两种方法在将服务器升级到 AD FS 2019 并启用 FBL 到2019后接收新设计。

- 允许通过 Powershell 进行新的登录。 运行以下命令以启用分页：``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - 通过 Powershell 或 AD FS 服务器管理器启用外部身份验证作为主要身份验证。 启用此功能后，将启用新的分页登录页。
如果你是 AD FS 的新客户，则默认情况下会收到新的设计。 但是，如果你是 AD FS 2012 R2 或2016的现有客户，则需要执行几个步骤才能接收新的设计：``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>自定义
自定义选项仍适用于 2019 AD FS。
下面是一些指向其他文档的链接，供你参考。

•对于不打算将服务器升级到 AD FS 2019 但仍需要新设计的用户：[在 Active Directory 联合身份验证服务中使用 Azure AD UX Web 主题](azure-ux-web-theme-in-ad-fs.md)

•自定义的一个中心位置：[AD FS 用户登录自定义](ad-fs-user-sign-in-customization.md)
