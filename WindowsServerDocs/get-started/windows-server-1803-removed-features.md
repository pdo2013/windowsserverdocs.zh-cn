---
title: Windows Server 版本 1803 - 已删除的功能
description: 了解将在 Windows Server 版本 1803 或未来版本中删除或弃用的功能
ms.prod: windows-server-threshold
ms.mktglfcycl: plan
ms.localizationpriority: medium
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.date: 08/22/2019
ms.openlocfilehash: 8b871d6fa939271c7468a8b51a195539ee268e9c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868288"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1803"></a>从 Windows Server 版本 1803 开始已删除或计划取代的功能

> 适用于：Windows Server 版本 1803

Windows Server 的每一次发布都增加了新的特性和功能；我们偶尔也会删除特性和功能，通常是因为我们增加了更好的选项。 以下是有关 Windows Server 版本 1803 中已删除的特性和功能的详细信息。   

> [!TIP]
> - 可以通过加入 [Windows 预览体验计划](https://insider.windows.com)来提前使用 Windows Server 版本 - 这是测试功能变动的好方法。
> - 对其他版本有疑问？ 请查看 [Windows Server 中已删除或计划取代的功能](../get-started-19/removed-features.md)。

**本列表可能会更改，可能未全部包括每个受影响的特性或功能。** 

## <a name="features-we-removed-in-this-release"></a>已在此版本中删除的功能

我们已从 Windows Server 版本 1803 安装的产品映像中删除了以下特性和功能。 除非你使用了替代方法，否则依赖于这些功能的应用程序或代码都将无法工作。   

| 功能    | 可以改用... |
| ----------- | -------------------- |
| [文件复制服务](https://support.microsoft.com/en-us/help/4025991/windows-server-version-1709-no-longer-supports-frs)|Windows Server 2003 R2 中引入的文件复制服务已替换为 DFS 复制。 需要[将使用 FRS 的任何域控制器全部迁移到使用 SYSVOL 的 DFS 复制](https://blogs.technet.microsoft.com/filecab/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol/)。 |
| Hyper-V 网络虚拟化 (HNV)|[网络虚拟化](../networking/sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)现在作为[软件定义的网络](../networking/sdn/software-defined-networking.md) (SDN) 解决方案的一部分包含在 Windows Server 中，其中还包括网络控制器、软件负载平衡、用户定义的路由和访问控制列表。 |

## <a name="features-were-no-longer-developing"></a>不再开发的功能

我们不再积极开发这些功能，并有可能在未来更新中将其删除。 部分功能已被替换为其他特性或功能，另外一些现在从其他提供源提供。 

>[!NOTE]
> 请注意，下面所述的部分功能和角色不包括在 Server Core 安装选项中，而在 Windows Server 版本 1803 中提供。 它们显示  在 Server 中并提供桌面体验安装选项，我们上次已通过 Windows Server 2016 发布了这些功能，并会在 Windows Server 2019 中再次发布。

如果你有关于建议替换其中任何一项功能方面的反馈，可以使用[反馈中心应用](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

| 功能或角色    | 可以改用... |
| ----------- | --------------------- |
| 业务扫描，也称为“分布式扫描管理 (DSM)”|[扫描管理功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759124\(v%3dws.11\)) 在 Windows Server 2008 R2 中引入，支持安全扫描和企业的扫描程序管理。 我们不再对此功能投入时间和精力，也不提供支持它的设备。 |
| IPv4/6 转换技术（6to4、ISATAP 和直接隧道）|自 Windows 10 版本 1607（周年更新）起，6to4 已默认禁用，自 Windows 10 版本 1703（创建者更新）起，ISATAP 已默认禁用，直接隧道一直被默认禁用。 请改用本机 IPv6 支持。 |
| [MultiPoint Services](../remote/multipoint-services/multipoint-services.md)|我们不再作为 Windows Server 的一部分开发 MultiPoint 服务角色。 MultiPoint Connector 服务通过[按需功能](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)为 Windows Server 和 Windows 10 提供。 可以使用[远程桌面服务](../remote/remote-desktop-services/welcome-to-rds.md)（尤其是远程桌面服务会话主机）提供 RDP 连接。 |
| [脱机符号程序包](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugger-download-symbols)（调试符号 MSI）|我们不再以可下载的 MSI 形式提供符号程序包。 [Microsoft 符号服务器正在转变为一个基于 Azure 的符号存储区](https://blogs.msdn.microsoft.com/windbg/2017/10/18/update-on-microsofts-symbol-server/)。 如果需要 Windows 符号，请连接到 Microsoft 符号服务器在本地缓存符号，或在有 Internet 连接的计算机上通过 SymChk.exe 使用清单文件。 |
| Server Core 安装中的[远程桌面连接代理和远程桌面虚拟化主机](../remote/remote-desktop-services/desktop-hosting-service.md)|大多数远程桌面服务部署通过远程桌面会话主机 (RDSH) 归置这些角色，这需要具有桌面体验的服务器；若要与我们将这些角色更改到其中的 RDSH 保持一致，也需要具有桌面体验的服务器。 我们不再开发这些在 [Server Core 安装](../administration/server-core/what-is-server-core.md)中使用的 RDS 角色。 如果需要[作为远程桌面基础结构的一部分部署这些角色](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，可以[在 Windows Server 2016 桌面体验上进行安装](getting-started-with-server-with-desktop-experience.md)。 <br/><br/>这些角色还包含在 Windows Server 2019 的桌面体验安装选项中。 可以在 [Windows Server 2019 的 Windows 预览体验成员版本](https://docs.microsoft.com/windows-insider/at-work/)中测试它们 - 只需确保选择 LTSC 映像。 |
| [RemoteFX vGPU](../remote/remote-desktop-services/rds-remotefx-vgpu.md)|我们正在开发用于虚拟化环境的新图形加速选项。 还可以使用[离散设备分配 (DDA)](../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md) 作为备选。 |
| 组策略中的[软件限制策略](../identity/software-restriction-policies/software-restriction-policies.md)|可以使用 [AppLocker](https://docs.microsoft.com/windows/security/threat-protection/applocker/applocker-overview) 或 [Windows Defender 应用程序控制](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control)来控制哪些应用用户可以访问内核，以及哪个代码可以在内核中运行，而不是通过组策略使用软件限制策略。 |
| 使用 SAS 结构的共享配置的存储空间|改为部署[存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)。 存储空间直通支持使用 HLK 认证的 SAS 机箱，但是在非共享配置中，如[存储空间直通的硬件要求](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)中所述。 |
| Windows Server Essentials 体验|我们不再为 Windows Server Standard 或 Windows Server Datacenter SKU 开发 Essentials 体验角色。 如果需要面向中小型企业的易于使用的服务器解决方案，请查看我们的新 [Microsoft 365 商业版](https://www.microsoft.com/microsoft-365/business)解决方案，或使用 [Windows Server 2016 Essentials](https://docs.microsoft.com/windows-server-essentials/get-started/get-started)。 |

