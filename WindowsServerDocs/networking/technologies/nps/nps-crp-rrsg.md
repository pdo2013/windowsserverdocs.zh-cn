---
title: 远程 RADIUS 服务器组
description: 本主题概述了网络策略服务器远程 RADIUS 服务器组在 Windows Server 2016 中。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9912927a7b75e4c9f04aa3d24eb7ed46c73a7dd2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855258"
---
# <a name="remote-radius-server-groups"></a>远程 RADIUS 服务器组

>适用于：Windows 服务器 （半年频道），Windows Server 2016

当你将网络策略服务器 (NPS) 配置为远程身份验证拨入用户服务 (RADIUS) 代理时，使用 NPS 将连接请求转发到 RADIUS 服务器能够处理连接请求，因为他们可以执行的身份验证和授权的用户或计算机帐户所在的域中。 例如，如果你想要连接请求转发到不受信任域中的一个或多个 RADIUS 服务器，您可以将 NPS 配置为 RADIUS 代理，以将请求转发到不受信任域中的远程 RADIUS 服务器。

>[!NOTE]
>远程 RADIUS 服务器组是无关，且独立于 Windows 组。

若要将 NPS 配置为 RADIUS 代理，必须创建包含所有所需的 NPS 评估转发哪些消息以及发送消息的位置信息的连接请求策略。

当在 NPS 中配置远程 RADIUS 服务器组和组配置连接请求策略时，指定 NPS 转发连接请求的位置。

## <a name="configuring-radius-servers-for-a-group"></a>配置 RADIUS 服务器组

远程 RADIUS 服务器组是包含一个或多个 RADIUS 服务器的命名的组。 如果配置多台服务器，则可以指定负载平衡设置或者确定代理使用服务器的顺序或跨组以防止过载一个或多个服务器中的所有服务器分发 RADIUS 消息流具有过多连接请求。

组中的每个服务器都具有以下设置。

- **名称或地址**。 每个组成员必须具有在组中的唯一名称。 名称可以是 IP 地址或可解析为其 IP 地址的名称。

- **身份验证和记帐**。 您可以将转发身份验证请求、 记帐请求和 / 或每个远程 RADIUS 服务器组成员。

- **负载均衡**。 优先级设置用于指示组的哪些成员是主服务器 （优先级设置为 1）。 对于具有相同的优先级的组成员，使用权重设置用于计算频率 RADIUS 消息发送到每个服务器。 可以使用其他设置来配置 NPS 检测以及已确定其不可用之后变成可用时组成员首先变成不可用的方式。

配置远程 RADIUS 服务器组后，你可以在身份验证和连接请求策略记帐设置中指定的组。 因此，可以先配置远程 RADIUS 服务器组。 接下来，你可以配置连接请求策略，以使用新配置的远程 RADIUS 服务器组。 或者，可以使用新的连接请求策略向导在创建连接请求策略时创建新的远程 RADIUS 服务器组。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。
