---
title: 远程桌面-允许访问外部网络您的 PC
description: 了解用于远程访问 PC 的网络外部您的 PC 的选项
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
ms.openlocfilehash: d77c362d9d06b70ad0747002ed8853a39e05b7ff
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708655"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>远程桌面-允许访问您从您的 PC 网络外部的 PC

>应用于： Windows 10，Windows Server 2016

使用远程桌面客户端连接到 PC，当您正在创建的对等连接。 这意味着需要直接访问 PC （有时称为"主机"）。 如果您需要连接到 PC 从网络运行您的 PC 之外，您需要启用访问。 您有几个选项： 使用端口转发或设置 VPN。

## <a name="enable-port-forwarding-on-your-router"></a>在路由器上启用端口转发

端口转发只需映射到的端口和 IP 地址的 PC 您要访问的路由器的 IP 地址 (您公用 IP) 上的端口。 

启用端口转发的具体步骤取决于的路由器您使用，因此您需要联机搜索的路由器的说明。 有关一般讨论的步骤，检查出[wikiHow 到设置 Up 端口转发路由器上](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router)。

映射端口之前您将需要以下各项：

- PC 内部 IP 地址： 查看**设置 > 网络与 Internet > 状态 > 查看您的网络属性**。 查找"操作"状态的网络配置，然后获取**IPv4 地址**。

   ![操作的网络配置](../media/rdclient-operational-network.png)

- (路由器的 IP) 您公共 IP 地址。 有多种方法来查找此-您可以搜索 （在 Bing 或 Google）"我的 IP"或视图[Wi-fi 网络属性](https://binged.it/2Gwob34)（对于 Windows 10)。
- 要映射的端口号。 在大多数情况下，这是 3389-这是使用远程桌面连接的默认端口。
- 为您路由器的管理员访问权限。  

   >[!WARNING]
   > 正在打开最多为 internet PC-请确保已为您的 PC 强密码。

映射端口后，您将能够连接到您的主机 PC 从本地网络外部连接到路由器 （上面的第二项） 的公共 IP 地址。

路由器的 IP 地址可以更改-internet 服务提供商 (ISP) 可以分配您一个新的 IP 在任何时间。 要避免遇到此问题，请考虑使用动态 DNS-这允许您将连接到 PC 使用易于记住域名，而非 IP 地址。 路由器自动更新 DDNS 服务与新的 IP 地址，它应更改。

与大多数路由器中，您可以定义哪些源 IP 或源网络可以使用端口映射。 因此，如果您知道您仅将从工作连接，则可以为工作网络-添加的 IP 地址的允许您避免打开到整个公共 internet 的端口。 如果您使用连接主机使用动态 IP 地址，设置要允许访问该特定 ISP 整个区域的源限制。

以便的内部 IP 地址不会更改，您可能还考虑 PC 上设置为[静态 IP 地址](/windows-hardware/customize/mobile/mcsf/enable-static-ip)。 如果您不执行操作，然后传送器的端口转发将始终指向正确的 IP 地址。


## <a name="use-a-vpn"></a>使用 VPN

如果您使用虚拟专用网络 (VPN) 连接到本地网络，您无需打开您的 PC 到公共 internet。 相反，当您连接到 VPN，远程桌面客户端操作如它是同一个网络的一部分，能够访问您的 PC。 有大量的 VPN 服务可用-您可以查找和使用者为准最适合您。