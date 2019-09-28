---
title: NPS 代理服务器负载平衡
description: 您可以使用本主题来了解 Windows Server 2016 和 Windows 10 VPN 的特性和功能。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d138b9891fb3cfa8e15060be312ff945942c660
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405424"
---
# <a name="nps-proxy-server-load-balancing"></a>NPS 代理服务器负载平衡

适用于：Windows Server 2016

远程身份验证拨入用户服务 (RADIUS) 客户端是网络访问服务器, 例如虚拟专用网络 (VPN) 服务器和无线访问点, 创建连接请求并将其发送到 NPS 等 RADIUS 服务器。 在某些情况下，NPS 一次可能会收到太多连接请求，导致性能下降或过载。 在重载 NPS 时，最好将更多 NPSs 添加到网络并配置负载平衡。 当你在多个 NPSs 之间平均分配传入连接请求以防止一个或多个 NPSs 重载时，它称为 "负载均衡"。

负载平衡特别适用于：

- 使用可扩展的身份验证协议-传输层安全性 \(EAP-TLS @ no__t 或受保护的可扩展身份验证协议的组织 \(PEAP @ no__t-TLS 用于身份验证。 由于这些身份验证方法使用证书进行服务器身份验证，并使用用户或客户端计算机身份验证，因此在使用基于密码的身份验证方法时，RADIUS 代理服务器和服务器上的负载要多很多。
- 需要维持服务持续可用性的组织。
- Internet 服务提供商 \(ISPs @ no__t，用于为其他组织外包 VPN 访问权限。 外包 VPN 服务可以生成大量的身份验证流量。

可以使用两种方法来平衡发送到 NPSs 的连接请求的负载：

- 将网络访问服务器配置为将连接请求发送到多个 RADIUS 服务器。 例如，如果有20个无线访问点和两个 RADIUS 服务器，请将每个访问点配置为将连接请求发送到两个 RADIUS 服务器。 可以通过将访问服务器配置为按指定的优先级顺序将连接请求发送到多个 RADIUS 服务器，来实现负载平衡并在每个网络访问服务器上提供故障转移。 此负载平衡方法通常最适用于不部署大量 RADIUS 客户端的小型组织。
- 使用配置为 RADIUS 代理的 NPS 在多个 NPSs 或其他 RADIUS 服务器之间对连接请求进行负载均衡。 例如，如果你有100无线访问点、一个 NPS 代理和三个 RADIUS 服务器，你可以将访问点配置为将所有流量发送到 NPS 代理。 在 NPS 代理上，配置负载均衡，使代理在三个 RADIUS 服务器之间平均分配连接请求。 此负载平衡方法最适用于具有多个 RADIUS 客户端和服务器的中型和大型组织。

在许多情况下，负载均衡的最佳方法是将 RADIUS 客户端配置为将连接请求发送到两个 NPS 代理服务器，然后将 NPS 代理配置为在 RADIUS 服务器之间进行负载平衡。 此方法为 NPS 代理和 RADIUS 服务器提供了故障转移和负载平衡。

## <a name="radius-server-priority-and-weight"></a>RADIUS 服务器优先级和权重

在 NPS 代理配置过程中，你可以创建远程 RADIUS 服务器组，然后将 RADIUS 服务器添加到每个组。 若要配置负载均衡，每个远程 RADIUS 服务器组必须有多个 RADIUS 服务器。 添加组成员时，或创建 RADIUS 服务器作为组成员后，可以访问 "添加 RADIUS 服务器" 对话框，在 "负载平衡" 选项卡上配置以下各项：

- **优先级**。 优先级指定 RADIUS 服务器对 NPS 代理服务器的重要性顺序。 必须为优先级分配一个整数值，如1、2或3。 数字越低，NPS 代理提供给 RADIUS 服务器的优先级就越高。 例如，如果为 RADIUS 服务器分配了优先级最高的1，则 NPS 代理首先向 RADIUS 服务器发送连接请求;如果优先级为1的服务器不可用，则 NPS 会将连接请求发送到优先级为2的 RADIUS 服务器，依此类推。 你可以为多个 RADIUS 服务器分配相同的优先级，然后使用权重设置来实现它们之间的负载平衡。

- **权重**。 NPS 使用此权重设置来确定当组成员具有相同的优先级级别时要向每个组成员发送多少个连接请求。 必须为权重设置分配一个介于1和100之间的值，该值表示 100% 的百分比。 例如，如果远程 RADIUS 服务器组包含两个优先级均为1且权重级别为50的成员，则 NPS 代理会将 50% 的连接请求转发给每个 RADIUS 服务器。

- **高级设置**。 这些故障转移设置为 NPS 提供了一种方法，用于确定远程 RADIUS 服务器是否不可用。 如果 NPS 确定 RADIUS 服务器不可用，它可以开始向其他组成员发送连接请求。 使用这些设置，你可以配置 NPS 代理在认为请求被丢弃之前等待 RADIUS 服务器响应的秒数;NPS 代理将 RADIUS 服务器标识为不可用之前的最大丢弃请求数;在 NPS 代理将 RADIUS 服务器标识为不可用之前，请求之间的间隔秒数。

## <a name="configure-nps-proxy-load-balancing"></a>配置 NPS 代理负载均衡

在配置负载平衡之前，请创建一个部署计划，其中包括你需要多少个远程 RADIUS 服务器组、哪些服务器是每个特定组的成员以及每个服务器的优先级和权重设置。

>[!NOTE]
>下面的步骤假定你已部署并配置了 RADIUS 服务器。

若要将 NPS 配置为充当代理服务器并将 RADIUS 客户端的连接请求转发到远程 RADIUS 服务器，则必须执行以下操作：

1. 部署 RADIUS 客户端 @no__t 0VPN 服务器、拨号服务器、终端服务网关服务器、802.1 X 身份验证交换机和 802.1 X 无线访问点 @ no__t，并将其配置为将连接请求发送到 NPS 代理服务器。

2. 在 NPS 代理上，将网络访问服务器配置为 RADIUS 客户端。 有关详细信息，请参阅[配置 RADIUS 客户端](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure)。

3. 在 NPS 代理上，创建一个或多个远程 RADIUS 服务器组。 在此过程中，将 RADIUS 服务器添加到远程 RADIUS 服务器组。 有关详细信息，请参阅[配置远程 RADIUS 服务器组](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure)。

4. 在 NPS 代理上，对于添加到远程 RADIUS 服务器组的每个 RADIUS 服务器，单击 "RADIUS 服务器**负载平衡**" 选项卡，然后配置 **"优先级**"、"**权重**" 和 "**高级" 设置**。

5. 在 NPS 代理上，配置连接请求策略，以将身份验证和记帐请求转发到远程 RADIUS 服务器组。 必须为每个远程 RADIUS 服务器组创建一个连接请求策略。 有关详细信息，请参阅[配置连接请求策略](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure)。


