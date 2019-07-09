---
title: 远程桌面 - 允许从你的网络外部访问你的电脑
description: 了解用于从电脑网络外部远程访问电脑的选项
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e90a2faa14b65bc766c7d7ec47d5e815658c06e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805057"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>远程桌面 - 允许从电脑网络外部访问电脑

>适用于：Windows 10、Windows Server 2016

当你使用远程桌面客户端连接到你的电脑时，将创建对等连接。 这意味着你需要直接访问电脑（有时称为“主机”）。 如果需要从运行电脑所在的网络外部连接到电脑，则需要启用该访问。 你有两种选择：使用端口转移或设置 VPN。

## <a name="enable-port-forwarding-on-your-router"></a>在路由器上启用端口转移

端口转移直接将路由器 IP 地址（公共 IP）上的端口映射到要访问的电脑的端口和 IP 地址。 

启用端口转移的具体步骤取决于所使用的路由器，因此需要在线搜索路由器说明。 有关这些步骤的一般性讨论，请查看 [wikiHow to Set Up Port Forwarding on a Router](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router)（如何在路由器上设置端口转移）。

在映射端口之前，需提供以下各项：

- 电脑内部 IP 地址：查看“设置”>“网络和 Internet”>“状态”>“查看网络属性”  。 找到状态为“可操作”的网络配置，然后获取 **IPv4 地址**。

   ![可操作的网络配置](../media/rdclient-operational-network.png)

- 公共 IP 地址（路由器的 IP）。 可通过多种方式查找此信息 - 可以搜索（在必应或 Google 中）“我的 IP”或查看 [Wi-Fi 网络属性](https://binged.it/2Gwob34)（适用于 Windows 10）。
- 要映射的端口号。 大多数情况下为 3389 - 这是远程桌面连接使用的默认端口。
- 对路由器的管理员访问权限。  

   >[!WARNING]
   > 你正在向 Internet 开放你的电脑 - 请确保为电脑设置了强密码。

映射端口后，将能够通过连接到路由器的公共 IP 地址（上面第二项），从本地网络外部连接到你的主机。

路由器的 IP 地址可以更改 - 你的 Internet 服务提供商 (ISP) 可以随时为你分配新的 IP。 为避免遇到此问题，请考虑使用动态 DNS - 这样就可以使用容易记住的域名而不是 IP 地址连接到电脑。 如果 IP 地址发生更改，路由器会自动使用新的 IP 地址更新 DDNS 服务。

对于大多数路由器，你可以定义哪个源 IP 或源网络可以使用端口映射。 因此，如果你知道自己只打算在工作时进行连接，则可以添加工作网络的 IP 地址 - 这样就可以避免向整个公共 Internet 开放端口。 如果用于连接的主机使用动态 IP 地址，请设置源限制，以允许从该特定 ISP 的整个范围进行访问。

还可以考虑在电脑上设置[静态 IP 地址](/windows-hardware/customize/mobile/mcsf/enable-static-ip)，以使内部 IP 地址不发生更改。 如果这样做，那么路由器的端口转移将始终指向正确的 IP 地址。


## <a name="use-a-vpn"></a>使用 VPN

如果使用虚拟专用网 (VPN) 连接到局域网，则无需向公共 Internet 开放你的电脑。 相反，当你连接到 VPN 时，RD 客户端就像属于同一网络一样，将能够访问你的电脑。 有许多 VPN 服务可供使用 - 你可以找到并使用最适合你的服务。