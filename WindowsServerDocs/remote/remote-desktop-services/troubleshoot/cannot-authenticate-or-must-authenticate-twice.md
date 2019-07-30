---
title: 用户无法完成身份验证或必须完成身份验证两次
description: 排查在启动远程桌面连接时用户无法完成身份验证或必须完成身份验证两次的问题。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 85e332f5f66b59676ddd3b5383b1e5844c2b4c83
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529917"
---
# <a name="user-cant-authenticate-or-must-authenticate-twice"></a>用户无法完成身份验证或必须完成身份验证两次

本文解决了多个可能会导致用户身份验证出现问题的问题。

## <a name="access-denied-restricted-type-of-logon"></a>拒绝访问并出现“受限制的登录类型”消息

在此情况下，尝试连接到 Windows 10 或 Windows Server 2016 计算机的 Windows 10 用户被拒绝访问并会收到以下消息：

> 远程桌面连接：  
> 系统管理员限制了可以使用的登录类型（网络或交互式）。 如需帮助，请与系统管理员或技术支持人员联系。

如果 RDP 连接需要网络级别身份验证 (NLA)，而用户不是“远程桌面用户”组的成员，则会出现此问题。  如果没有为“远程桌面用户”组分配“从网络访问此计算机”用户权限，则也会出现此问题。  

