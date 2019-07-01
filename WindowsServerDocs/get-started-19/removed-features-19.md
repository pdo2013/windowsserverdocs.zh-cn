---
title: Windows Server 2019 中已删除或计划删除的功能
description: 了解从 Windows Server 2019 开始已删除或计划删除的功能。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: jasongerend
ms.author: jgerend
manager: jasgro
ms.date: 05/21/2019
ms.localizationpriority: medium
ms.openlocfilehash: a27fb6bfa0d96d7d3cdd0b9b5bc4912b12b4462b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280360"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>从 Windows Server 2019 开始已删除或计划取代的功能

>适用于：Windows Server 2019

Windows Server 的每一次发布都增加了新的特性和功能；我们偶尔也会删除特性和功能，通常是因为我们增加了更好的选项。 以下是有关 Windows Server 2019 中已删除的功能的详细信息。

> [!TIP]
> - 你可以通过加入 [Windows 预览体验计划](https://insider.windows.com)来提前使用 Windows Server 版本 - 这是测试功能变动的好方法。
> - 对其他版本有疑问？ 查看 [Windows Server 中的新增功能](../get-started/whats-new-in-windows-server.md)的信息。

**本列表可能会更改，可能未全部包括每个受影响的功能。** 

## <a name="features-we-removed-in-this-release"></a>已在此版本中删除的功能

我们即将从 Windows Server 2019 安装的产品映像中删除以下功能。 除非你使用了替代方法，否则依赖于这些功能的应用程序或代码都将无法工作。

|功能    |可以改用...|
|-----------|--------------------
|业务扫描，也称为分布式扫描管理 (DSM)|我们即将删除此安全扫描和扫描程序管理功能 - 没有任何设备支持此功能。|
|打印组件 - 现在为 Server Core 安装提供可选的组件|在以前 Windows Server 版本中，打印组件默认已在 Server Core 安装选项中禁用  。 我们已在 Windows Server 2016 中更改此设置，默认会启用打印组件。 在 Windows Server 2019 中，Server Core 安装再一次默认禁用这些打印组件。 如果需要启用打印组件，可以运行 **Install-WindowsFeature Print-Server** cmdlet。|
|Server Core 安装中的[远程桌面连接代理和远程桌面虚拟化主机](../remote/remote-desktop-services/desktop-hosting-service.md)|大多数远程桌面服务部署通过远程桌面会话主机 (RDSH) 归置这些角色，这需要具有桌面体验的服务器；若要与我们将这些角色更改到其中的 RDSH 保持一致，也需要具有桌面体验的服务器。 不再提供可在 [Server Core 安装](../administration/server-core/what-is-server-core.md)中使用的这些 RDS 角色。 如果你需要[作为远程桌面基础结构的一部分部署这些角色](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，可以[在 Windows Server 桌面体验上进行安装](../get-started/getting-started-with-server-with-desktop-experience.md)。 <br/><br/>这些角色还包含在 Windows Server 2019 的桌面体验安装选项中。 |

## <a name="features-were-no-longer-developing"></a>不再开发的功能

我们不再积极开发这些功能，并有可能在未来更新中将其删除。 部分功能已被替换为其他特性或功能，另外一些现在从其他提供源提供。 

如果你有关于建议替换其中任何一项功能方面的反馈，可以使用[反馈中心应用](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

| 功能   | 可以改用... |
|-----------|---------------------|
| Hyper-V 中的密钥存储驱动器|我们不再开发 Hyper-V 中的密钥存储驱动器功能。 如果你使用的是第 1 代 VM，请查看[第 1 代 VM 虚拟化安全性](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v)来了解今后的选项。 如果你正在创建新的 VM，请结合 TPM 设备使用第 2 代虚拟机，以获得更安全的解决方案。 |
| 受信任的平台模块 (TPM) 管理控制台|以前在 TPM 管理控制台中提供的信息现在会在 [Windows Defender 安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)的[**设备安全性**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)页中提供。 |
| 主机防护服务 Active Directory 证明模式|我们不再开发主机防护服务 Active Directory 证明模式 - 而是添加了一种新的证明模式[主机密钥证明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，与基于 Active Directory 的证明相比，此模式要简单得多，且兼容性相当。  与 Active Directory 证明相比，此新模式为安装体验提供等效的功能、更简单的管理和更少的基础结构依赖关系。 主机密钥证明的硬件要求不比 Active Directory 证明更高，因此，所有现有系统可与新模式保持兼容。 有关证明选项的详细信息，请参阅[部署受保护的主机](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)。 |
| OneSync 服务|OneSync 服务可以同步邮件、日历和人员应用的数据。 我们已在 Outlook 应用中添加了一个同步引擎用于提供相同的同步。 |
| 远程差异压缩 API 支持|借助远程差异压缩 API 支持，可以使用压缩技术来实现与远程源同步数据，最大程度地减少通过网络发送的数据量。 |
| WFP 轻型筛选交换机扩展|WFP 轻型筛选交换机扩展可让开发人员构建 [Hyper-V 虚拟交换机的简单网络数据包筛选扩展](https://docs.microsoft.com/windows-hardware/drivers/network/using-virtual-switch-filtering)。 可以通过创建完整的筛选扩展来实现相同的功能。 因此，我们将来会删除此扩展。 |
