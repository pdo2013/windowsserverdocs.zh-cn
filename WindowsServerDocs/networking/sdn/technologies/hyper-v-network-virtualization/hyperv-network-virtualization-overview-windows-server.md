---
title: 在 Windows Server 2016 hyper V 网络虚拟化概述
description: 本主题提供 Windows Server 2016 中 HYPER-V 网络虚拟化的概述
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c20f9b2d81ac2d49ed0bbea5f3aca48dea0c50
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>在 Windows Server 2016 hyper V 网络虚拟化概述

>适用于：Windows Server（半年通道），Windows Server 2016

在 Windows Server 2016 和虚拟机管理器中，Microsoft 提供的端到端网络虚拟化解决方案。  有构成了 Microsoft 的网络虚拟化解决方案的五个主要组件：  
  
-   **Windows Azure Windows Server 的包**提供面对门户创建虚拟网络，并管理门户管理虚拟网络租户。  
  
-   **虚拟机管理器**(VMM) 提供的集中的管理网络结构。  
  
-   **Microsoft 网络控制器**提供集中、 可编程点自动化以管理、 配置、 监视和解决你的数据中心中的虚拟和物理网络基础结构。  
  
-   **HYPER-V 网络虚拟化**提供所需虚拟化网络通信的基础结构。  
  
-   **HYPER-V 网络虚拟化网关**提供虚拟和物理网络之间的连接。  
  
本主题介绍概念，并介绍的关键优势和 HYPER-V 网络虚拟化 （整体网络虚拟化解决方案的一部分） 的 Windows Server 2016 的功能。 它将介绍如何网络虚拟化受益这两个专用云霞寻找企业工作负载整合和公开云服务提供商的基础结构即服务 (IaaS)。  
  
有关 Windows Server 2016 中的网络虚拟化更有技术性详细信息，请参阅[HYPER-V 网络虚拟化技术详细信息在 Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md)。  
  
**你意味着**  
  
-   [Hyper V 网络虚拟化概述](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b)(Windows Server 2012 R2)  
  
