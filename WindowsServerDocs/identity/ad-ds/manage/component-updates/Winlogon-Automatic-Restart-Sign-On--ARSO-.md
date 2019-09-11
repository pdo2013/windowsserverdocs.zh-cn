---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Winlogon 自动重新启动登录（ARSO）
description: Windows 自动重启登录如何有助于提高用户的工作效率。
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56f485491340b3974d8bf5ba697c6cf01f3e56ac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868206"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自动重新启动登录（ARSO）

在 Windows 更新期间，必须执行特定于用户的进程才能完成更新。 这些进程要求用户登录到其设备。 在启动更新后第一次登录时，用户必须等待这些用户特定进程完成，然后才能开始使用其设备。

## <a name="how-does-it-work"></a>如何运作？

当 Windows 更新启动自动重新启动时，ARSO 将提取当前已登录用户的派生凭据，将其保存到磁盘，并为用户配置自动登录。 Windows 更新以 TCB 权限运行的系统将启动 RPC 调用来执行此操作。

最后 Windows 更新重新启动后，用户将通过自动登录机制自动登录，用户的会话将与保留的密码解除冻结。 此外，设备会被锁定以保护用户的会话。 锁定将通过 Winlogon 启动，而凭据管理是由本地安全机构（LSA）完成的。 成功 ARSO 配置和登录后，将立即从磁盘中删除保存的凭据。

通过在控制台上自动登录并锁定用户，Windows 更新可以在用户返回到设备之前完成用户特定的进程。 这样，用户就可以立即开始使用其设备。

ARSO 以不同的方式对待非托管和托管设备。 对于非托管设备，将使用设备加密，但不需要用户获取 ARSO。 对于托管设备，ARSO 配置需要 TPM 2.0、SecureBoot 和 BitLocker。 IT 管理员可以通过组策略覆盖此要求。 托管设备的 ARSO 当前仅适用于加入 Azure Active Directory 的设备。

|   | Windows 更新| shutdown-g t 0  | 用户启动的重新启动 | 具有 SHUTDOWN_ARSO/EWX_ARSO 标志的 Api |
| --- | :---: | :---: | :---: | :---: |
| 托管设备 | :heavy_check_mark:  | :heavy_check_mark: |   | :heavy_check_mark: |
| 非托管设备 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!NOTE]
> Windows 更新导致重新启动后，最后一个交互式用户会自动登录，并且会话将被锁定。 这使得即使 Windows 更新重新启动，用户的锁屏应用仍可运行。

