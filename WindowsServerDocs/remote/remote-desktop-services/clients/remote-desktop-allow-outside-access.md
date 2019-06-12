---
title: 远程桌面-允许对您的 PC 从网络外部进行访问
description: 了解用于远程访问您的 PC 免受外部 PC 的网络选项
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
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805057"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>远程桌面-允许对您的 PC 从你的电脑的网络外部进行访问

>适用于：Windows 10,  Windows Server 2016

当使用远程桌面客户端连接到您的 PC 时，您创建的对等连接。 这意味着您需要直接访问权限 （有时称为"主机"） 的 PC。 如果需要连接到您的 PC 从网络运行您的 PC 之外，您需要启用该访问权限。 有几个选项： 使用端口转发或设置 VPN。

## <a name="enable-port-forwarding-on-your-router"></a>在路由器上启用端口转发

端口转发只需将路由器的 IP 地址 (公共 IP) 上的端口映射到的端口和想要访问的 pc 的 IP 地址。 

启用端口转发的特定步骤取决于的路由器使用的，因此将需要在线搜索您的路由器的说明。 有关步骤的一般讨论，请参阅[wikiHow 到设置了端口转发的路由器上](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router)。

将端口映射之前你将需要以下各项：

- PC 内部 IP 地址：查找范围**设置 > 网络和 Internet > 状态 > 查看您的网络属性**。 查找"操作"状态的网络配置，然后获取**IPv4 地址**。

   ![操作网络配置](../media/rdclient-operational-network.png)

- 公共 IP 地址 (路由器的 IP)。 有很多种，若要查找此-您可以搜索 （在必应或 Google）"我的 IP"或查看[Wi-fi 网络属性](https://binged.it/2Gwob34)（适用于 Windows 10)。
- 要映射的端口号。 在大多数情况下，这是 3389-这是使用远程桌面连接的默认端口。
- 对你的路由器的管理员访问权限。  

   >[!WARNING]
   > 正在打开您到 internet 的电脑-请确保已为您的 PC 设置了强密码。

将端口映射后，您可以通过连接到你的路由器 （上面的第二项） 的公共 IP 地址连接到您的主机 PC 从本地网络之外。

可以更改路由器的 IP 地址-internet 服务提供商 (ISP) 可以分配一个新的 IP 在任何时间。 为了避免遇到此问题，请考虑使用动态 DNS-这样就可以连接到 PC 使用容易记住的域名，而不是 IP 地址。 你的路由器会自动更新 DDNS 服务使用新 IP 地址、 它应更改。

与大多数路由器可以定义的源 IP 或源网络可以使用端口的映射。 因此，如果您知道您只打算从工作进行连接，可以为您工作的网络-添加 IP 地址，可以让你避免打开到整个公共 internet 的端口。 如果你要用于连接的主机使用动态 IP 地址，设置源限制允许从该特定的 ISP 的整个范围进行访问。

您还可以考虑设置[静态 IP 地址](/windows-hardware/customize/mobile/mcsf/enable-static-ip)使内部 IP 地址不会更改在 PC 上。 如果这样做，则路由器的端口转发将始终指向正确的 IP 地址。


## <a name="use-a-vpn"></a>使用 VPN

如果使用虚拟专用网络 (VPN) 连接到本地网络，你无需打开您的 PC 到公共 internet。 相反，当连接到 VPN，远程桌面客户端的作用像它是同一网络的一部分，将无法访问您的 PC。 有多个 VPN 服务-你可以查找和使用者为准最适合您。