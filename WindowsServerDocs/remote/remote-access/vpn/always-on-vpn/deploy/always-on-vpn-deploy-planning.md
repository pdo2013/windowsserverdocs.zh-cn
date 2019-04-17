---
title: 规划始终启用 VPN 部署
description: 本主题提供规划部署始终启用 VPN Windows Server 2016 中的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067034"
---
# 步骤 1： 规划始终启用 VPN 部署

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10


& #171; [**上一步：** 了解部署始终启用 VPN 的工作流](always-on-vpn-deploy-deployment.md)<br>
& #187; [**下一步：** 步骤 2。配置服务器基础结构](vpn-deploy-server-infrastructure.md)

在此步骤中，你开始规划和准备始终启用 VPN 部署。 你打算使用为 VPN 服务器的计算机上安装远程访问服务器角色之前，请执行以下任务。 正确的规划之后, 可以部署始终启用 VPN，并 （可选） 配置为使用 Azure AD 的 VPN 连接的条件访问。 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## 准备远程访问服务器

你必须执行以下操作用作 VPN 服务器的计算机上： 

- **请确保 VPN 服务器软件和硬件配置正确无误**。 在你计划使用作为远程访问 VPN 服务器的计算机上安装 Windows Server 2016。 在此服务器必须具有两个物理网络适配器安装到外部外围网络，连接，一个连接到内部外围网络。

- **识别网络适配器连接到 Internet 和的网络适配器连接到你的专用网络**。 配置面向的公共 IP 地址与 Internet 的网络适配器，同时适配器面向 Intranet 可以使用从本地网络的 IP 地址。

    >[!TIP]
    >如果你不希望在外围网络上使用的公共 IP 地址，你可以使用一个公共 IP 地址，配置边缘防火墙，然后配置防火墙以转发到 VPN 服务器的 VPN 连接请求。

- **连接到网络的 VPN 服务器**。 安装在外围网络上，在 edge 防火墙和外围防火墙之间的 VPN 服务器。

## 计划身份验证方法

IKEv2 VPN 隧道协议[Internet 工程任务强制请求 Comments7296](https://datatracker.ietf.org/doc/rfc7296/)中所述。 IKEv2 主要优势在于它能够规避基础的网络连接中断。 例如，如果临时中连接中断或在用户移动的客户端计算机从一个网络到另一个，当重新建立网络连接 IKEv2 VPN 连接自动还原 — 无需用户干预。

>[!TIP]
>你可以将远程访问 VPN 服务器配置为支持 IKEv2 连接，同时还禁用未使用的协议，这减少了服务器的安全占用空间。 

## 规划远程客户端 IP 地址

你可以配置 VPN 服务器分配给 VPN 客户端配置静态地址池中的地址或从 DHCP 服务器的 IP 地址。 

## 准备环境

- **确保你有权配置外部防火墙，并且你有一个有效的公共 IP 地址**。 打开上防火墙来支持 IKEv2 VPN 连接的端口。 你还需要接受来自外部客户端连接的公共 IP 地址。

- **选择用于 VPN 客户端的静态 IP 地址范围**。 确定你想要支持的同时 VPN 客户端的最大数量。 此外，规划内部外围网络以满足该要求，即*静态地址池*上的一系列静态 IP 地址。 如果你使用 DHCP 提供内部外围网络上的 IP 地址，你可能需要在 DHCP 中创建这些静态 IP 地址的排除项。

- **请确保你可以编辑你的公用 DNS 区域**。 公用 DNS 域，支持的 VPN 基础结构添加 DNS 记录。 

- **请确保所有 VPN 用户都具有 Active Directory 用户 \(AD DS\) 中的用户帐户**。 用户可以将连接到 VPN 连接的网络之前，他们必须拥有中添加的用户帐户。

## 准备路由和防火墙 

安装分区为内部和外部外围网络外围网络外围网络内的 VPN 服务器。 具体取决于你的网络环境，你可能需要修改几个路由。

- **\(Optional\) 配置端口转移。** Edge 防火墙必须打开端口和协议与 IKEv2 VPN 关联的 Id，并将其转发到 VPN 服务器。 在大多数环境中，这样做因此需要配置端口转移。 将通用数据报协议 (UDP) 端口 500 和 4500 重定向到 VPN 服务器。

- **配置路由，以便 DNS 服务器和 VPN 服务器可以访问 Internet**。 此部署使用 IKEv2 和网络地址转换 \(NAT\)。 请确保 VPN 服务器可以访问的所有所需的内部网络和网络资源。 任何网络或从 VPN 服务器不可访问的资源不可还通过从远程位置的 VPN 连接。

在大多数环境中，为了访问新的内部外围网络，调整边缘防火墙和 VPN 服务器上的静态路由。 在更复杂的环境中，但是，你可能需要添加静态路由到内部路由器或调整的 VPN 服务器和与 VPN 客户端相关联的 IP 地址的块的内部防火墙规则。

## 下一步
[第 2 步。配置服务器基础结构](vpn-deploy-server-infrastructure.md)： 在此步骤中，你安装和配置支持 VPN 所需的服务器端组件。 服务器端组件，包括配置 PKI 分配使用的用户、 VPN 服务器和 NPS 服务器证书。 

---