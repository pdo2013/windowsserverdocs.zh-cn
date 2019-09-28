---
title: 远程 RADIUS 服务器组
description: 本主题概述了 Windows Server 2016 中的网络策略服务器远程 RADIUS 服务器组。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63a6eb5f0f78ed8dbcc0144602f16274fd6ec213
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396311"
---
# <a name="remote-radius-server-groups"></a>远程 RADIUS 服务器组

>适用于：Windows Server（半年频道）、Windows Server 2016

将网络策略服务器（NPS）配置为远程身份验证拨入用户服务（RADIUS）代理时，可以使用 NPS 将连接请求转发到能够处理连接请求的 RADIUS 服务器，因为它们可以执行用户或计算机帐户所在的域中的身份验证和授权。 例如，如果你想要将连接请求转发到不受信任的域中的一个或多个 RADIUS 服务器，你可以将 NPS 配置为 RADIUS 代理，以将请求转发到不受信任的域中的远程 RADIUS 服务器。

>[!NOTE]
>远程 RADIUS 服务器组与 Windows 组无关并独立于它们。

若要将 NPS 配置为 RADIUS 代理，您必须创建一个连接请求策略，该策略包含 NPS 评估哪些消息要转发的消息以及将消息发送到的位置所需的所有信息。

当你在 NPS 中配置远程 RADIUS 服务器组，并使用组配置连接请求策略时，将指定 NPS 转发连接请求的位置。

## <a name="configuring-radius-servers-for-a-group"></a>为组配置 RADIUS 服务器

远程 RADIUS 服务器组是一个命名组，其中包含一个或多个 RADIUS 服务器。 如果配置了多台服务器，则可以指定负载平衡设置，以确定代理使用服务器的顺序，或将 RADIUS 消息的流分散到组中的所有服务器，以防止重载一个或多个服务器连接请求太多。

该组中的每个服务器都具有以下设置。

- **名称或地址**。 每个组成员必须在组内具有唯一的名称。 名称可以是可解析为 IP 地址的 IP 地址或名称。

- **身份验证和计帐**。 可以将身份验证请求和/或记帐请求转发到每个远程 RADIUS 服务器组成员。

- **负载均衡**。 优先级设置用于指示组的哪个成员是主服务器（优先级设置为1）。 对于具有相同优先级的组成员，权重设置用于计算 RADIUS 消息发送到每台服务器的频率。 你可以使用其他设置来配置 NPS 在组成员首次变为不可用时的检测方法，以及在确定其不可用后该成员变为可用的方式。

配置远程 RADIUS 服务器组之后，可以在连接请求策略的 "身份验证和记帐" 设置中指定组。 因此，你可以先配置远程 RADIUS 服务器组。 接下来，你可以将连接请求策略配置为使用新配置的远程 RADIUS 服务器组。 或者，你可以在创建连接请求策略时使用新建连接请求策略向导创建新的远程 RADIUS 服务器组。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。
