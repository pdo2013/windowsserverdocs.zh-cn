---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: "Winlogon 自动重启登录 (ARSO)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 27d4285d34105908555458a95bd70fc04fd2901a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自动重启登录 (ARSO)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

**作者**：与 Windows 组索旋转器高级支持升级工程师

> [!NOTE]
> 内容由 Microsoft 客户支持工程师和适用于经验丰富的管理员系统师寻找更深入 technical 说明功能和 Windows Server 2012 R2 的解决方案不是通常提供 TechNet 上的主题。 但是，它不经历了同一编辑通行证，因此一些语言可能看起来比通常 TechNet 上找到内容小于优化。

## <a name="overview"></a>概述
Windows 8 中引入了锁屏界面应用。  以下是运行，并在用户的会话已锁定时显示通知的应用程序 （日历约会、 电子邮件和邮件、 等）。  由于 Windows 更新过程会重新启动的设备无法显示在重新启动时这些锁屏界面通知。  某些用户取决于这些锁屏界面应用程序。

## <a name="whats-changed"></a>更改的内容？
当用户在 Windows 8.1 设备上登录时，LSA 将用户凭据加密在内存中保存访问仅通过 lsass.exe。 Windows 更新启动时自动重新启动而用户存在不，将使用这些凭据用户配置自动登录。 作为系统 TCB 特权与运行 Windows 更新会初始化 RPC 通话，若要执行此操作。

在重新启动，用户将会自动将自动登录机制通过登录，然后此外锁定保护用户会话。 将通过 Winlogon 启动的锁定，而通过 LSA 完成凭据管理。  通过自动登录并锁定用户在主机上，用户的锁屏界面应用将重新启动和可用。

> [!NOTE]
> Windows 更新引起重新启动、 交互式的最后一个用户自动登录并会话处于锁定状态后，因此可以运行用户的锁屏界面应用。

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**快速概述**

-   Windows 更新将需要重新启动

-   是能够重新启动计算机 (*没有运行该应用会丢失数据*)？

    -   为你重启

    -   重新登录

    -   锁屏计算机

-   启用或禁用由组策略

    -   默认情况下，在服务器 Sku 已禁用

-   为什么？

    -   用户登录重新之前，不能完成某些更新。

    -   更好的用户体验： 无需等待 15 分钟才能完成安装的更新

-   如何？ 自动登录

    -   存储的密码，然后使用该凭据登录你

    -   保存凭据为已分页内存中 LSA 机密

    -   如果已启用 BitLocker，只可以启用

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>组策略： 登录最后一个交互式用户在系统启动重启后自动
在 Windows 8.1 / Windows Server 2012 R2 的锁定屏幕用户在 Windows 更新重启后自动登录加入个服务器 Sku 的客户端 Sku 退出。

**策略位置：**计算机配置 > 策略 > 管理模板 > Windows 组件 > Windows 登录选项

**策略名称：**登录最后一个交互式用户在系统启动重启后自动

**在受支持：**至少 Windows Server 2012 R2、 Windows 8.1 或 Windows RT 8.1

**描述/帮助：**

此策略设置控制是否设备将自动登录最后交互式的用户进行 Windows 更新后重新启动系统。

启用或禁用此策略设置，如果设备牢固保存用户的凭据 （包括用户名、 域和加密的密码） Windows 更新重启后自动登录配置。 在 Windows 更新重新启动后用户是自动登录，并自动与该用户配置所有锁屏界面应用锁定会话。

禁用此策略，如果设备在 Windows 更新重新启动后不存储的自动登录该用户的凭据。 系统重新启动后，不会重新启动的用户的锁屏界面应用。

**注册表编辑器**

|值名称|键入|数据|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**示例：**<br /><br />0 （已启用）<br /><br />1 （禁用）|

**策略注册表位置：** HKLM\SOFTWARE\ Microsoft \Windows\CurrentVersion\Policies\System

**类型：** DWORD

**注册表名称：** DisableAutomaticRestartSignOn

值： 0 或 1

0 = 启用

1 = 禁用

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>故障排除
当 WinLogon 自动锁定时，则 WinLogon 的状态跟踪将存储在 WinLogon 事件日志。

记录试图自动登录配置的状态

-   如果成功

    -   这种情况下录制它

-   如果故障：

    -   记录失败是什么

-   更改 BitLocker 状态时：

    -   将记录凭据删除

        -   这些将存储在 LSA 运营日志。

### <a name="reasons-why-autologon-might-fail"></a>为什么自动登录可能失败的原因
有几种情况下，无法实现用户自动登录。  本部分适用捕获可能导致该已知的方案。

### <a name="user-must-change-password-at-next-login"></a>用户必须更改密码，在下次登录
需要密码更改在下次登录时，用户登录可以输入阻止的状态。  这可以检测到的大多数情况下，重启之前但并非所有 (例如，可以达到密码到期之间关机并重下次登录。

### <a name="user-account-disabled"></a>用户帐户已禁用
即使已禁用，可以保留现有的用户会话。  重启即可已禁用的帐户可以检测到本地在大多数情况下，具体取决于 gp 可能无法对于域帐户 （即使控制器上的帐户已禁用某个域缓存登录方案工作）。

### <a name="logon-hours-and-parental-controls"></a>登录的时间和家长控制
登录时间以及家长控制可以禁止从创建的新用户会话。  如果出现在此窗口期间重启，不想允许用户进行登录。  没有其他导致锁定或注销作为合规性操作的策略。  尤其是维护窗口较通常在此期间，这可能是许多孩子情况下在哪里帐户锁定可能会在床时间和唤醒，之间有问题。

## <a name="additional-resources"></a>更多资源
**表用 \\\ * 阿拉伯语 3: ARSO 词汇表**

|术语|定义|
|--------|--------------|
|自动登录|自动登录是一项功能已被里几个版本的 Windows。  它是一个的工具，例如适用于 Windows 的自动登录 v3.01 者甚至还的 Windows 公开功能* [http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />它使设备的单个用户自动无需输入凭据登录。 配置的凭据，为加密 LSA 机密的注册表中存储。|


