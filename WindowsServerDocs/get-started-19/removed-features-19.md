---
title: 删除或计划在 Windows Server 2019 中删除的功能
description: 了解特性和功能中删除或计划删除从 Windows Server 2019。
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
ms.openlocfilehash: 820dfed8a0a58d3ccc64023325c373b761461ba8
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "65976525"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>已删除或计划启动 Windows Server 2019 的替换功能

>适用于：Windows Server 2019

Windows Server 的每一次发布都增加了新的特性和功能；我们偶尔也会删除特性和功能，通常是因为我们增加了更好的选项。 以下是有关功能和我们在 Windows Server 2019 中删除的功能的详细信息。

> [!TIP]
> - 你可以通过加入 [Windows 预览体验计划](https://insider.windows.com)来提前使用 Windows Server 版本 - 这是测试功能变动的好方法。
> - 对其他版本有疑问？ 签出的信息[什么是 Windows Server 中的新增](../get-started/whats-new-in-windows-server.md)。

**列表可能会发生更改，可能不包括每个受影响的功能。** 

## <a name="features-we-removed-in-this-release"></a>已在此版本中删除的功能

我们将从 Windows Server 2019 中的已安装的产品图像的以下特性和功能。 除非你使用了替代方法，否则依赖于这些功能的应用程序或代码都将无法工作。

|功能    |可以改用...|
|-----------|--------------------
|业务扫描，也称为分布式扫描管理 (DSM)|我们要删除此安全扫描和扫描程序管理功能-没有支持此功能的设备。|
|打印组件-现在可选组件的服务器核心安装|在以前版本的 Windows Server 中，打印组件处于*禁用*默认情况下，在服务器核心安装选项。 我们在 Windows Server 2016 中，默认情况下启用这些更改的功能。 在 Windows Server 2019 这些打印将再一次禁用组件默认情况下为 Server Core。 如果你需要启用打印组件，就可以做到通过运行**Install-windowsfeature 打印服务器**cmdlet。|
|Server Core 安装中的[远程桌面连接代理和远程桌面虚拟化主机](../remote/remote-desktop-services/desktop-hosting-service.md)|大多数远程桌面服务部署通过远程桌面会话主机 (RDSH) 归置这些角色，这需要具有桌面体验的服务器；若要与我们将这些角色更改到其中的 RDSH 保持一致，也需要具有桌面体验的服务器。 这些 RDS 角色将不再可用于[服务器核心安装](../administration/server-core/what-is-server-core.md)。 如果你需要[部署这些角色的远程桌面基础结构一部分](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，你可以[它们安装在带桌面体验的 Windows Server 上](../get-started/getting-started-with-server-with-desktop-experience.md)。 <br/><br/>这些角色还包含在 Windows Server 2019 的桌面体验安装选项中。 |

## <a name="features-were-no-longer-developing"></a>不再开发的功能

我们不再主动制订这些功能，并可能会从将来的更新中删除它们。 部分功能已被替换为其他特性或功能，另外一些现在从其他提供源提供。 

如果你有关于建议替换其中任何一项功能方面的反馈，可以使用[反馈中心应用](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

| 功能   | 可以改用... |
|-----------|---------------------|
| 在 HYPER-V 中的密钥存储驱动器|我们不再正致力于在 HYPER-V 中的密钥存储驱动器功能。 如果使用的第 1 代 Vm，请查看[代 1 VM 虚拟化的安全性](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v)今后的选项有关的信息。 如果你是要使用更安全的解决方案的 TPM 设备创建新的 Vm 使用第 2 代虚拟机。 |
| 受信任的平台模块 (TPM) 管理控制台|以前在 TPM 管理控制台中提供的信息现在已接入[**设备安全**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)页[Windows Defender 安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)。 |
| 主机保护者服务 Active Directory 证明模式|我们不会再开发主机保护者服务 Active Directory 证明模式-现在我们已添加了新的证明模式，[托管密钥证明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，这就是简单得多和同样与基于 Active Directory 为兼容证明。  这个新模式提供了等效功能的安装体验、 更简单的管理和 Active Directory 证明比较少的基础结构依赖项。 主机密钥证明具有必需的哪些 Active Directory 证明以外没有其他硬件要求，因此所有的现有系统将保留与新模式兼容。 请参阅[部署受保护的主机](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)证明选项有关的详细信息。 |
| OneSync 服务|OneSync 服务会同步邮件、 日历和人员应用的数据。 我们已添加到的 Outlook 应用程序提供相同的同步的同步引擎。 |
| 远程差分压缩 API 支持|远程差分压缩 API 支持启用与使用压缩技术，通过网络发送的数据量最小化的远程源同步数据。 这种支持当前未由任何 Microsoft 产品使用。 |
| WFP 轻型筛选器交换机扩展|WFP 轻型筛选器交换机扩展开发人员可以构建[简单的网络数据包筛选的 HYPER-V 虚拟交换机扩展](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering)。 可以通过创建完整的筛选扩展来实现相同的功能。 在这种情况下，我们将删除此扩展在将来。 |