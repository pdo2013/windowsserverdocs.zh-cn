---
title: 网络控制器高可用性
description: 可以使用本主题以了解有关高可用性网络控制器的软件定义网络 (SDN) Windows Server 2016 中。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbd3ae9f4c1f1fc3035fae9ace880046312df2f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813348"
---
# <a name="network-controller-high-availability"></a>网络控制器高可用性

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题，了解如何为软件定义的网络的网络控制器高可用性和可伸缩性配置\(SDN\)。

当在你的数据中心中部署 SDN 时，可用于网络控制器集中部署、 监视和管理多个网络元素，其中包括用于租户通信的 RAS 网关，软件负载均衡器、 虚拟网络策略的数据中心防火墙策略、 服务质量\(QoS\) SDN 策略、 混合网络策略和的详细信息。

由于网络控制器是 SDN 管理的基础，至关重要的网络控制器部署，以提供高可用性和功能为您轻松地增加或减少网络控制器节点到你的数据中心的需求。

尽管可以部署为单台计算机群集的网络控制器，对于高可用性和故障转移必须部署网络控制器带有最少三个计算机的多个计算机群集中。

>[!NOTE]
>您可以对服务器计算机或虚拟机上部署网络控制器\(Vm\)的运行的 Windows Server 2016 Datacenter edition。 如果部署网络控制器 Vm 上，必须也运行数据中心版的 HYPER-V 主机上运行虚拟机。 网络控制器不在 Windows Server 2016 标准版上可用。

## <a name="network-controller-as-a-service-fabric-application"></a>作为 Service Fabric 应用程序的网络控制器

若要实现高可用性和可伸缩性，网络控制器依赖于 Service Fabric。 Service Fabric 提供一个分布式的系统平台来构建可缩放、 可靠和易于管理的应用程序。

作为一个平台，Service Fabric 提供所需用于生成可缩放的分布式的系统的功能。 它提供了服务托管在多个操作系统实例中，同步情况下，选择一个领导、 故障检测、 负载平衡，和的详细信息之间的状态信息。

>[!NOTE]
>有关在 Azure 中的 Service Fabric 的信息，请参阅[的 Azure Service Fabric 概述](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)。

在多台计算机上部署网络控制器时，网络控制器上 Service Fabric 群集运行作为单一的 Service Fabric 应用程序。 您可以通过连接一系列的操作系统实例构成 Service Fabric 群集。

网络控制器应用程序包含多个有状态 Service Fabric 服务。 每个服务负责网络功能，例如物理网络管理、 虚拟网络管理、 防火墙管理或网关管理。 

每个 Service Fabric 服务都有一个主副本和两个辅助副本。 主服务副本处理请求，而两个辅助服务副本提供主副本的情况已禁用或不可用出于某种原因中的高可用性。

下图描绘了使用五个计算机的网络控制器 Service Fabric 群集。 四个服务分布在五个机：防火墙服务，网关服务软件负载平衡\(SLB\)服务和虚拟网络\(Vnet\)服务。  每个四个服务包括一个主服务副本和两个辅助服务副本。

![网络控制器 Service Fabric 群集](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>使用 Service Fabric 的优点

以下是将用于网络控制器群集的 Service Fabric 的主要优势在于。

### <a name="high-availability-and-scalability"></a>高可用性和可伸缩性

网络控制器是数据中心网络的核心，因为它必须同时从故障中复原并具有足够的可伸缩性，随着时间的推移，在数据中心网络中允许灵活更改。 以下功能提供了这些功能： 

- **快速故障转移**。 Service Fabric 提供了极快的故障转移。 多个热辅助服务副本将始终可用。 如果由于硬件故障的操作系统实例不可用，一个辅助副本会立即升级到主副本。 
- **缩放灵活性**。 可以轻松快速地缩放极少数情况下最多的数千个实例从这些可靠服务，并再移到几个实例，具体取决于你的资源需求。 

### <a name="persistent-storage"></a>持久存储

网络控制器应用程序具有较大的存储要求为其配置和状态。 应用程序还必须是可用的计划内和计划外停机。 为此，Service Fabric 提供键-值存储\(KVS\)是复制、 事务性和持久性存储区。

### <a name="modularity"></a>模块化

网络控制器设计使用模块化体系结构，与每个网络服务，如生成的虚拟网络服务和防火墙服务\-中作为单个服务。 

此应用程序体系结构提供以下优势。

1. 网络控制器模块化作为允许的每个受支持服务的独立开发需求的发展。 例如，软件负载平衡服务可以更新而不会影响任何其他服务或网络控制器的正常操作。
2. 网络控制器模块化允许添加新的服务，随着网络的发展。 可以向网络控制器添加新服务，而不会影响现有服务。

>[!NOTE]
>在 Windows Server 2016 中不支持添加到网络控制器的第三方服务。

Service Fabric 模块化使用服务模型架构来最大限度地开发、 部署和维护应用程序的易用性。

## <a name="network-controller-deployment-options"></a>网络控制器部署选项

若要使用 System Center Virtual Machine Manager 部署网络控制器\(VMM\)，请参阅[设置 SDN 网络控制器在 VMM 构造中](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要部署网络控制器使用脚本，请参阅[部署软件定义网络基础结构使用的脚本](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要部署网络控制器使用 Windows PowerShell，请参阅[部署网络控制器使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

有关网络控制器的详细信息，请参阅[网络控制器](Network-Controller.md)。