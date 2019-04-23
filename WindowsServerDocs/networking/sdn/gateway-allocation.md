---
title: 网关带宽分配
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 3c259b96e1a8ee27888a5cccc50b285a2f7cb8c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850428"
---
# <a name="gateway-bandwidth-allocation"></a>网关带宽分配

>适用于：Windows Server

在 Windows Server 2016 中，IPsec、 GRE 和 L3 的单个隧道带宽是总网关容量的比率。 因此，客户可提供基于标准的 TCP 带宽网关 VM 带着这些网关容量。

此外，在网关上的最大 IPsec 隧道带宽是限制为 (3/20)\*客户提供的网关容量。 因此，例如，如果您设置网关容量为 100 Mbps，则 IPsec 隧道容量应 150 Mbps。 GRE 和 L3 隧道等效比率为 1/5 和 1/2，分别。

虽然这对于大多数部署工作，但不是适用于高吞吐量环境固定的比率模型。 甚至当数据传输费率是高时 （比如，高于 40 Gbps），由于内部因素造成的上限的 SDN 网关隧道的最大吞吐量。

在 Windows Server 2019 隧道类型的最大吞吐量为已修复：

-   IPsec = 5 Gbps

-   GRE = 为 15 Gbps

-   L3 = 5 Gbps

因此，即使你的网关主机/VM 支持的吞吐量远 Nic，最大可用隧道吞吐量被固定。 这负责处理的另一个问题随机过度预配网关，这种情况发生时提供极大量的网关容量。

## <a name="gateway-capacity-calculation"></a>网关容量计算

理想情况下，您将网关吞吐量容量设置为供网关 VM 的吞吐量。 因此，例如，如果使用单个网关 VM 和基础主机 NIC 吞吐量为 25 Gbps，网关吞吐量可以设置为 25 Gbps 也。

如果使用仅对 IPsec 连接的网关的最大可用固定的容量为 5 Gbps。 因此，例如，如果预配网关的 IPsec 连接时，您只能预配到聚合带宽 （传入 + 传出） 为 5 gbps;。

如果使用网关的 IPsec 和 GRE 连接，则可以设置最大 5 Gbps 的 IPsec 吞吐量或最大 15 GRE Gbps 吞吐量。 因此，例如，如果预配 2 个 Gbps IPsec 吞吐量，则必须保持预配网关或保留的 9 GRE Gbps 吞吐量的 3 个 Gbps IPsec 吞吐量。

若要将此放在较为复杂的数学术语中：

- 总容量的网关 = 25 Gbps

- 总可用 IPsec 容量 = （固定） 为 5 Gbps

- 总可用 GRE 容量 = （固定） 为 15 Gbps

- 为此网关的 IPsec 吞吐量比率 = 25/5 = 5 Gbps

- 为此网关的 GRE 吞吐量比率 = 25/15 = 5/3 Gbps

例如，如果向客户分配 2 个 Gbps IPsec 吞吐量：

剩余网关上的可用容量 = 总容量的网关 – IPsec 吞吐量比率 * IPsec 吞吐量分配 （已用的容量）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25–5*2 = 15 Gbps

你可以在网关分配的剩余 IPsec 吞吐量 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

你可以在网关分配的剩余 GRE 吞吐量 = 网关/GRE 吞吐量比率的剩余容量 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15*3/5 = 9 Gbps

吞吐量比率各不相同，具体取决于网关的总容量。 需要注意的一点是您应该将总容量设置为网关 VM 可用的 TCP 带宽。 如果必须在网关上托管多个 Vm，必须相应地调整网关的总容量。

此外，如果网关容量小于总可用隧道容量，总可用隧道容量设置为网关容量。 例如，如果您设置网关容量为 4 Gbps 的 IPsec 和 L3，GRE 的总可用容量设置为 4 Gbps，将每个隧道的吞吐量比保留为 1 Gbps。

## <a name="windows-server-2016-behavior"></a>Windows Server 2016 行为

Windows Server 2016 的网关容量计算算法保持不变。 在 Windows Server 2016 中，最大 IPsec 隧道带宽被限制为 (3/20)\*网关的网关容量。 GRE 和 L3 隧道等效比率的 1/5 和 1/2，分别。

如果要从 Windows Server 2016 升级到 Windows Server 2019:

1.  **GRE 和 L3 隧道：** Windows Server 2019 分配逻辑网络控制器节点获取更新为 Windows Server 2019 之后将生效

2.  **IPSec 隧道：** Windows Server 2016 网关分配逻辑将继续工作，直至在网关池的所有网关升级到 Windows Server 2019。 中的网关池的所有网关，必须将 Azure 网关服务设置为**自动**。

>[!NOTE]
>有可能，升级到 Windows Server 2019 后，网关变得过度预配 （如分配逻辑会从 Windows Server 2016 更改为 Windows Server 2019）。 在这种情况下，在网关的现有连接继续存在。 网关 REST 资源将引发网关超配置一条警告。 在这种情况下，应将某些连接移到另一个网关。

---