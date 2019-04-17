---
title: 删除或计划在 Windows Server 2019 中删除的功能
description: 了解有关的特性和功能已删除或计划删除从 Windows Server 2019 开始的信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: jasgro
ms.date: 03/29/2019
ms.localizationpriority: medium
ms.openlocfilehash: 470857616a9b36d238de031b4ccf80a68eff1e61
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279128"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>删除或计划替换启动 Windows Server 2019 的功能

>适用于：Windows Server 2019

Windows Server 的每一次发布都增加了新的特性和功能；我们偶尔也会删除特性和功能，通常是因为我们增加了更好的选项。 下面是有关的特性和功能，我们在 Windows Server 2019 中删除的详细信息。   

> [!TIP]
> - 你可以通过加入 [Windows 预览体验计划](https://insider.windows.com)来提前使用 Windows Server 版本 - 这是测试功能变动的好方法。
> - 对其他版本有疑问？ 查看[Windows Server、 版本 1803年](../get-started/windows-server-1803-removed-features.md) [Windows Server、 版本 1709年](../get-started/removed-features-1709.md)，以及[Windows Server 2016](../get-started/deprecated-features.md)的信息。

**本列表可能会更改，可能未全部包括每个受影响的特性或功能。** 

## <a name="features-we-removed-in-this-release"></a>已在此版本中删除的功能

我们正在从 Windows Server 2019 中安装的产品映像中删除以下特性和功能。 除非你使用了替代方法，否则依赖于这些功能的应用程序或代码都将无法工作。   

|功能    |可以改用...|
|-----------|--------------------
|业务扫描，也称为分布式扫描管理 (DSM)|我们正在删除此安全扫描和扫描仪管理功能-没有设备支持此功能。|
|打印组件-现在可选组件服务器核心安装|在以前版本的 Windows Server，打印组件均已在服务器核心安装选项的默认情况下*禁用*。 我们更改会在 Windows Server 2016 中，默认情况下启用它们。 在 Windows Server 2019，这些打印组件再次默认处于禁用状态的服务器核心。 如果你需要启用打印组件，你可以通过运行**安装 WindowsFeature 打印服务器**cmdlet 执行此操作。|
|Server Core 安装中的[远程桌面连接代理和远程桌面虚拟化主机](../remote/remote-desktop-services/desktop-hosting-service.md)|大多数远程桌面服务部署通过远程桌面会话主机 (RDSH) 归置这些角色，这需要具有桌面体验的服务器；若要与我们将这些角色更改到其中的 RDSH 保持一致，也需要具有桌面体验的服务器。 不再可在[服务器核心安装](../administration/server-core/what-is-server-core.md)中使用这些 RDS 角色。 如果你需要[作为远程桌面基础结构的一部分部署这些角色](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，你可以[将它们具有桌面体验的 Windows Server 上安装](../get-started/getting-started-with-server-with-desktop-experience.md)。 <br/><br/>这些角色还包含在 Windows Server 2019 的桌面体验安装选项中。 |



## <a name="features-were-no-longer-developing"></a>不再开发的功能

我们不再积极开发这些功能，并可能在未来更新中删除它们。 部分功能已被替换为其他特性或功能，另外一些现在从其他提供源提供。 

如果你有关于建议替换其中任何一项功能方面的反馈，可以使用[反馈中心应用](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

|功能    |可以改用...|
|-----------|---------------------|
|HYPER-V 中的密钥存储驱动器|我们不再正在努力 HYPER-V 中的密钥存储驱动器功能。 如果你使用的第 1 代 Vm，查看有关接下来的选项的信息[生成 1 虚拟机虚拟化的安全性](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v)。 如果你要创建新的 Vm 将第 2 代虚拟机使用 TPM 设备更安全的解决方案。 |
|受信任的平台模块 (TPM) 管理控制台|TPM 管理控制台中以前提供的信息目前在[Windows Defender 安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)中的[**设备安全**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)页面上可用。|
|主机保护者服务 Active Directory 证明模式|我们不再开发主机保护者服务 Active Directory 证明模式-改为我们已添加了新的证明模式，[主机密钥证明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，即简单得多，同样为兼容如基于 Active Directory 的证明。  此新模式提供与安装体验、 更简单的管理和 Active Directory 证明比更少的基础结构依赖关系的等效功能。 主机密钥证明有之外的 Active Directory 证明是必需的没有其他硬件要求，因此所有现有系统将保持与新模式兼容。 有关证明选项的详细信息，请参阅[部署受保护的主机](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)。|
|OneSync 服务|OneSync 服务将同步邮件、 日历和用户应用的数据。 我们已添加到 Outlook 应用提供相同的同步的同步引擎。|
|远程差分压缩 API 支持|远程差分压缩 API 支持已同步数据启用与远程源使用压缩技术，最小化通过网络发送的数据量。 这一支持当前未使用任何 Microsoft 产品。|
|WFP 轻量的筛选器交换机扩展|WFP 轻量的筛选器交换机扩展允许开发人员生成[简单网络数据包筛选扩展 HYPER-V 虚拟交换机](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering)。 你可以通过创建一个完整的筛选扩展实现相同的功能。 因此，我们将删除此扩展将来。|

