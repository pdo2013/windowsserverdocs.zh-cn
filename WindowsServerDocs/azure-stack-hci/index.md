---
title: Azure Stack HCI 概述
description: Azure Stack HCI 是超融合的 Windows Server 2019 群集，使用经验证的硬件在本地运行虚拟化工作负荷，并可选择性地连接到 Azure 服务进行基于云的备份、站点恢复等操作。 Azure Stack HCI 解决方案使用经 Microsoft 验证的硬件来确保最佳性能和可靠性，并提供对 NVMe 驱动器、持久性内存、远程直接内存访问 (RDMA) 网络等技术的支持。
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/22/2019
ms.openlocfilehash: 92d600eeb833cd70bd714702b1fd950c4fe2cd87
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008979"
---
# <a name="azure-stack-hci-overview"></a>Azure Stack HCI 概述

>适用于：Windows Server 2019

Azure Stack HCI 是超融合的 Windows Server 2019 群集，使用经验证的硬件在本地运行虚拟化工作负荷，并可选择性地连接到 Azure 服务进行基于云的备份、站点恢复等操作。 Azure Stack HCI 解决方案使用经 Microsoft 验证的硬件来确保最佳性能和可靠性，并提供对 NVMe 驱动器、持久性内存、远程直接内存访问 (RDMA) 网络等技术的支持。

Azure Stack HCI 是结合了多个产品的解决方案：

- 来自 OEM 合作伙伴的硬件

- Windows Server 2019 Datacenter Edition

- Windows Admin Center

- Azure 服务（可选）

![Azure Stack HCI 是 Microsoft 的超融合解决方案，由范围广泛的硬件合作伙伴提供。](media/AS_HCI_solution.png)

Azure Stack HCI 是 Microsoft 的超融合解决方案，由范围广泛的硬件合作伙伴提供。 请考虑下述超融合解决方案场景，确定 Azure Stack HCI 是否是最符合需求的解决方案：

- **更换老化硬件。** 更换较旧的服务器和存储基础结构，在本地和边缘运行使用现有 IT 技能和工具的 Windows 和 Linux 虚拟机。

- **合并虚拟化工作负荷。** 在高效的超融合基础设施上合并旧版应用。 利用运行超大规模数据中心（例如 Microsoft Azure）时利用的相同类型的云效率。

- **连接到 Azure 以使用混合云服务。** 简化对 Azure 中的云管理服务和安全服务的访问，这些服务包括场外备份、站点恢复、基于云的监视等。

## <a name="the-azure-stack-family"></a>Azure Stack 系列

Azure Stack HCI 是 Azure 和 Azure Stack 系列的一部分，使用与 Azure Stack 相同的软件定义的计算、存储和网络软件。 以下是不同解决方案的核心摘要：

- [Azure](https://azure.microsoft.com) - 使用公有云服务
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) - 在本地运行云服务
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - 在本地运行虚拟化应用，可以选择连接到 Azure

![Azure 和 Azure Stack 运行云服务，而 Azure Stack HCI 在本地运行虚拟化应用程序](media/azure_family.png)

|Azure：使用公有云服务|Azure Stack：在本地运行云服务|Azure Stack HCI：在本地运行虚拟化应用|
|-----------------|-----------------|-----------------|
|方便按需自助服务计算资源迁移现有应用并将其现代化，以及构建新的云原生应用。|在本地使用一致的 Azure 服务构建并运行云应用程序。此操作可以在边缘进行，在断开连接的情况下进行，或者按照监管要求进行。| 在本地运行虚拟化应用程序、更换和合并老化服务器基础结构，以及连接到 Azure 以使用云服务。|
|在全球 54 个区域提供 100 多项服务|Windows 和 Linux 版 Azure VM、Azure Web 应用和 Azure Functions、Azure Key Vault、Azure 资源管理器、Azure 市场、容器、Azure IoT 和事件中心、管理工具（计划、套餐、RBAC）|由 Hyper V 和存储空间直通提供支持的经验证的 HCI 解决方案可以与 Windows Server 2019 和 Windows Admin Center 配合使用，以便进行管理并对 Azure 服务进行集成访问。|

