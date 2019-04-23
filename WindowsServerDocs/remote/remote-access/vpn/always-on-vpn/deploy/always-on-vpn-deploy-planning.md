---
title: 规划始终启用 VPN 部署
description: 本主题提供规划部署 Always On VPN Windows Server 2016 中的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881908"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>步骤 1： 规划 Always On VPN 部署

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10


&#171;  [**上一：** 了解如何部署 Always On VPN 的工作流](always-on-vpn-deploy-deployment.md)<br>
&#187;  [**下一步：** 步骤 2：配置服务器基础结构](vpn-deploy-server-infrastructure.md)

在此步骤中，您开始规划和准备 Always On VPN 部署。 在你计划使用作为 VPN 服务器的计算机上安装远程访问服务器角色之前，请执行以下任务。 正确的规划之后, 可以部署 Always On VPN，并 （可选） 配置的 VPN 连接使用 Azure AD 条件性访问。 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="prepare-the-remote-access-server"></a>准备远程访问服务器

必须执行以下操作作为 VPN 服务器使用的计算机上： 

- **请确保 VPN 服务器软件和硬件配置是否正确**。 你打算使用作为远程访问 VPN 服务器的计算机上安装 Windows Server 2016。 此服务器必须安装两个物理网络适配器，一个用于连接到外部外围网络，一个连接到内部外围网络。

- **确定哪个网络适配器连接到 Internet 和哪个网络适配器连接到专用网络**。 配置面向 Internet 与公共 IP 地址，而面向 Intranet 的适配器可以使用从本地网络的 IP 地址的网络适配器。

    >[!TIP]
    >如果不想在外围网络上使用的公共 IP 地址，你可以使用公共 IP 地址，配置边缘防火墙，然后配置将 VPN 连接请求转发到 VPN 服务器的防火墙。

- **连接到网络的 VPN 服务器**。 安装在外围网络上，在边缘防火墙与外围防火墙之间的 VPN 服务器。

## <a name="plan-authentication-methods"></a>规划身份验证方法

IKEv2 是 VPN 隧道协议中所述[Internet 工程任务组为请求注释 7296](https://datatracker.ietf.org/doc/rfc7296/)。 IKEv2 的主要优点是它不必完全在基础网络连接中断。 例如，如果临时服务器的连接断开或用户将从客户端计算机移到另一个，当重新建立网络连接 IKEv2 的一个网络的 VPN 连接会自动还原，而无需用户干预。

>[!TIP]
>可以配置远程访问 VPN 服务器以支持建立的 IKEv2 连接，同时还禁用未使用的协议，从而降低服务器的安全占用空间。 

## <a name="plan-ip-addresses-for-remote-clients"></a>规划远程客户端的 IP 地址

可以配置 VPN 服务器分配给 VPN 客户端从你配置的静态地址池中的地址或 DHCP 服务器的 IP 地址。 

## <a name="prepare-the-environment"></a>准备环境

- **请确保您具有权限才能配置外部防火墙并且具有有效的公共 IP 地址**。 若要支持 IKEv2 VPN 连接的防火墙上打开的端口。 您还需要为接受来自外部客户端连接的公共 IP 地址。

- **为 VPN 客户端选择的静态 IP 地址范围**。 确定你想要支持的同时进行 VPN 客户端的最大数目。 此外，若要符合这一要求，即内部外围网络上计划的静态 IP 地址范围*静态地址池*。 如果使用 DHCP 提供内部外围网络上的 IP 地址，可能还需要在 DHCP 中创建排除这些静态 IP 地址。

- **请确保您可以编辑在公用 DNS 区域**。 将 DNS 记录添加到你的公共 DNS 域支持的 VPN 基础结构。 

- **请确保所有 VPN 用户都拥有用户帐户的 Active Directory 用户\(AD DS\)**。 用户可以连接到使用 VPN 连接网络之前，它们必须在 AD DS 中具有用户帐户。

## <a name="prepare-routing-and-firewall"></a>准备路由和防火墙 

安装在外围网络中，分区在外围网络到内部和外部外围网络的 VPN 服务器。 具体取决于您的网络环境，可能需要进行多个路由的修改。

- **\(可选\)配置端口转发。** 边缘防火墙必须打开的端口和协议与 IKEv2 VPN 相关联的 Id，并将其转发到 VPN 服务器。 在大多数环境中，这样做因此要求您配置端口转发。 将通用数据报协议 (UDP) 端口 500 和 4500 重定向到 VPN 服务器。

- **配置路由，以便在 DNS 服务器和 VPN 服务器可以连接到互联网**。 此部署使用 IKEv2 和网络地址转换\(NAT\)。 请确保在 VPN 服务器可以访问所有所需的内部网络和网络资源。 任何网络或从 VPN 服务器无法访问的资源是也无法访问通过 VPN 连接从远程位置。

在大多数环境中，若要访问新的内部外围网络，调整边缘防火墙和 VPN 服务器上的静态路由。 在更复杂环境中，但是，您可能需要将静态路由添加到内部路由器或调整 VPN 服务器和 VPN 客户端与关联的 IP 地址块的内部防火墙规则。

## <a name="next-step"></a>下一步
[第 2 步。配置服务器基础结构](vpn-deploy-server-infrastructure.md):此步骤中，安装并配置支持的 VPN 所必需的服务器端组件。 服务器端组件包括配置要分发所用的用户、 VPN 服务器和 NPS 服务器证书的 PKI。 

---