---
title: 网络控制器高可用性
description: 可以使用此主题以了解网络控制器高可用性的软件定义网络 (SDN) 在 Windows Server 2016。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f260f3e4d8ca5fcd998824327478c2fbe3c81875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-high-availability"></a>网络控制器高可用性

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解有关高可用性的网络控制器和软件定义网络 \(SDN\) 可扩展性配置。

在您的数据中心中部署 SDN 时，你可以使用网络控制器集中部署监视器和管理多网络元素，包括 RAS 网关、软件负载平衡、租户通信、数据中心防火墙策略、SDN 策略和混合联网策略、等服务质量 \(QoS\) 虚拟网络策略。

因为网络控制器 SDN 管理基础，至关重要网络控制器部署高可用性以及的功能供你向提供轻松地缩放向上或向下网络控制器节点 datacenter 需求。

尽管可以部署作为单机群集网络控制器、高可用性和故障转移你必须部署网络控制器在多台计算机群集最少的三个机。

>[!NOTE]
>你可以在任一服务器计算机上或在虚拟机 \(VMs\) 正在运行 Windows Server 2016 Datacenter edition 部署网络控制器。 如果部署网络控制器在虚拟机的功能，必须在同时运行 Datacenter edition Hyper-V 主机上运行虚拟机的功能。 不适用于 Windows Server 2016 Standard edition 网络控制器。

## <a name="network-controller-as-a-service-fabric-application"></a>作为服务结构应用程序的网络控制器

要实现高可用性和可扩展性，网络控制器依赖服务结构。 服务结构提供分布式的系统平台生成可缩放且可靠，并且易于管理应用程序。

作为平台，服务结构提供所需的生成分布式可缩放系统的功能。 它提供了多个操作系统实例，同步之间情况下，选择了领先、故障检测、负载平衡，等的状态信息举办的服务。

>[!NOTE]
>有关在 Azure 服务结构信息，请参阅[的 Azure 服务结构概述](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)。

当你在多台计算机上部署了网络控制器时，网络控制器在服务结构群集上运行的单个服务结构应用。 你可以通过连接操作系统实例一套从群集服务结构。

多个状态服务结构服务包括网络控制器应用程序。 每个服务负责网络功能，如物理网络管理、虚拟网络管理、防火墙管理或网关管理。 

每个服务结构服务都有一个主要的副本和次要的两个副本。 两个辅助服务副本提供在主副本所在禁用或不可用出于某种原因的情况下高发布时，主要服务副本处理请求。

下图显示五计算机与了网络控制器服务结构群集。 四个服务分布在 5 台机器：防火墙服务、网关服务、软件负载平衡 \(SLB\) 服务和虚拟网络 \(Vnet\) 服务。  每个四个服务包括一个主要服务副本和辅助服务的两个副本。

![网络控制器服务结构群集](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>使用服务结构优势

以下是使用网络控制器群集服务结构主要优势。

### <a name="high-availability-and-scalability"></a>高可用性和可扩展性

由于数据中心网络核心网络控制器，必须同时复原发生故障并可缩放到足以随着时间的推移数据中心网络允许敏捷更改。 以下功能提供这些功能： 

- **Fast 故障转移**。 服务结构提供速度极快故障转移。 多个热门辅助服务副本都始终可用。 如果操作系统实例可用时不由于硬件故障，其中一个辅助副本会立即升级到主的副本。 
- **缩放的灵活性**。 你可以快速、轻松地缩放这些从少数情况下数千实例最可靠的服务，然后再到极少数情况下，具体取决于你资源的需求。 

### <a name="persistent-storage"></a>永久存储

网络控制器应用程序大型存储和有要求其配置的状态。 应用程序还必须可供使用在计划和非计划中断。 为此，服务结构提供是复制、事务和持续的应用商店键值官方商城 \(KVS\)。

### <a name="modularity"></a>模块

使用模块的体系结构，每个网络的服务，如虚拟网络服务和防火墙服务，在作为个别服务的 built\ 旨在网络控制器。 

此应用程序体系结构提供以下好处。

1. 网络控制器的模块化允许为每个受支持服务的独立开发需要不断改进。 例如，软件负载平衡服务可以更新不会影响的任何其他服务或网络控制器的正常工作。
2. 网络控制器的模块化允许添加新的服务，该网络发展。 新的服务可以添加到网络控制器，而不会影响现有的服务。

>[!NOTE]
>在 Windows Server 2016，不支持添加到网络控制器的第三方服务。

服务结构模块化使用服务型号架构来最大化轻松开发、部署和服务的应用程序。

## <a name="network-controller-deployment-options"></a>网络控制器部署选项

若要使用 System Center 虚拟机 Manager \(VMM\) 部署网络控制器，请参阅[设置 VMM 结构中 SDN 网络控制器](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要使用脚本网络控制器部署时，请参阅[部署软件定义的网络的基础结构使用脚本](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要使用 Windows PowerShell 网络控制器部署时，请参阅[使用 Windows PowerShell 部署网络控制器](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

有关网络控制器的详细信息，请参阅[网络控制器](Network-Controller.md)。