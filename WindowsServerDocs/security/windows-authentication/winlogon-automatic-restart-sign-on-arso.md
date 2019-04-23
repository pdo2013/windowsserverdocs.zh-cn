---
title: Winlogon 自动重启登录 (ARSO)
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 172eb34fbfdb8a91adf55e35f888e90f5688d0e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849238"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自动重启登录 (ARSO)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**作者**:与 Windows 组的 Justin Turner，高级支持呈报工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
Windows 8 引入了锁定屏幕应用程序。  这些是应用程序运行并在用户会话锁定时显示通知 （日历约会、 电子邮件和消息，等等）。  由于 Windows 更新过程将重新启动的设备无法显示重新启动时这些锁定屏幕通知。  某些用户依赖于这些锁屏幕应用程序。  
  
## <a name="whats-changed"></a>更改了哪些内容？  
用户登录时在 Windows 8.1 设备上，LSA 将仅由 lsass.exe 在可访问的加密内存中保存的用户凭据。 当 Windows 更新启动时自动重新启动而无需用户状态显示时，这些凭据将用于为用户配置自动登录。 以系统具有 TCB 权限运行的 Windows 更新将启动 RPC 调用来执行此操作。  
  
在重新启动，用户会自动登录通过自动登录机制，然后另外锁定以保护用户的会话。 将通过 Winlogon 启动的锁定，而通过 LSA 在凭据管理。  通过在自动登录并在控制台上锁定用户，用户的锁定屏幕应用程序将重新启动且可用。  
  
> [!NOTE]  
> Windows 更新引起重新启动上一个交互式用户自动登录将会锁定会话之后，因此可以运行用户的锁定屏幕应用。  
  
![显示在锁定屏幕的屏幕截图](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)  
  
![显示锁定屏幕的应用的屏幕截图](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)  
  
**简要概述**  
  
-   Windows 更新要求重新启动  
  
-   是能够重新启动计算机 (*运行的任何应用程序可能会丢失数据*)？  
  
    -   为你重新启动  
  
    -   重新登录  
  
    -   锁定计算机  
  
-   启用或禁用通过组策略  
  
    -   在服务器 Sku 中默认已禁用  
  
-   为什么？  
  
    -   返回在用户登录之前，无法完成某些更新。  
  
    -   更好的用户体验： 无需等待 15 分钟才能完成安装的更新  
  
-   如何操作？ AutoLogon  
  
    -   存储密码，将使用该凭据在登录时  
  
    -   为 LSA 机密分页的内存中保存凭据  
  
    -   仅当启用了 BitLocker，才能启用  
  
## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>组策略：登录中最后一个交互式用户在系统启动的重新启动后自动  
在 Windows 8.1 / Windows Server 2012 R2，自动锁定屏幕用户在 Windows Update 重新启动的登录服务器 Sku 选择加入和退出客户端 Sku。  
  
**策略位置：** 计算机配置 > 策略 > 管理模板 > Windows 组件 > Windows 登录选项  
  
**策略名称：** 登录中最后一个交互式用户在系统启动的重新启动后自动  
  
**支持：** 至少为 Windows Server 2012 R2 中，Windows 8.1 或 Windows RT 8.1  
  
**说明/帮助：**  
  
此策略设置控制是否在设备将自动登录在最后一个交互式用户 Windows 更新后重新启动系统。  
  
如果启用或未配置此策略设置，设备安全地保存用户的凭据 （包括用户名、 域和加密的密码） 在 Windows 更新重启后自动登录配置。 Windows 更新重启，用户自动登录的并使用为该用户配置的所有锁屏幕应用自动锁定会话。  
  
如果禁用此策略设置，设备在 Windows 更新重启后不存储用户的凭据进行自动登录。 系统重新启动后未重新启动用户的锁定屏幕应用。  
  
**注册表编辑器**  
  
|值名称|在任务栏的搜索框中键入|数据|  
|-------|----|----|  
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**示例：**<br /><br />0 （已启用）<br /><br />1 （已禁用）|  
  
**策略注册表位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System  
  
**类型：** DWORD  
  
**注册表名称：** DisableAutomaticRestartSignOn  
  
值：0 或 1  
  
0 = 已启用  
  
1 = 已禁用  
  
![显示策略设置的屏幕截图控制可以在其中指定是否在设备将自动在最后一个交互式用户之后登录 Windows 更新重新启动系统的 UI](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)  
  
## <a name="troubleshooting"></a>疑难解答  
当 WinLogon 自动锁定时，WinLogon 的状态跟踪将存储在 WinLogon 事件日志。  
  
记录自动登录配置尝试的状态  
  
-   如果成功  
  
    -   这种情况下记录  
  
-   如果故障：  
  
    -   记录失败时  
  
-   当 BitLocker 的状态更改：  
  
    -   将记录删除凭据  
  
        -   这些将存储在 LSA 操作日志。  
  
### <a name="reasons-why-autologon-might-fail"></a>自动登录可能失败的原因  
有几个情况下不能在其中实现用户自动登录。  本部分旨在捕获，这可以在其中发生的已知的方案。  
  
### <a name="user-must-change-password-at-next-login"></a>用户必须更改在下次登录密码  
需要在下次登录时更改密码时，用户登录名可以进入阻止的状态。  这可以在大多数情况下，重新启动之前检测到，但不是所有 (例如，可以达到在密码过期时关闭和下一次登录之间。  
  
### <a name="user-account-disabled"></a>用户帐户已禁用  
可以维护现有的用户会话，即使禁用。  重新启动已禁用的帐户可以检测到本地在大多数情况下提前，具体取决于 gp 则可能不是域帐户 （即使在 DC 上禁用帐户某些域缓存登录方案工作）。  
  
### <a name="logon-hours-and-parental-controls"></a>登录时间和家长控制  
登录时间和家长控制可以禁止在正在创建一个新用户会话。  如果要在此时段内发生重新启动，不会允许用户登录。  没有其他策略，让锁或注销作为符合性操作。  这可能是许多子情况下可能发生帐户锁定唤醒，平台时间之间有问题，尤其是在维护时段是通常在此时间。  
  
## <a name="additional-resources"></a>其他资源  
**表 SEQ 表\\ \* ARABIC 3:ARSO 术语表**  
  
|术语|定义|  
|----|-------|  
|Autologon|自动登录是一项功能已在 Windows 中存在几个版本。  它是有案可稽的甚至已自动登录的 Windows v3.01 等工具的 Windows 功能 *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />它允许单个用户的设备的自动登录而无需输入凭据。 配置并存储在注册表中为加密的 LSA 机密凭据。|  
  