![“设置”页面](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>策略 #1

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>重新启动后自动登录并锁定上次交互用户

在 Windows 10 中，为服务器 Sku 禁用了 ARSO 并为客户端 Sku 选择退出。

**组策略位置：** 计算机配置 > 管理模板 > Windows 组件 > Windows 登录选项

**Intune 策略：**

- 平台Windows 10 和更高版本
- 配置文件类型：管理模板
- 路径： \Windows \Windows 登录选项

**支持：** 至少为 Windows 10 版本1903

**说明：**

此策略设置控制在系统重新启动之后或在关机和冷启动之后，设备是否将自动登录并锁定最后一个交互式用户。

仅当上次交互用户在重新启动或关机之前未注销时才会出现这种情况。

如果设备已加入到 Active Directory 或 Azure Active Directory，则此策略仅适用于 Windows 更新重启。 否则，这将适用于 Windows 更新重启和用户启动的重新启动和关闭。

如果未配置此策略设置，则默认情况下启用它。 启用策略后，用户会自动登录，并且在设备启动后，将自动锁定会话并为该用户配置所有锁定屏幕应用。

启用此策略后，可以通过 ConfigAutomaticRestartSignOn 策略配置其设置，该策略配置自动登录的模式并在重启或冷启动后锁定最后一个交互式用户。

如果禁用此策略设置，则设备不会配置自动登录。 在系统重新启动后，用户的锁屏应用程序不会重新启动。

**注册表编辑器：**

| 值名称 | 类型 | Data |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0（启用 ARSO） |
|   |   | 1（禁用 ARSO） |

**策略注册表位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**类型：** DWORD

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>策略 #2

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>配置在重启或冷启动后自动登录并锁定上次交互用户的模式

**组策略位置：** 计算机配置 > 管理模板 > Windows 组件 > Windows 登录选项

**Intune 策略：**

- 平台Windows 10 和更高版本
- 配置文件类型：管理模板
- 路径： \Windows \Windows 登录选项

**支持：** 至少为 Windows 10 版本1903

**说明：**

此策略设置控制在重新启动或冷启动之后，自动重启和登录和锁定进行的配置。 如果选择了 "在重新启动后自动登录并锁定上次交互用户" 策略中的 "已禁用"，则不会进行自动登录，并且不需要配置此策略。

如果启用此策略设置，您可以选择以下两个选项之一：

1. "如果 BitLocker 处于启用状态且未挂起"，则指定仅当 BitLocker 处于活动状态且在重启或关闭过程中未挂起时才会进行自动登录和锁定。 如果 BitLocker 在更新过程中不打开或挂起，此时可以在设备的硬盘驱动器上访问个人数据。 BitLocker 挂起暂时会删除对系统组件和数据的保护，但在某些情况下，可能需要成功更新启动关键组件。
   - 在更新过程中，BitLocker 将在以下情况中挂起：
      - 设备没有 TPM 2.0 和 PCR7，或
      - 设备不使用仅 TPM 保护程序
2. "始终启用" 指定即使在重启或关闭期间 BitLocker 关闭或挂起，也会进行自动登录。 如果未启用 BitLocker，则可以在硬盘驱动器上访问个人数据。 如果确信配置的设备位于安全的物理位置，则只能在此条件下运行自动重启和登录。

如果禁用或未配置此设置，自动登录将默认为 "如果 BitLocker 已启用且未挂起" 行为。

**注册表编辑器**

| 值名称 | type | Data |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0（启用 ARSO，如果安全） |
|   |   | 1（启用 ARSO always） |

**策略注册表位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**类型：** DWORD

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>疑难解答

当 WinLogon 自动锁定时，WinLogon 的状态跟踪将存储在 WinLogon 事件日志中。

记录自动登录配置的状态

- 如果成功
   - 记录此类
- 如果失败：
   - 记录失败的原因
- 当 BitLocker 的状态发生更改时：
   - 将记录删除凭据
   - 它们将存储在 LSA 操作日志中。

### <a name="reasons-why-autologon-might-fail"></a>自动登录失败的原因

在某些情况下，无法实现用户自动登录。  本部分旨在捕获可能出现此情况的已知情况。

### <a name="user-must-change-password-at-next-login"></a>用户下次登录时必须更改密码

如果需要在下一次登录时更改密码，用户登录可以进入 "已阻止" 状态。  在大多数情况下，可以在重新启动之前检测到这种情况，但并非全部（例如，在关机和下次登录之间可以达到密码过期时间）。

### <a name="user-account-disabled"></a>用户帐户已禁用

即使禁用了现有用户会话也是如此。  在大多数情况下，可在大多数情况下在本地检测到已禁用的帐户的重新启动，这取决于 gp，因为它可能不是域帐户（即使在 DC 上禁用了帐户，也可以使用某些域缓存登录方案）。

### <a name="logon-hours-and-parental-controls"></a>登录时间和家长控制

登录时间和家长控制可以禁止创建新的用户会话。  如果在此时段内发生重启，则不允许用户登录。  还有其他策略导致锁定或注销作为符合性操作。 将记录自动登录配置尝试的状态。

## <a name="security-details"></a>安全详细信息

### <a name="credentials-stored"></a>存储的凭据

|   | 密码哈希 | 凭据密钥 | 票证授予票证 | 主刷新令牌 |
| --- | :---: | :---: | :---: | :---: |
| 本地帐户 | :heavy_check_mark: | :heavy_check_mark: |   |   |
| MSA 帐户 | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Azure AD 联接的帐户 | :heavy_check_mark: | :heavy_check_mark: | ： heavy_check_mark：（如果为混合） | :heavy_check_mark: |
| 已加入域的帐户 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | ： heavy_check_mark：（如果为混合） |

### <a name="credential-guard-interaction"></a>Credential Guard 交互

如果设备启用了 Credential Guard，将使用特定于当前启动会话的密钥来加密用户的派生机密。 因此，在启用 Credential Guard 的设备上当前不支持 ARSO。

## <a name="additional-resources"></a>其他资源

自动登录是在 Windows 中为多个版本提供的一项功能。 它是 Windows 的一项文档功能，甚至包含 Windows [http：/sysinternals/bb963905](https://technet.microsoft.com/sysinternals/bb963905.aspx)等工具。 它允许设备的单个用户自动登录而无需输入凭据。 凭据作为加密的 LSA 机密配置并存储在注册表中。 这对于很多子情况都可能会导致帐户锁定在平台时间和唤醒之间发生，尤其是在这段时间内通常会发生的情况。
