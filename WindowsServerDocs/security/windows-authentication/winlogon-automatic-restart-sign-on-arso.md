---
title: Winlogon 自动重启登录 (ARSO)
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f085cf78a01148f97a450577131213ce977a432a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402322"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自动重启登录 (ARSO)

>适用于：Windows Server（半年频道）、Windows Server 2016

**作者**：Justin Turner，具有 Windows 组的高级支持升级工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
Windows 8 引入了锁定屏幕应用。  这些应用程序是在锁定用户会话时运行和显示通知的应用程序（日历约会、电子邮件和消息等）。  由于 Windows 更新进程而重新启动的设备在重新启动时无法显示这些锁定屏幕通知。  某些用户依赖于这些锁屏界面应用程序。  
  
## <a name="whats-changed"></a>更改了哪些内容？  
当用户登录 Windows 8.1 设备时，LSA 会将用户凭据保存到只能由 lsass.exe 访问的加密内存中。 如果 Windows 更新在没有用户状态的情况下启动自动重新启动，则将使用这些凭据为用户配置自动登录。 Windows 更新以 TCB 权限运行的系统将启动 RPC 调用来执行此操作。  
  
重新启动时，用户将通过自动登录机制自动登录，并另外锁定以保护用户会话。 将通过 Winlogon 启动锁定，而由 LSA 完成凭据管理。  通过在控制台上自动登录并锁定用户，用户的锁屏应用程序将重新启动并可用。  
  
> [!NOTE]  
> 在 Windows 更新导致重新启动后，最后一个交互式用户会自动登录，并且会话将被锁定，因此用户的锁屏界面应用可以运行。  
  
![显示锁屏界面的屏幕截图](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)  
  
![显示锁屏界面应用程序的屏幕截图](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)  
  
**快速概述**  
  
-   Windows 更新需要重新启动  
  
-   计算机能否重新启动（*没有运行的应用将丢失数据*）？  
  
    -   重新启动  
  
    -   重新登录  
  
    -   锁定计算机  
  
-   组策略启用或禁用  
  
    -   在服务器 Sku 中默认禁用  
  
-   为什么？  
  
    -   在用户重新登录之前，某些更新无法完成。  
  
    -   更好的用户体验：若要完成安装，无需等待15分钟  
  
-   帮助? AutoLogon  
  
    -   存储密码，使用该凭据登录  
  
    -   在分页内存中将凭据保存为 LSA 机密  
  
    -   只有启用了 BitLocker，才能启用  
  
## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>组策略：在系统启动的重新启动后自动登录上次交互用户  
在 Windows 8.1/Windows Server 2012 R2 中，在 Windows 更新重新启动后锁定屏幕用户的自动登录是选择用于服务器 Sku，并选择退出客户端 Sku。  
  
**策略位置：** 计算机配置 > 策略 > 管理模板 > Windows 组件 > Windows Logon 选项  
  
**策略名称：** 在系统启动的重新启动后自动登录上次交互用户  
  
**支持：** 至少为 Windows Server 2012 R2、Windows 8.1 或 Windows RT 8。1  
  
**Description/Help：**  
  
此策略设置控制在 Windows 更新重新启动系统后设备是否会自动登录上一个交互式用户。  
  
如果启用或未配置此策略设置，则设备会安全地保存用户的凭据（包括用户名、域和加密密码），以便在 Windows 更新重新启动后配置自动登录。 Windows 更新重新启动后，用户会自动登录，并将自动锁定会话，同时为该用户配置了所有锁屏界面应用。  
  
如果禁用此策略设置，则在 Windows 更新重新启动后，设备不会将用户的凭据存储到自动登录。 在系统重新启动后，用户的锁屏应用程序不会重新启动。  
  
**注册表编辑器**  
  
|值名称|type|Data|  
|-------|----|----|  
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**示例：**<br /><br />0（已启用）<br /><br />1（禁用）|  
  
**策略注册表位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System  
  
**类型：** DWORD  
  
**注册表名称：** DisableAutomaticRestartSignOn  
  
值：0 或 1  
  
0 = 启用  
  
1 = 禁用  
  
![显示策略设置控制 UI 的屏幕截图，你可以在其中指定设备在 Windows 更新重新启动系统后是否会自动登录上一个交互式用户](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)  
  
## <a name="troubleshooting"></a>疑难解答  
当 WinLogon 自动锁定时，WinLogon 的状态跟踪将存储在 WinLogon 事件日志中。  
  
记录自动登录配置的状态  
  
-   如果成功  
  
    -   记录此类  
  
-   如果失败：  
  
    -   记录失败的原因  
  
-   当 BitLocker 的状态发生更改时：  
  
    -   将记录删除凭据  
  
        -   它们将存储在 LSA 操作日志中。  
  
### <a name="reasons-why-autologon-might-fail"></a>自动登录失败的原因  
在某些情况下，无法实现用户自动登录。  本部分旨在捕获可能出现此情况的已知情况。  
  
### <a name="user-must-change-password-at-next-login"></a>用户下次登录时必须更改密码  
如果需要在下一次登录时更改密码，用户登录可以进入 "已阻止" 状态。  在大多数情况下，可以在重新启动之前检测到这种情况，但并非全部（例如，在关机和下次登录之间可以达到密码过期时间）。  
  
### <a name="user-account-disabled"></a>用户帐户已禁用  
即使禁用了现有用户会话也是如此。  在大多数情况下，可在大多数情况下在本地检测到已禁用的帐户的重新启动，这取决于 gp，因为它可能不是域帐户（即使在 DC 上禁用了帐户，也可以使用某些域缓存登录方案）。  
  
### <a name="logon-hours-and-parental-controls"></a>登录时间和家长控制  
登录时间和家长控制可以禁止创建新的用户会话。  如果在此时段内发生重启，则不允许用户登录。  还有其他策略导致锁定或注销作为符合性操作。  这对于很多子情况都可能会导致帐户锁定在平台时间和唤醒之间发生，尤其是在这段时间内通常会发生的情况。  
  
## <a name="additional-resources"></a>其他资源  
**Table SEQ Table \\ @ no__t-2 阿拉伯3：ARSO 术语表 @ no__t-0  
  
|术语|定义|  
|----|-------|  
|Autologon|自动登录是在 Windows 中为多个版本提供的一项功能。  它是 Windows 的已记录功能，甚至包含 Windows 3.01  *[http：/sysinternals/Bb963905](https://technet.microsoft.com/sysinternals/bb963905.aspx)的自动登录等工具*<br /><br />它允许设备的单个用户自动登录而无需输入凭据。 凭据作为加密的 LSA 机密配置并存储在注册表中。|  
  

