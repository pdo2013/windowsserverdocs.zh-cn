---
title: 网关带宽分配
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: fc59fc9d7dc22b9c5567bae314b4312d76fcff19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406124"
---
# <a name="gateway-bandwidth-allocation"></a>网关带宽分配

>适用于：Windows Server

在 Windows Server 2016 中，IPsec、GRE 和 L3 的单个隧道带宽是网关总容量的比率。 因此，客户可以根据需要从网关 VM 传出的标准 TCP 带宽提供网关容量。

此外，网关上的最大 IPsec 隧道带宽仅限（3/20）客户提供 @no__t 0Gateway 的容量。 例如，如果将网关容量设置为 100 Mbps，则 IPsec 隧道容量将为 150 Mbps。 GRE 和 L3 隧道的等效比率分别为1/5 和1/2。

虽然这适用于大多数部署，但固定比率模型不适用于高吞吐量环境。 即使数据传输速率较高（比如，高于 40 Gbps），最大程度上，最大程度地增加了 SDN 网关隧道，因为存在内部因素。

在 Windows Server 2019 中，对于隧道类型，最大吞吐量是固定的：

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

因此，即使你的网关主机/VM 支持具有较高吞吐量的 Nic，但最大可用隧道吞吐量也是固定的。 此问题的另一个问题是任意过度预配网关，在为网关容量提供非常高的数量时，会发生这种情况。

## <a name="gateway-capacity-calculation"></a>网关容量计算

理想情况下，将网关吞吐量容量设置为可供网关 VM 使用的吞吐量。 例如，如果你有一个网关 VM，并且基础主机 NIC 吞吐量是 25 Gbps，则网关吞吐量也可以设置为 25 Gbps。

如果仅使用网关进行 IPsec 连接，则最大可用固定容量是 5 Gbps。 例如，如果在网关上设置 IPsec 连接，则只能将聚合带宽（传入 + 传出）设置为 5 Gbps。

如果同时使用网关进行 IPsec 和 GRE 连接，则可以预配最多 5 Gbps 的 IPsec 吞吐量或最大 15 Gbps 的 GRE 吞吐量。 例如，如果你预配了 2 Gbps IPsec 吞吐量，则会有 3 Gbps 的 IPsec 吞吐量，可在网关或第9

若要将其放在更数学术语中：

- 网关总容量 = 25 Gbps

- 可用 IPsec 容量总计 = 5 Gbps （固定）

- 可用 GRE 总容量 = 15 Gbps （固定）

- 此网关的 IPsec 吞吐量比率 = 25/5 = 5 Gbps

- 此网关的 GRE 吞吐量比率 = 25/15 = 5/3 Gbps

例如，如果向客户分配 2 Gbps IPsec 吞吐量：

网关上的剩余可用容量 = 网关的总容量– IPsec 吞吐量比已分配的 IPsec 吞吐量（已用容量）

&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-525 – 5 * 2 = 15 Gbps

可在网关上分配的其余 IPsec 吞吐量 

&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t = 3 Gbps

可在网关分配的其余 GRE 吞吐量 = 网关/GRE 吞吐量比的剩余容量 

&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-515 * 3/5 = 9 Gbps

吞吐量比根据网关的总容量而定。 需要注意的一点是，应将总容量设置为可用于网关 VM 的 TCP 带宽。 如果在网关上托管多个 Vm，则必须相应地调整网关的总容量。

此外，如果网关容量小于可用的总隧道容量，则可用的隧道容量总数将设置为 "网关容量"。 例如，如果将网关容量设置为 4 Gbps，则 IPsec、L3 和 GRE 的可用总容量均设置为 4 Gbps，并将每个隧道的吞吐量比率保留为 1 Gbps。

## <a name="windows-server-2016-behavior"></a>Windows Server 2016 行为

Windows Server 2016 的网关容量计算算法保持不变。 在 Windows Server 2016 中，最大 IPsec 隧道带宽限制为（3/20）在网关上 @no__t 0gateway 容量。 GRE 和 L3 隧道的等效比率分别为1/5 和1/2。

如果要从 Windows Server 2016 升级到 Windows Server 2019：

1.  **GRE 和 L3 隧道：** 一旦将网络控制器节点更新到 Windows Server 2019，Windows Server 2019 分配逻辑就会生效

2.  **IPSec 隧道：** Windows Server 2016 网关分配逻辑将继续工作，直到网关池中的所有网关升级到 Windows Server 2019。 对于网关池中的所有网关，必须将 Azure 网关服务设置为 "**自动**"。

>[!NOTE]
>升级到 Windows Server 2019 后，网关可能会过度预配（因为分配逻辑从 Windows Server 2016 更改为 Windows Server 2019）。 在这种情况下，网关上的现有连接将继续存在。 网关的 REST 资源会引发一条警告，指出网关已过度设置。 在这种情况下，应将一些连接移到另一个网关。

---