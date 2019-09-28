---
title: 规划始终启用 VPN 部署
description: 本主题提供有关在 Windows Server 2016 中部署 Always On VPN 的规划说明。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: f92cfdbe13633dd4c59012f566c6888fdc7fc7a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388158"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>步骤 1： 规划 Always On VPN 部署

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**了解用于部署 Always On VPN 的工作流](always-on-vpn-deploy-deployment.md)
- [**一个**步骤 2：配置服务器基础结构](vpn-deploy-server-infrastructure.md)

在此步骤中，你将开始规划和准备 Always On VPN 部署。 在作为 VPN 服务器使用的计算机上安装远程访问服务器角色之前, 请执行以下任务。 进行适当规划后, 可以部署 Always On VPN, 还可以选择使用 Azure AD 配置 VPN 连接的条件性访问。

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>准备远程访问服务器

你必须在用作 VPN 服务器的计算机上执行以下操作：

- **请确保 VPN 服务器软件和硬件配置正确**。 在计划用作远程访问 VPN 服务器的计算机上安装 Windows Server 2016。 此服务器必须安装两个物理网络适配器，一个用于连接到外部外围网络，另一个用于连接到内部外围网络。

- **确定连接到 Internet 的网络适配器以及连接到专用网络的网络适配器**。 将网络适配器配置为带有公共 IP 地址的 Internet，而面向 Intranet 的适配器可以使用本地网络中的 IP 地址。

    >[!TIP]
    >如果不想使用外围网络中的公共 IP 地址，可以使用公共 IP 地址来配置边缘防火墙，然后将防火墙配置为将 VPN 连接请求转发到 VPN 服务器。

- **将 VPN 服务器连接到网络**。 在外围网络、边缘防火墙和外围防火墙之间安装 VPN 服务器。

## <a name="plan-authentication-methods"></a>规划身份验证方法

IKEv2 是[Internet 工程任务团队请求注释 7296](https://datatracker.ietf.org/doc/rfc7296/)中所述的 VPN 隧道协议。 IKEv2 的主要优点是它不必完全了基础网络连接中的中断。 例如，如果连接暂时断开或用户将客户端计算机从一个网络移到另一个网络，则重新建立网络连接 IKEv2 时，IKEv2 会自动恢复 VPN 连接，而无需用户干预。

>[!TIP]
>你可以配置远程访问 VPN 服务器以支持 IKEv2 连接，同时还禁用未使用的协议，从而减少服务器的安全空间。 

## <a name="plan-ip-addresses-for-remote-clients"></a>规划远程客户端的 IP 地址

你可以配置 VPN 服务器，以将地址分配给 VPN 客户端，从你配置的静态地址池或从 DHCP 服务器中的 IP 地址。 

## <a name="prepare-the-environment"></a>准备环境

- **请确保你有权配置外部防火墙并且具有有效的公共 IP 地址**。 打开防火墙上的端口以支持 IKEv2 VPN 连接。 还需要一个公共 IP 地址来接受来自外部客户端的连接。

- **为 VPN 客户端选择一系列静态 IP 地址**。 确定要支持的同时 VPN 客户端的最大数目。 另外，在内部外围网络上规划一系列静态 IP 地址，以满足该要求，即*静态地址池*。 如果使用 DHCP 在内部 DMZ 上提供 IP 地址，则可能还需要为 DHCP 中的这些静态 IP 地址创建排除项。

- **请确保可以编辑公共 DNS 区域**。 将 DNS 记录添加到公共 DNS 域以支持 VPN 基础结构。 

- **确保所有 VPN 用户在 Active Directory 用户（AD DS）中具有用户帐户**。 在用户可以使用 VPN 连接连接到网络之前，他们必须具有 AD DS 中的用户帐户。

## <a name="prepare-routing-and-firewall"></a>准备路由和防火墙 

将 VPN 服务器安装在外围网络中，这会将外围网络划分为内部和外部外围网络。 根据网络环境，可能需要进行几次路由修改。

- **可有可无配置端口转发。** 边缘防火墙必须打开与 IKEv2 VPN 关联的端口和协议 Id，并将其转发到 VPN 服务器。 在大多数环境中，执行此操作要求你配置端口转发。 将通用数据报协议（UDP）端口500和4500重定向到 VPN 服务器。

- **配置路由，以便 DNS 服务器和 VPN 服务器可以访问 Internet**。 此部署使用 IKEv2 和网络地址转换（NAT）。 请确保 VPN 服务器可以访问所有需要的内部网络和网络资源。 从 VPN 服务器无法访问的任何网络或资源也无法通过来自远程位置的 VPN 连接访问。

在大多数环境中，若要访问新的内部外围网络，请调整边缘防火墙和 VPN 服务器上的静态路由。 但在更复杂的环境中，可能需要将静态路由添加到内部路由器，或者调整 VPN 服务器的内部防火墙规则，以及与 VPN 客户端关联的 IP 地址块。

## <a name="next-steps"></a>后续步骤

[步骤 2.配置服务器基础结构](vpn-deploy-server-infrastructure.md)：在此步骤中，将安装和配置支持 VPN 所需的服务器端组件。 服务器端组件包括配置 PKI 以分发用户、VPN 服务器和 NPS 服务器使用的证书。