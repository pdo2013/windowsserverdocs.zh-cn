---
title: NPS 的代理服务器负载平衡
description: 你可以使用此主题以了解有关 Windows Server 2016 和 Windows 10 VPN 特性和功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f00eb6575284a69358c58b36d08672cdd69dc18
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="nps-proxy-server-load-balancing"></a>NPS 的代理服务器负载平衡

适用于：Windows Server 2016

远程身份验证拨入用户服务(RADIUS) 客户端，如虚拟专用网络 (VPN) 服务器和无线接入点的网络访问权限服务器，即创建连接请求并将它们发送到如 NPS RADIUS 服务器。 在某些情况下，NPS 服务器可能会收到过多连接请求一次导致性能下降或重载。 当重载 NPS 服务器时，它是添加到你的网络，并且配置的更多 NPS 服务器负载平衡一个好主意。 当你激增，均匀地分布传入连接请求多个 NPS 服务器之间以免的一个或多个 NPS 服务器重载时，就称为负载平衡。

负载平衡时尤其有用：

- 使用扩展验证协议传输层安全性 \(EAP-TLS\) 或保护可扩展身份验证协议的组织 \ (PEAP\)-TLS 进行身份验证。 因为这些身份验证方法使用为服务器身份验证和用户或客户端计算机身份验证的证书，RADIUS 代理服务器的负担是多于何时使用密码基于身份验证方法。
- 需要维持可用性连续服务的公司。
- Internet 服务提供商 \(ISPs\) 可提供的其他机构 VPN 访问。 外包的 VPN 服务可生成大量的身份验证的交通。

有两种方法可用于平衡发送给你 NPS 服务器的连接要求负载：

- 配置你的网络访问服务器发送到多台 RADIUS 服务器连接请求。 例如，如果你有 20 的无线接入点和两个 RADIUS 服务器、配置发送到这两个 RADIUS 服务器连接请求每次接入点。 你可以负载平衡并提供在每个网络的访问权限服务器故障转移通过配置访问服务器发送到优先级指定订单中的多个 RADIUS 服务器连接请求。 负载平衡这种方法是通常最好不要部署大量 RADIUS 客户端的小型企业。
- 使用多个 NPS 服务器或其他 RADIUS 服务器之间的连接请求平衡 NPS RADIUS 代理配置。 例如，如果你有 100 的无线接入点，一个 NPS 代理，三个 RADIUS 服务器，您可以配置向 NPS 代理发送的所有通信的接入点。 上 NPS 的代理配置负载平衡以便代理激增，均匀地分布三个 RADIUS 服务器之间的连接请求。 有许多 RADIUS 客户端和服务器的中等和大型企业最好负载平衡此方法。

在许多情况下，负载平衡的最佳方法是将 RADIUS 客户端，将请求连接发送到两个 NPS 的代理服务器，并然后配置负载平衡之间 RADIUS 服务器 NPS 代理配置。 这种方法提供故障转移和负载平衡以及代理 NPS RADIUS 服务器。

## <a name="radius-server-priority-and-weight"></a>RADIUS 服务器优先级和粗细

在 NPS 的代理配置过程中，你可以创建远程 RADIUS 服务器组，并将 RADIUS 服务器添加到每组。 若要配置负载平衡，你必须每远程 RADIUS 服务器组多个 RADIUS 服务器。 添加组成员时，或者创建 RADIUS 服务器为组成员后，你可以访问添加 RADIUS 服务器对话框配置负载平衡选项卡上的以下各项：

- **优先级**。 优先级指定重要性 NPS 的代理服务器 RADIUS 服务器的顺序。 必须优先级分配是整数，如 1、2 或 3 平板电脑的值。 低数字，较高优先级 NPS 代理让 RADIUS 服务器。 例如，如果 RADIUS 服务器分配 1 最高优先级，NPS 代理连接请求向 RADIUS 服务器发送首先;如果使用优先级 1 服务器不可用，NPS 则向 RADIUS 服务器具有优先级 2，依此类推发送连接请求。 可以将多个 RADIUS 服务器、指定相同的优先级，并将体重设置加载它们之间的余额。

- **重量**。 此重量设置，以确定多少连接发送到每个请求 NPS 使用组成员时组成员所拥有的相同的优先级。 必须分配体重设置 1 和 100，之间的值和的值代表比例 100%。 例如，如果远程 RADIUS 服务器组包含两个都具有 1 优先级和 50 的粗细分级的成员，NPS 代理转发到每个 RADIUS 服务器的连接请求 50%。

- **高级设置**。 这些故障转移设置提供的方式 NPS 以确定是否远程 RADIUS 服务器不可用。 如果确定有关 NPS RADIUS 服务器不可，它可以开始发送到其他组成员连接请求。 可以使用以下设置配置的数秒 NPS 代理之前，等待 RADIUS 服务器的响应它还会考虑漏译; 请求放置请求之前 NPS 代理最大号来标识为不可用; RADIUS 服务器并的数秒之间请求之前 NPS 代理可以经过标识 RADIUS 服务器为不可用。

## <a name="configure-nps-proxy-load-balancing"></a>配置 NPS 代理负载平衡

配置负载平衡之前, 创建的部署套餐，其中包括多少远程 RADIUS 服务器组需要，哪些服务器是每个组中，并为每个服务器的优先级和粗细设置中的成员。

>[!NOTE]
>按照这些步骤假定已部署和配置 RADIUS 服务器。

若要配置 NPS 用作代理服务器和 RADIUS 客户端到远程 RADIUS 服务器前进连接请求，你必须执行以下操作：

1. 部署 RADIUS 客户 \ （VPN 服务器拨号服务器、 服务器终端服务网关、 802.1 X 身份验证交换机无线、 和 802.1 X 访问 points\） 并将其发送到您 NPS 的代理服务器连接请求配置。

2. NPS 代理上, 配置为 RADIUS 客户端的网络的访问服务器。 有关详细信息，请参阅[将配置客户端 RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure)。

3. 上 NPS 代理，创建一个或多个远程 RADIUS 服务器组。 在此过程中，将 RADIUS 服务器添加到远程 RADIUS 服务器组。 有关详细信息，请参阅[配置远程 RADIUS 服务器组](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure)。

4. 在你添加到远程 RADIUS 服务器组中，每个 RADIUS 服务器的 NPS 代理，依次单击 RADIUS 服务器**负载平衡**选项卡，然后配置**优先级**，**重量**，并**高级设置**。

5. 在 NPS 代理服务器上，连接请求来配置策略转发给远程 RADIUS 服务器组的身份验证和会计请求。 必须创建一个连接请求策略每远程 RADIUS 服务器组。 有关详细信息，请参阅[配置连接请求策略](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure)。


