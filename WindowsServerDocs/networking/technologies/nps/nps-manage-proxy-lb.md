---
title: NPS 代理服务器负载平衡
description: 可以使用本主题以了解有关 Windows Server 2016 和 Windows 10 VPN 功能和功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04f466f7646fc5109af7379cab1240d8cd6829ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874398"
---
# <a name="nps-proxy-server-load-balancing"></a>NPS 代理服务器负载平衡

适用于：Windows Server 2016

远程身份验证拨入用户服务 (RADIUS) 客户端，是如虚拟专用网络 (VPN) 服务器和无线访问点的网络访问服务器，创建连接请求，并将其发送到 RADIUS 服务器，例如 NPS。 在某些情况下，NPS 可能会收到连接请求过多，一次导致性能下降或重载。 NPS 是在重载时，它是向网络添加更多 NPSs 并配置负载平衡是个好主意。 如果均匀分布在多个 NPSs 之间有传入的连接请求以防止一个或多个 NPSs 过载，它称为负载平衡。

负载平衡时尤其有用：

- 使用可扩展身份验证协议-传输层安全性的组织\(EAP-TLS\)或受保护的可扩展身份验证协议\(PEAP\)-TLS 进行身份验证。 这些身份验证方法使用证书进行服务器身份验证和用户或客户端计算机身份验证，因此，在 RADIUS 代理和服务器上的负载高于时使用基于密码的身份验证方法。
- 组织需要以保持连续服务可用性。
- Internet 服务提供商\(Isp\)外包为其他组织的 VPN 访问。 外包的 VPN 服务可以生成大量的身份验证流量。

有两种方法可用于对连接请求发送到你 NPSs 的负载进行平衡：

- 配置网络访问服务器将连接请求发送到多个 RADIUS 服务器。 例如，如果您有 20 无线访问点和两个 RADIUS 服务器，每个访问点配置为将连接请求发送到这两个 RADIUS 服务器。 可以进行负载均衡，并通过配置要将连接请求发送到指定的优先级顺序中的多个 RADIUS 服务器的访问服务器提供在每个网络访问服务器的故障转移。 不要部署大量 RADIUS 客户端的小型组织通常最好是这种负载平衡方法。
- 使用 NPS 配置为 RADIUS 代理服务器进行负载平衡多个 NPSs 或其他 RADIUS 服务器之间的连接请求。 例如，如果您有 100 个无线访问点、 一个 NPS 代理服务器和三个 RADIUS 服务器，可以配置要将所有流量都发送到 NPS 代理的访问点。 在 NPS 代理服务器上，配置负载平衡，以便代理均匀分布在三个 RADIUS 服务器之间的连接请求。 此方法的负载平衡适合有多个 RADIUS 客户端和服务器的中型和大型组织。

在许多情况下，负载均衡的最佳方法是配置 RADIUS 客户端将连接请求发送到两台 NPS 代理服务器，然后配置 NPS 代理在 RADIUS 服务器之间平衡负载。 此方法提供了故障转移和负载平衡 NPS 代理和 RADIUS 服务器。

## <a name="radius-server-priority-and-weight"></a>RADIUS 服务器优先级和权重

在 NPS 代理配置过程中，可以创建远程 RADIUS 服务器组并将 RADIUS 服务器添加到每个组。 若要配置负载均衡，必须具有每个远程 RADIUS 服务器组的多个 RADIUS 服务器。 添加组成员时或之后创建作为组成员的 RADIUS 服务器，可以访问添加 RADIUS 服务器对话框中，在负载平衡选项卡上配置以下各项：

- **优先级**。 优先级指定到 NPS 代理服务器的 RADIUS 服务器的重要性的顺序。 必须为整数，如 1、 2 或 3 的值分配优先级别。 越小数值，较高优先级 NPS 代理向 RADIUS 服务器。 例如，如果 RADIUS 服务器分配的最高优先级为 1，NPS 代理将连接请求发送到 RADIUS 服务器第一;如果具有优先级为 1 的服务器不可用，则 NPS 然后将连接请求发送到 RADIUS 服务器具有优先级 2，依此类推。 您可以将相同的优先级分配给多个 RADIUS 服务器，以及如何将权重设置它们之间进行负载平衡。

- **权重**。 当组成员具有相同的优先级别时，NPS 使用此权重设置，以确定多少连接请求发送给每个组成员。 权重设置必须分配一个介于 1 和 100 之间，而值表示百分比为 100%。 例如，如果远程 RADIUS 服务器组包含两个成员都有优先级级别为 1 且权重分级为 50，NPS 代理将转发到每个 RADIUS 服务器的连接请求的 50%。

- **高级设置**。 这些故障转移设置提供 nps 来确定远程 RADIUS 服务器是否不可用的方法。 如果 NPS 确定 RADIUS 服务器不可用，它可以开始将连接请求发送到其他组成员。 可以使用这些设置配置 NPS 代理在考虑删除; 请求之前等待来自 RADIUS 服务器的响应的秒的数最大数被放弃的请求在 NPS 代理标识为不可用; 的 RADIUS 服务器和 NPS 代理之前在请求之间所经过的秒数标识为不可用的 RADIUS 服务器。

## <a name="configure-nps-proxy-load-balancing"></a>配置 NPS 代理负载均衡

在配置负载平衡之前, 创建的部署计划所包含多少远程 RADIUS 服务器组所需的服务器是成员的每个特定组，以及每个服务器的优先级和权重设置。

>[!NOTE]
>后续步骤假定你已部署和配置 RADIUS 服务器。

若要将 NPS 配置为充当代理服务器和远程 RADIUS 服务器转发连接请求从 RADIUS 客户端，必须执行以下操作：

1. 将 RADIUS 客户端部署\(VPN 服务器、 拨号服务器、 终端服务网关服务器、 802.1 X 身份验证交换机和 802.1x 无线访问点\)并将其配置为将连接请求发送到 NPS 代理服务器服务器。

2. 在 NPS 代理服务器上，将配置为 RADIUS 客户端的网络访问服务器。 有关详细信息，请参阅[配置 RADIUS 客户端](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure)。

3. 在 NPS 代理上创建一个或多个远程 RADIUS 服务器组。 在此过程中，添加到远程 RADIUS 服务器组的 RADIUS 服务器。 有关详细信息，请参阅[配置远程 RADIUS 服务器组](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure)。

4. 单击 NPS 代理服务器，您将添加到远程 RADIUS 服务器组，每个 RADIUS 服务器上的 RADIUS 服务器**负载平衡**选项卡，然后配置**优先级**，**权重**并**高级设置**。

5. 在 NPS 代理服务器上，配置连接请求策略，以将身份验证和记帐请求转发到远程 RADIUS 服务器组。 必须创建一个连接请求策略，每个远程 RADIUS 服务器组。 有关详细信息，请参阅[配置连接请求策略](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure)。