-   [Hyper V 概述](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [Hyper V 虚拟交换机用来概述](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>功能描述  
虚拟机类似于如何服务器虚拟化 （虚拟机监控程序） 提供的操作系统的"虚拟机"HYPER-V 网络虚拟化使"虚拟网络"（称为 VM 网络）。 网络虚拟化将分离从物理网络基础结构的虚拟网络，并删除 VLAN 和分层 IP 地址分配约束虚拟机预配。 这种灵活性可以轻松客户移至 IaaS 云霞高效宿主和 datacenter 管理员以管理他们的基础结构，同时保持必要多租户隔离、 安全要求，并支持重叠虚拟机 IP 地址。  
  
客户希望无缝延长他们到云中的数据中心。 目前在进行此类无缝混合云体系结构有应对技术挑战。 大障碍客户面部之一是重复使用他们现有网络拓扑 （个子网、 IP 地址、 网络服务和等。） 在云中，并且在其本地资源和其云资源之间桥。  HYPER-V 网络虚拟化提供基本物理网络无关 VM 网络的概念。 使用此 VM 网络上的一个或多个虚拟子网，组成的概念从虚拟网络拓扑分离虚拟机连接到虚拟网络中的物理网络精确的位置。 这样一来，客户可以轻松切换其虚拟子网到云同时保留现有的 IP 地址和拓扑在云中，以便继续运行的现有的服务不知道的物理位置个子网。 也就是说，HYPER-V 网络虚拟化允许无缝混合云。  
  
除了混合云许多组织都合并了他们的数据中心和创建专用云霞内部获取云体系结构的效率和可扩展性好处。 HYPER-V 网络虚拟化允许更好的灵活性和效率为专用云霞通过分离企业单元 （通过使虚拟） 网络拓扑实际物理网络拓扑从。 这种方式，业务设备轻松地共享时正在远离内部专用云和保留现有网络拓扑继续。 操作数据中心团队具有灵活地部署和动态移动不提供更好地运营效率的服务器中断 datacenter 和整体更有效地数据中心中的任意位置的工作负载。  
  
对于工作负载所有者的关键优势是，他们可以现在移动其工作负载"拓扑"到云而不更改其 IP 地址或重写他们的应用程序。 例如，典型三层 LOB 应用组成前端层业务逻辑层，数据库层。 通过策略，HYPER-V 网络虚拟化允许客户载入所有或部分内容的三个层到云中进行构建，同时又使路由拓扑和的 IP 地址的服务 （即虚拟机 IP 地址），而无需要更改应用程序。  
  
对于基础结构所有者虚拟机的位置中更大的灵活性使工作负载任意位置进行移动数据中心中无需更改虚拟机或重新网络配置。 例如 HYPER-V 网络虚拟化使交叉网实时迁移，以便在虚拟机可以实时迁移任意位置，而无需服务中断 datacenter。 实时迁移以前仅限于相同子网虚拟机能定位限制。 跨子网实时迁移允许管理员整合工作负载基于动态资源要求，energy 提高效率，并还容纳而不会中断客户正常运行时间的工作负载的基础结构维护。  
  
## <a name="BKMK_APP"></a>实际应用  
对虚拟化数据中心的成功，IT 部门和承载提供商 （提供商提供也存在或物理服务器租赁） 已经开始提供更灵活虚拟化的基础结构，以使其更轻松地向他们的客户提供点播 server 实例。 这类新服务称为基础结构即服务 (IaaS)。 Windows Server 2016 提供的所有必需的平台功能，以实现企业客户立即专用云霞和转换到一名 IT 作为服务运营模型版本。 Windows Server 2016 2016年还使宿主公共云彩和为其客户提供 IaaS 解决方案。 结合使用时虚拟机管理器和 Windows Azure 包管理 HYPER-V 网络虚拟化策略，Microsoft 将提供一个功能强大的云中的解决方案。  
  
Windows Server 2016 HYPER-V 网络虚拟化提供基于策略、 软件控制网络虚拟化，减少了头顶上飞过时怪脸庞的企业当他们展开专用的 IaaS 海角，并提供云宿主更好的灵活性和可扩展性管理虚拟机，以实现更高版本的资源使用率管理。  
  
已从不同组织的分隔 （专用云） 或不同的客户 （托管云） 虚拟机 IaaS 方案需要安全隔离。 今天的解决方案，虚拟本地网络 (Vlan)，可以显示较大的缺点此方案中。  
  
**Vlan**  
  
当前，Vlan 是大多数组织用于支持地址空间重用租户隔离机制。 VLAN 的以太网帧标题中使用明确标签 (VLAN ID) 和它依赖以太网交换机强制隔离和到具有相同 VLAN ID 网络节点限制流量 与 Vlan 主缺点如下所示：  
  
-   增加了由于生产的麻烦重新配置无意中断的风险切换时，在动态数据中心中的虚拟机或隔离限制移动。  
  
-   在可扩展性的限制，因为 4094 Vlan 最多，典型开关支持不超过 1000 VLAN Id。  
  
-   在的限制中单个 VLAN 节点数，并限制的虚拟机上的物理位置基于位置的单个 IP 子约束。 即使 Vlan 可在站点上进行扩展，整个 VLAN 必须相同子网。  
  
**IP 地址分配**  
  
由 Vlan 缺点，除了虚拟机 IP 地址分配存在问题，其中包括：  
  
-   数据中心网络基础结构的物理位置决定虚拟机 IP 地址。 这样一来，将移动到云通常需要更改服务工作负载的 IP 地址。  
  
-   策略紧密为 IP 地址，如防火墙规则资源发现和目录服务，以及等。 更改 IP 地址，则需要更新的所有关联的策略。  
  
-   虚拟机部署和交通隔离是依赖拓扑。  
  
当数据中心网络管理员计划 datacenter 物理布局时，他们必须做出决定位置的子网将物理放置并路由。 这些决策基于 IP 和以太网影响给定的服务器或已连接到特定机架数据中心中刀口运行虚拟机允许有潜在的 IP 地址的技术。 当虚拟机预配，并且放置在数据中心中时，它必须遵守这些选项和有关 IP 地址的限制。 因此，典型结果是 datacenter 管理员，将新的 IP 地址分配给虚拟机。  
  
有此要求问题是，除了正在地址，没有语义式 IP 地址与关联的信息。 例如，一个子网可能包含给定的服务，也可以是在不同的物理位置。 防火墙规则、 访问控制策略和 IPsec 安全关联通常与程序关联的 IP 地址。 更改 IP 地址强制虚拟机所有者若要调整所有它们都基于原始的 IP 地址的策略。 此重新编号头顶上飞过时是许多企业选择部署到云中进行构建，只需如此便离开旧版应用的唯一的新服务过高。  
  
HYPER-V 网络虚拟化将分离从物理网络基础结构的客户虚拟机虚拟网络。 因此，它使客户虚拟机维护其原始的 IP 地址，同时允许 datacenter 管理员预配无需重新物理 IP 地址或 VLAN Id 配置的客户虚拟机数据中心中的任意位置。 下一步部分总结了主要功能。  
  
## <a name="BKMK_NEW"></a>重要的功能  
下面是关键功能、 好处，和 HYPER-V 网络虚拟化 Windows Server 2016 中的功能的列表：  
  
-   **可以灵活工作负载位置的网络隔离和 IP 地址重新使用，无需 Vlan**  
  
    HYPER-V 网络虚拟化将分离从物理网络基础结构宿主，为提供相应自由工作负载放置在中心内的客户虚拟网络。 虚拟机的工作负载位置不再受限制的 IP 地址分配或物理网络 VLAN 隔离要求因为它实施根据软件定义、 租户虚拟化策略 HYPER-V 主机内。  
  
    虚拟机来自重叠 IP 地址与不同的客户可以现在部署在相同的主服务器无需麻烦 VLAN 配置或违反 IP 地址层次。 这可以优化迁移的客户到共享 IaaS 举办提供程序，使客户能够在不进行修改，其中包括保持不变的虚拟机 IP 地址这些工作负载的工作负载。 承载服务提供商，支持想要扩展到共享 IaaS datacenter 他们现有网络地址空间的许多客户是配置和维护为每个客户，以确保的潜在重叠地址空间并存独立的 Vlan 复杂练习。 与 HYPER-V 网络虚拟化支持重叠的地址变得更加容易网络且需要重新配置较少网络承载提供程序。  
  
    此外，而不会导致客户工作负载下的时间可以完成物理基础结构维护和升级。 与 HYPER-V 网络虚拟化、 虚拟机在特定主机、 机架、 个子网、 VLAN 或整个群集可以而无需更改物理 IP 地址或主要重新配置迁移。  
  
-   **启用轻松进入工作负载的共享的 IaaS 云**  
  
    与 HYPER-V 网络虚拟化 IP 地址和配置虚拟机保持不变。 这使更轻松地从他们的数据中心中将工作负载移动到共享 IaaS 宿主提供商用降至最低重新的工作负载或基础结构工具和策略 IT 部门。 在情况下其中之间两个数据中心是连接，IT 管理员可以继续使用他们的工具，无需重新它们配置。  
  
-   **使实时迁移子网间**  
  
    虚拟机工作负载实时迁移传统由于已被限于同一 IP 子或 VLAN 交叉网所需的虚拟机的来宾操作系统更改它的 IP 地址。 此地址更改折断现有的通信和会打乱虚拟机上运行的服务。 使用 HYPER-V 网络虚拟化工作负载可以实时迁移到服务器而无需更改工作负载 IP 地址的子网不同运行 Windows Server 2016 一个子网在运行 Windows Server 2016 的服务器。 HYPER-V 网络虚拟化确保虚拟机由于实时迁移的位置更改了更新并在具有与迁移虚拟机持续通信主机之间同步。  
  
-   **使更轻松地管理分离服务器和网络管理**  
  
    由于迁移和工作负载的位置的基础物理网络配置独立简化了服务器的工作负载位置。 服务器管理员可以专注于管理服务器、 和服务和网络管理员可以专注于整个网络的基础结构和交通管理。 这使 datacenter server 管理员部署和迁移虚拟机，而无需更改虚拟机的 IP 地址。 那里减轻头顶上飞过时因为 HYPER-V 网络虚拟化允许虚拟机的位置，以独立网络拓扑于发生可减少网络管理员会涉及可能会发生变化隔离限制的位置。  
  
-   **简化了该网络，并改进了服务器中的网络资源使用状况**  
  
    Vlan 坚硬和虚拟机上物理网络基础结构的位置相关性产生超量配置和未充分。 通过中断依赖关系，提高了的虚拟机的工作负载位置的灵活性可以简化网络管理，并改进 server 和网络资源使用状况。 注意 HYPER-V 网络虚拟化物理 datacenter 上下文中支持 Vlan。 例如，datacenter 可能想要在特定 VLAN 上的所有 HYPER-V 网络虚拟化通信。  
  
-   **现有的基础结构和新兴的技术与兼容**  
  
    可以在当今 datacenter 部署 HYPER-V 网络虚拟化但与新兴 datacenter"直网络"技术兼容。  
  
    例如，在 Windows Server 2016 HNV 支持 VXLAN 封装格式和打开 vSwitch 数据库管理协议 (OVSDB) 作为 SouthBound 接口 (SBI)..  
  
-   **提供的互操作性和生态系统准备工作**  
  
    HYPER-V 网络虚拟化跨前提连接、 存储区域网络 （圣）、 非虚拟化资源访问等，如支持多个配置中进行通信使用现有的资源。 Microsoft 致力于与生态系统合作伙伴协作，以支持和增强 HYPER-V 网络虚拟化性能、 可扩展性和可管理性方面的体验。  
  
-   **基于策略配置**  
  
    通过 Microsoft 网络控制器配置 Windows Server 2016 中的网络虚拟化策略。 网络控制器具有 rest 风格 northbound API，以及 Windows PowerShell 接口配置策略。 有关 Microsoft 网络控制器的详细信息，请参阅[网络控制器](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)。  
  
## <a name="BKMK_SOFT"></a>软件要求  
使用 Microsoft 网络控制器 HYPER-V 网络虚拟化需要 Windows Server 2016 和 HYPER-V 角色。  
  
## <a name="BKMK_LINKS"></a>请参阅  
若要了解有关在 Windows Server 2016 HYPER-V 网络虚拟化的详细信息，请参阅以下链接：  
  
|键入的内容|引用|  
|----------------|--------------|  
|**社区资源**|-   [专用云体系结构博客](http://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-提问：[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-VXLAN- [RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**相关的技术**|-   [网络控制器](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Hyper V 网络虚拟化概述](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b)(Windows Server 2012 R2)|  
  