若要解决此问题，请执行以下操作之一：

  - [修改用户的组成员身份或用户权限分配](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 禁用 NLA（不建议）。
  - 使用除 Windows 10 以外的远程桌面客户端。 例如，Windows 7 客户端就不会出现此问题。

### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>修改用户的组成员身份或用户权限分配

如果此问题只是影响一个用户，则最直接的解决方法就是将该用户添加到“远程桌面用户”组。 

如果该用户已是此组的成员（或者多个组成员遇到相同的问题），请检查远程 Windows 10 或 Windows Server 2016 计算机上的用户权限配置。

1. 打开组策略对象编辑器 (GPE) 并连接到远程计算机的本地策略。
2. 导航到“计算机配置”\\“Windows 设置”\\“安全设置”\\“本地策略”\\“用户权限分配”，右键单击“从网络访问此计算机”，然后选择“属性”。   
3. 检查“远程桌面用户”（或父组）的用户和组列表。 
4. 如果该列表不包含“远程桌面用户”或类似于“任何人”的父组，则必须将它添加到列表。   如果部署中有不止一台计算机，请使用组策略对象。  
    例如，“从网络访问此计算机”的默认成员身份包括“任何人”。   如果部署使用组策略对象来删除“任何人”，则你可能需要通过更新组策略对象来添加“远程桌面用户”，以还原访问权限。  

## <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>拒绝访问并出现“已拒绝远程调用 SAM 数据库”消息

如果域控制器运行 Windows Server 2016 或更高版本，而用户使用自定义的连接应用尝试建立连接，则很有可能会出现此行为。 具体而言，访问 Active Directory 中用户配置文件信息的应用程序将被拒绝访问。

此行为是 Windows 中的一项更改造成的。 在 Windows Server 2012 R2 及更低版本中，当用户登录到远程桌面时，远程连接管理器 (RCM) 会联系域控制器 (DC)，以查询特定于远程桌面的有关 Active Directory 域服务 (AD DS) 中用户对象的配置。 此信息显示在“Active Directory 用户和计算机”MMC 管理单元中用户对象属性的“远程桌面服务配置文件”选项卡上。

从 Windows Server 2016 开始，RCM 不再查询 AD DS 中用户的对象。 如果由于使用远程桌面服务属性而需要 RCM 查询 AD DS，必须手动启用查询。

> [!IMPORTANT]  
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，以便在出现问题时可以还原。

若要在 RD 会话主机服务器上启用旧的 RCM 行为，请配置以下注册表项，然后重启“远程桌面服务”服务：   
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Name：**fQueryUserConfigFromDC**
      - 键入：**Reg\_DWORD**
      - 值：**1**（十进制）

若要在除 RD 会话主机服务器以外的服务器上启用旧的 RCM 行为，请配置上述注册表项和以下附加的注册表项（然后重启该服务）：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

有关此行为的详细信息，请参阅 KB 3200967 [Windows Server 中远程连接管理器的更改](https://support.microsoft.com/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

## <a name="user-cant-sign-in-using-a-smart-card"></a>用户无法使用智能卡登录

此部分介绍如何解决用户无法使用智能卡登录到远程桌面的三种常见情况。

### <a name="cant-sign-in-with-a-smart-card-in-a-branch-office-with-a-read-only-domain-controller-rodc"></a>无法通过智能卡登录到使用只读域控制器 (RODC) 的分支机构

使用 RODC 的分支站点中包含 RDSH 服务器的部署会出现此问题。 RDSH 服务器托管在根域中。 分支站点上的用户属于一个子域，并使用智能卡进行身份验证。 RODC 配置为缓存用户密码（RODC 属于“允许的 RODC 密码复制组”）。  当用户尝试登录到 RDSH 服务器上的会话时，他们会收到类似于“尝试的登录无效。 原因是用户名或身份验证信息错误”的消息。

此问题是根 DC 和 RODC 管理用户凭据加密的方式导致的。 根 DC 使用加密密钥来加密凭据，RODC 为客户端提供解密密钥。 如果用户收到“无效”错误，则意味着这两个密钥不匹配。

若要解决此问题，可尝试以下方法之一：

- 通过在 RODC 上禁用密码缓存或者将可写 DC 部署到分支站点，来更改 DC 拓扑。
- 将 RDSH 服务器移到用户所在的子域。
- 允许用户在不使用智能卡的情况下登录。

请注意，上述所有解决方案都需要降低性能或安全级别。

### <a name="user-cant-sign-in-to-a-windows-server-2008-sp2-computer-using-a-smart-card"></a>用户无法使用智能卡登录到 Windows Server 2008 SP2 计算机

当用户登录到已使用 KB4093227 (2018.4B) 更新的 Windows Server 2008 SP2 计算机时，会出现此问题。 当用户尝试使用智能卡登录时，他们被拒绝访问并收到类似于“找不到有效的证书。 请检查卡是否已正确稳固地插入”的消息。 同时，Windows Server 计算机会记录应用程序事件“从插入的智能卡检索数字证书时出错。 签名无效。”

若要解决此问题，请使用 KB 4093227 的 2018.06 B 再发行版 [Windows Server 2008 中 Windows 远程桌面协议 (RDP) 拒绝服务漏洞的安全更新说明：2018 年 4 月 10 日](https://support.microsoft.com/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008)更新 Windows Server 计算机。

### <a name="cant-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>无法使用智能卡保持登录状态且“远程桌面服务”服务挂起

当用户登录到已使用 KB 4056446 更新的 Windows 或 Windows Server 计算机时，会出现此问题。 一开始，用户也许可以使用智能卡登录到该系统，但随后会接收“SCARD\_E\_NO\_SERVICE”错误消息。 远程计算机可能无响应。

若要解决此问题，请重启远程计算机。

若要解决此问题，请使用相应的修复程序更新远程计算机：

  - Windows Server 2008 SP2：KB 4090928 [Windows 泄漏 lsm.exe 进程中的句柄，智能卡应用程序可能会显示“SCARD\_E\_NO\_SERVICE”错误](https://support.microsoft.com/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2：KB 4103724 [2018 年 5 月 17 — KB4103724（月度汇总预览）](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 和 Windows 10 版本 1607：KB 4103720 [2018 年 5 月 17 — KB4103720（OS 内部版本 14393.2273）](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)

## <a name="if-the-remote-pc-is-locked-the-user-needs-to-enter-a-password-twice"></a>如果远程电脑已锁定，则用户需要输入密码两次

当用户尝试连接到部署中运行 Windows 10 版本 1709 的远程桌面，而该部署中的 RDP 连接不需要 NLA 时，可能会出现此问题。 在这种情况下，如果远程桌面已锁定，则用户在连接时需要输入其凭据两次。

若要解决此问题，请使用 KB 4343893 [2018 年 8 月 30 日 - KB4343893（OS 内部版本 16299.637）](https://support.microsoft.com/help/4343893/windows-10-update-kb4343893)更新 Windows 10 版本 1709 计算机。

## <a name="user-cant-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>用户无法登录并收到“身份验证错误”和“CredSSP 加密数据库修正”消息

当用户尝试从任何 Windows Vista SP2 及更高版本或者 Windows Server 2008 SP2 及更高版本登录时，他们被拒绝访问并收到如下所示消息：

```
An authentication error has occurred. The function requested is not supported.
...
This could be due to CredSSP encryption oracle remediation
...
```

“CredSSP 加密 Oracle 修正”是指在 2018 年 3 月、4 月和 5 月发布的一组安全更新。 CredSSP 是一个身份验证提供程序，用于处理其他应用程序的身份验证请求。 在 2018 年 3 月 13 日，“3B”和后续更新解决了一个漏洞，攻击者可能会利用该漏洞来中继用户凭据，以在目标系统上执行代码。

初始更新添加了对新的组策略对象“加密数据库修正”的支持，该对象的可能设置如下：

  - 有漏洞：使用 CredSSP 的客户端应用程序可以回退到不安全的版本，但此行为会将远程桌面透露给攻击者。 使用 CredSSP 的服务接受尚未更新的客户端。
  - 已缓解：使用 CredSSP 的客户端应用程序无法回退到不安全的版本，但使用 CredSSP 的服务接受尚未更新的客户端。
  - 强制更新的客户端：使用 CredSSP 的客户端应用程序无法回退到不安全的版本，使用 CredSSP 的服务不会接受未修补的客户端。 
    > [!NOTE]
    > 在所有远程主机支持最新版本之前，不应部署此设置。

2018 年 5 月 8 日更新已将默认的“加密数据库修正”设置从“有漏洞”更改为“已缓解”。 进行此项更改后，包含更新的远程桌面客户端无法连接到不包含更新的服务器（或尚未重启的已更新服务器）。 有关 CredSSP 更新的详细信息，请参阅 [KB 4093492](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解决此问题，请更新并重启所有系统。 有关更新的完整列表以及有关漏洞的详细信息，请参阅 [CVE-2018-0886 | CredSSP 远程代码执行漏洞](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2018-0886)。

在完成更新之前若要解决此问题，请在 KB 4093492 中查看允许的连接类型。 如果没有可行的替代方法，可以考虑以下方法之一：

- 对于受影响的客户端计算机，请将“加密数据库修正”策略设置回“有漏洞”。 
- 在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“安全性”组策略文件夹中修改以下策略：   
  - 将“要求对远程(RDP)连接使用特定的安全层”设置为“已启用”，并选择“RDP”。   
  - 将“要求使用网络级别身份验证来远程连接的用户进行身份验证”设置为“已禁用”。  
    > [!IMPORTANT]  
    > 更改这些组策略会降低部署的安全性。 建议仅临时使用它们，如果需要使用的话。

有关使用组策略的详细信息，请参阅[修改阻止 GPO](rdp-error-general-troubleshooting.md#modifying-a-blocking-gpo)。

## <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>更新客户端计算机后，某些用户需要登录两次

用户使用运行 Windows 7 或 Windows 10 版本 1709 的计算机登录到远程桌面时，会立即看到再次登录的提示。 如果客户端计算机有以下更新，则会出现此问题：

  - Windows 7：KB 4103718 [2018 年 5 月 8 日 - KB4103718（每月汇总）](https://support.microsoft.com/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709：KB 4103727 [2018 年 5 月 8 日 - KB4103727（OS 内部版本 16299.431）](https://support.microsoft.com/help/4103727/windows-10-update-kb4103727)

若要解决此问题，请确保用户要连接到的计算机（以及 RDSH 或 RDVI 服务器）已完全通过 2018 年 6 月更新包更新。 这包括以下更新：

  - Windows Server 2016：KB 4284880 [2018 年 6 月 12 日 - KB4284880（OS 内部版本 14393.2312）](https://support.microsoft.com/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2：KB 4284815 [2018 年 6 月 12 日 - KB4284815（每月汇总）](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012：KB 4284855 [2018 年 6 月 12 日 - KB4284855（每月汇总）](https://support.microsoft.com/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2：KB 4284826 [2018 年 6 月 12 日 - KB4284826（每月汇总）](https://support.microsoft.com/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2：KB4056564 [Windows Server 2008、Windows Embedded POSReady 2009 和 Windows Embedded Standard 2009 中 CredSSP 远程代码执行漏洞的安全更新说明：2018 年 3 月 13 日](https://support.microsoft.com/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

## <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>在使用 Remote Credential Guard 且包含多个 RD 连接代理的部署中，用户被拒绝访问

如果使用了 Windows Defender Remote Credential Guard，使用两个或更多个远程桌面连接代理的高可用性部署中会出现此问题。 用户无法登录到远程桌面。

出现此问题的原因是 Remote Credential Guard 使用 Kerberos 进行身份验证，并限制 NTLM。 但是，在使用负载均衡的高可用性配置中，RD 连接代理无法支持 Kerberos 操作。

如果需要将高可用性配置与负载均衡的 RD 连接代理配合使用，可以通过禁用 Remote Credential Guard 来解决此问题。 有关如何管理 Windows Defender Remote Credential Guard 的详细信息，请参阅[使用 Windows Defender Remote Credential Guard 保护远程桌面凭据](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。
