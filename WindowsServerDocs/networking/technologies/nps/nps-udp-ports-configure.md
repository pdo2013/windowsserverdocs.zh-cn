---
title: 配置 NPS UDP 端口信息
description: 本主题可用于配置远程身份验证拨入用户服务 (RADIUS) 身份验证和 Windows Server 2016 中的记帐流量的网络策略服务器 (NPS) 使用的端口。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44c20092180c47e97f1505271203f4491606bbcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851968"
---
# <a name="configure-nps-udp-port-information"></a>配置 NPS UDP 端口信息

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用以下过程配置网络策略服务器 (NPS) 为远程身份验证拨入用户服务使用的端口\(RADIUS\)身份验证和记帐通信。

默认情况下，NPS 将侦听的端口 1812年、 1813年、 1645年和 1646 用于这两个 Internet 协议版本 6 上的 RADIUS 流量\(IPv6\)和适用于所有已安装的网络适配器的 IPv4。

>[!NOTE]
>如果您卸载网络适配器上的 IPv4 或 IPv6，NPS 不会监视已卸载协议的 RADIUS 流量。

身份验证的 1812年和用于记帐的 1813年的端口值是定义由 Internet 工程任务组的 RADIUS 标准端口\(IETF\) Rfc 2865 和 2866年中。 但是，默认情况下，许多访问服务器的记帐请求使用端口 1645 用于身份验证请求和 1646年。 无论你决定使用哪个端口号，请确保 NPS 和访问服务器配置为使用相同的。

>[重要]如果不使用 RADIUS 默认端口号，你必须在本地计算机以允许新端口上的 RADIUS 流量的防火墙上配置例外。 有关详细信息，请参阅[配置对 RADIUS 流量的防火墙](nps-firewalls-configure.md)。

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

## <a name="to-configure-nps-udp-port-information"></a>若要配置 NPS UDP 端口信息 

1. 打开 NPS 控制台。
2. 右键单击**网络策略服务器**，然后单击**属性**。
3. 单击**端口**选项卡，然后检查端口设置。 如果您的 RADIUS 身份验证和 RADIUS 记帐 UDP 端口与提供的默认值 （1812年和 1645 用于身份验证，1813年和 1646 用于记帐） 不同，则键入中的端口设置**身份验证**和**记帐**。
4. 若要进行身份验证或记帐请求使用多个端口设置，请用逗号分隔的端口号。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。