了解详细信息：

- 在 [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) 解决方案网站了解详细信息。
- 观看 Microsoft 专家 Jeff Woolsey 和 Vijay Tewari 的[讨论新 Azure Stack HCI 解决方案](https://aka.ms/AzureStackOverviewVideo)的视频。

## <a name="hyperconverged-efficiencies"></a>超融合效率

Azure Stack HCI 解决方案在符合行业标准的 x86 服务器和组件上将高度虚拟化的计算、存储和网络功能汇集在一起。 将资源组合到同一群集中可以更方便地进行部署、管理和缩放。 自行选择命令行自动化或 Windows Admin Center 进行管理。

使用 Hyper-V（Microsoft 云的基础性虚拟机监控程序技术）和存储空间直通技术（为 NVMe、持久性内存、远程直接内存访问 (RDMA) 网络提供内置支持），针对服务器应用程序实现行业领先的虚拟机性能。

使用受防护的虚拟机、网络微分段以及针对静态数据和传输数据进行的本机加密，确保应用和数据的安全性。

## <a name="hybrid-capabilities"></a>混合功能

可以充分利用能够与公有云中的超融合基础设施平台配合使用的云和本地功能。 你的团队可以开始构建能够以内置方式集成到 Azure 基础结构管理服务的云技能：

- Azure Site Recovery，可以实现高可用性和灾难恢复即服务 (DRaaS)。

- Azure Monitor，一个集中管理的中心，用于跨应用程序、网络和基础结构跟踪发生的事件，并提供 AI 推动的高级分析。

- 云见证，将使用 Azure 作为群集仲裁的轻型中断程序。

- Azure 备份，用于场外数据保护和勒索软件防护。

- Azure 更新管理，可以针对 Azure 中运行的和本地运行的 Windows VM 进行更新评估和更新部署。

- Azure 网络适配器，可通过点到站点 VPN 将本地资源与 Azure 中的 VM 连接。

- 使用 Azure 文件同步将文件服务器与云同步。

有关详细信息，请参阅 [Connecting Windows Server to Azure hybrid services](..\manage\windows-admin-center\azure\index.md)（将 Windows Server 连接到 Azure 混合服务）。

## <a name="management-tools-and-system-center"></a>管理工具和 System Center

Azure Stack HCI 使用与 Azure Stack 相同的虚拟化和软件定义的存储以及网络软件。 有了 Azure Stack HCI 就有了群集的完全管理权限：可以使用 [Windows Admin Center](..\manage\windows-admin-center\overview.md) 和 [System Center](https://www.microsoft.com/cloud-platform/system-center)，还可以使用 [Hyper-V](../virtualization/hyper-v/hyper-v-on-windows-server.md)、[存储空间直通](..\storage\storage-spaces\storage-spaces-direct-overview.md)、PowerShell 和第三方工具（例如 5Nine Manager）的任何功能。

Microsoft Azure 在相同的 Windows Server 以及 Windows Server 中包含的 Hyper-V 平台上运行。 Windows Server 和 System Center 包含来自 Microsoft 在运营全球规模数据中心网络（如 Microsoft Azure）方面的体验的改进和最佳做法，使你可以在使用软件设计的网络技术时部署相同技术来获得灵活性、自动化和控制。

使用 System Center Virtual Machine Management (VMM) 和 System Center Operations Manager 部署和管理基础结构。 可以使用 VMM 预配和管理所需资源，以便创建虚拟机和服务并将其部署到私有云。 可以使用 Operations Manager 监视整个企业的服务、设备和运营，在确定问题后立即行动。

## <a name="hardware-partners"></a>硬件合作伙伴

可以从 15 个合作伙伴处购买经验证的运行 Windows Server 2019 的 Azure Stack HCI 解决方案。 你首选的 Microsoft 合作伙伴可以让你在不需冗长的设计和构建时间的情况下启动并运行应用程序，并为实现和支持服务提供单一联系点。

请访问 [Azure Stack HCI 网站](https://azure.microsoft.com/overview/azure-stack/hci)，查看目前由这些 Microsoft 合作伙伴提供的 70 多个 Azure Stack HCI 解决方案：ASUS、Axellio、bluechip、DataON、Dell EMC、Fujitsu、HPE、Hitachi、Huawei、Lenovo、NEC、primeLine Solutions、QCT、SecureGUARD、Supermicro。

## <a name="faq"></a>常见问题

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>Azure Stack 和 Azure Stack HCI 解决方案的共同点在哪里？

Azure Stack HCI 解决方案提供的基于 Hyper-V 的软件定义的计算、存储和网络技术与 Azure Stack 提供的相同。 两种套餐都符合严格的测试和验证标准，可以确保可靠性并与基础硬件平台兼容。

### <a name="how-are-they-different"></a>它们的区别在哪里？

使用 Azure Stack 时，可以在本地运行云服务。 可以在本地运行 Azure IaaS 和 PaaS 服务，以一致的方式在任何位置构建并运行云应用程序，这些应用程序通过本地 Azure 门户进行管理。

使用 Azure Stack HCI 时，可以在本地运行虚拟化工作负荷，这些工作负荷使用 Windows Admin Center 和你熟悉的 Windows Server 工具进行管理。 对于混合方案（例如基于云的站点恢复、监视等），可以选择连接到 Azure。

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>为什么 Microsoft 要将其 HCI 套餐引入 Azure Stack 系列？

Microsoft 的超融合技术已经成为 Azure Stack 的基础。

许多 Microsoft 客户有着复杂的 IT 环境，我们的目标是根据其业务需求针对其技术提供相应的解决方案。 Azure Stack HCI 是基于 Windows Server 2016 的 Windows Server 软件定义 (WSSD) 解决方案（以前由我们的硬件合作伙伴提供）的进化版。 我们将其引入 Azure Stack 系列是因为我们已开始针对基础结构管理服务提供可以无缝连接 Azure 的新选项。

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>Azure Stack HCI 是否需要连接到 Azure？

否，这完全是可选的。 可以利用与 Azure 的集成来实现混合方案，例如场外备份和灾难恢复、基于云的监视和更新管理，但这些都是可选的。 如果你首选以断开连接的方式运行，或者必须这样做，我们完全理解你的做法，并且会竭诚为你服务。

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>Azure Stack HCI 与 Windows Server 的关系是什么样的？

Windows Server 2019 是几乎所有 Azure 产品的基础。 我们会继续在 Windows Server 中提供所有重要的功能及其相关支持。 建议通过 Azure Stack HCI 在本地部署 HCI，使用我们的合作伙伴提供的经 Microsoft 验证的硬件。

### <a name="will-i-be-able-to-upgrade-from-azure-stack-hci-to-azure-stack"></a>我是否可以从 Azure Stack HCI 升级到 Azure Stack？ 

否，但是客户可以将其工作负荷从 Azure Stack HCI 迁移到 Azure Stack 或 Azure。

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>我可以将哪些 Azure 服务连接到 Azure Stack HCI？

如需可以与 Azure Stack HCI 连接的 Azure 服务的更新列表，请参阅 [Connecting Windows Server to Azure hybrid services](../azure-hybrid-services/index.md)（将 Windows Server 连接到 Azure 混合服务）。

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>Azure Stack HCI 的费用与 Azure Stack 相比如何？ 

Azure Stack 是以完全集成的系统形式销售的，其中包括服务和支持。 你可以从我们的合作伙伴处购买 Azure Stack 作为自行托管的系统，或者作为完全托管的服务。 除了基础系统，在 Azure Stack 或 Azure 上运行的 Azure 服务是在即用即付基础上销售的。

Azure Stack HCI 解决方案遵循传统的购买模型。 可以从 Azure Stack HCI 合作伙伴处购买经验证的硬件，从各种现有的渠道购买软件（Windows Server 2019 Datacenter Edition（具有软件定义的数据中心功能）和 Windows Admin Center）。 对于可以与 Windows Admin Center 配合使用的 Azure 服务，请通过 Azure 订阅付款。

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>如何购买 Azure Stack HCI 解决方案？

请执行下列步骤：

1. 从首选的硬件合作伙伴处购买经 Microsoft 验证的硬件系统。
1. 安装 Windows Server 2019 Datacenter Edition 和 Windows Admin Center，以便进行管理并连接到 Azure 以使用云服务。
1. （可选）使用 Azure 帐户将基于云的管理和安全服务附加到工作负荷。

![若要购买 Azure Stack HCI 解决方案，请选择最符合自己需求的硬件合作伙伴和配置。](media/howbuy_ashci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>比较 Azure Stack 和 Azure Stack HCI

在组织进行数字转型时，你可能会发现，如果使用公有云服务在现代体系结构上进行构建并更换旧版应用，则可加快转型速度。 但是，由于某些原因（包括技术和法规障碍），许多工作负荷必须保留在本地。 可以通过下表来确定哪项 Microsoft 混合云策略符合你的需求并且能够为驻留在任意位置的工作负荷提供云创新。

|Azure Stack|Azure Stack HCI|
|--------|-------|
|新技能，创新的流程|相同的技能，熟悉的流程|
|Azure 服务位于数据中心内|将数据中心连接到 Azure 服务|

### <a name="when-to-use-azure-stack"></a>何时使用 Azure Stack

|Azure Stack|Azure Stack HCI|
|--------|-------|
|将 Azure Stack 用于自助型基础结构即服务 (IaaS)，为多个共置租户提供强大的隔离功能、精确的使用情况跟踪功能以及退款功能。 适用于服务提供商和企业私有云。 模板来自 Azure 市场。|Azure Stack HCI 本身并不会强制实施多租户，也不会规定这么做。|
|使用 Azure Stack 开发并运行依赖于平台即服务 (PaaS) 服务（例如 Web 应用、Functions 或本地事件中心）的应用。 这些服务在 Azure Stack 上的运行方式与在 Azure 中的运行方式完全相同，提供一致的混合开发和运行时环境。|Azure Stack HCI 不在本地运行 PaaS 服务。
|通过 Azure Stack 实现应用部署的现代化，并使用 DevOps 做法进行运营，这些做法包括基础结构即代码、持续集成和持续部署 (CI/CD) 以及各种方便的功能，例如 Azure 一致性 VM 扩展。 适用于开发团队和 DevOps 团队。|Azure Stack HCI 本身并不包含任何 DevOps 工具。

### <a name="when-to-use-azure-stack-hci"></a>何时使用 Azure Stack HCI

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|Azure Stack 需要至少 4 个节点及其自身的网络交换机。|使用 Azure Stack HCI 可以最大程度减少远端机构/分支机构 (ROBO) 的足迹。 一开始只需 2 个服务器节点，进行无交换机背靠背网络连接，特别简单且经济适用。 硬件套餐最低配置为 4 个驱动器、64 GB 内存，每个节点往往不到 1 万美元。
|Azure Stack 约束了 Hyper V 的可配置性和功能集，确保与 Azure 一致。|将 Azure Stack HCI 用于经典企业应用（例如 Exchange、SharePoint 和 SQL Server）的基本 Hyper-V 虚拟化，以及用来虚拟化 Windows Server 角色（例如，文件服务器、DNS、DHCP、IIS、AD）。 不受限制地访问所有 Hyper-V 功能，例如受防护的 VM。|
|Azure Stack 不公开这些基础结构技术。|使用 Azure Stack HCI 将老化的存储阵列或网络设备替换为软件定义的基础结构，不需进行大的重新架构。 内置的存储空间直通和软件定义的网络 (SDN) 允许顺畅地与 Hyper-V 环境集成。|