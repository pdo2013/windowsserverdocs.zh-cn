---
title: 配置 NPS UDP 端口的信息
description: 你可以使用本主题配置网络策略 Server (NPS) 使用远程身份验证拨入用户服务 (RADIUS) 身份验证和 Windows Server 2016 中的记帐交通端口。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0e703dc6f9083f1e79091a6cee6d1ac58753d12
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-udp-port-information"></a>配置 NPS UDP 端口的信息

>适用于：Windows Server（半年通道），Windows Server 2016

可以使用下面的过程配置网络策略 Server (NPS) 使用远程身份验证拨入用户服务 \(RADIUS\) 身份验证和记帐交通端口。

默认情况下，NPS RADIUS 端口 1812 年、1813 年，1645 年和同时 Internet 协议版本 6 \(IPv6\) 的 1646 年和 IPv4 所有已安装的网络适配器上的流量侦听。

>[!NOTE]
>如果你卸载网络适配器 IPv4 或 IPv6，NPS 如此监视器已卸载协议的 RADIUS 通信。

身份验证的 1812 年和记帐 1813 年端口值是 RADIUS 标准端口，2866 年 Internet 工程任务强制 \(IETF\) 定义。 但是，默认情况下，许多访问服务器用于端口 1645 用于验证请求和 1646 年记帐请求。 无论你决定使用哪个端口号，确保 NPS 和你访问服务器配置为使用相同的。

>[重要]如果你不使用 RADIUS 默认端口号，你必须在本地计算机允许 RADIUS 流量新端口上的防火墙配置例外。 有关详细信息，请参阅[RADIUS 交通配置防火墙](nps-firewalls-configure.md)。

在会员**域管理员**，或等效的最低要求完成此过程。

## <a name="to-configure-nps-udp-port-information"></a>若要配置 NPS UDP 端口的信息 

1. 打开 NPS 主机。
2. 右键单击**网络策略服务器**，然后单击**属性**。
3. 单击**端口**选项卡，然后检查端口的设置。 如果你 RADIUS 身份验证和 RADIUS 会计 UDP 端口有所不同提供的默认值（1812 年 1645 以供验证，并 1813 年和记帐 1646 年）中，键入端口中的设置**身份验证**和**记帐**。
4. 若要进行身份验证或记帐请求使用多个端口设置，请用逗号分隔端口号。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。
