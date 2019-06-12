---
title: AD FS 登录分页
description: 本文档描述了 AD FS 2019 的新登录体验。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c528b9c4e944849b7ed9a2fc5213a7b263be70c7
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687377"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS 登录分页


中的 AD FS Windows Server 2019，我们已重新设计登录 UI。  现在，AD FS 登录中将有相同的外观和感觉的 Azure AD。  这将使用户获得更一致登录体验，合并居中和分页用户流。

## <a name="whats-changing"></a>正在更改的内容
在 Windows Server 2012 R2 和 2016年中 AD FS 中，在登录屏幕看起来类似如下：

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

我们正在抛弃显示一个窗体位于屏幕的右侧。

在 Windows Server 2019 中的 AD FS，这些是你将看到重大设计更改：


- **一个以 UI 为中心**。 以前，如上所示，在屏幕右侧将存在登录 UI。 我们已移动 UI 前端和中心以实现现代化的体验。
- **分页**。 而不是提供要填写的长格式，我们已合并将引导你完成分步的登录体验的新流。 我们的遥测数据显示了使用此方法时，我们的客户有更大的成功的登录名。它还会为我们提供更大的灵活性以合并各种身份验证方法，此类美国 phone 身份验证。

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

在第一页上，你将需要输入你的用户名。 您还可以选择"使我保持登录"选项以减少登录提示的频率并安全地执行此操作时保持登录状态。 （此选项禁用默认情况下。）

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

在第二页上，系统将显示由您的管理员配置的身份验证选项。 如果启用了允许作为主要的外部身份验证，这也将包括这些属性。

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

在第三个页上，你将需要输入密码 （作为身份验证选项假定选择了"密码"）。

## <a name="how-to-get-the-new-experience"></a>如何获得新体验

### <a name="new-installation-of-ad-fs"></a>新安装的 AD FS
如果你是到 AD FS 的新客户，你将收到新的设计，默认情况下。

### <a name="upgrading-a-farm"></a>对场进行升级
如果你是现有客户 AD FS 2012 R2 或 2016 中，有两种方法在服务器升级到 AD FS 2019 并启用到 2019 FBL 后接收新的设计。

- 允许新登录通过 Powershell。 运行以下命令以启用分页： ``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - 启用为主要，通过 Powershell 或通过 AD FS 服务器管理器中的外部身份验证。 启用此功能时，将启用新分页的登录页。
如果你是到 AD FS 的新客户，你将收到新的设计，默认情况下。 但是，如果你是现有客户使用 AD FS 2012 R2 或 2016年，有几个步骤都需要能接收到新的设计： ``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>自定义
自定义选项仍会适用于 AD FS 2019。
以下是一些指向其他文档以供参考。

• 对于那些执行未计划其服务器升级到 AD FS 2019 但仍想使用新的设计：[在 Active Directory 联合身份验证服务中使用 Azure AD 用户体验 Web 主题](azure-ux-web-theme-in-ad-fs.md)

• 自定义一个中心位置：[AD FS 用户登录自定义](ad-fs-user-sign-in-customization.md)
