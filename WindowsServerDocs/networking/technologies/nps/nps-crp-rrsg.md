---
title: 远程 RADIUS 服务器组
description: 本主题提供网络策略 Server 远程 RADIUS 服务器组在 Windows Server 2016 的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f27b5e501f110a038264cd54d75c8b8f9566a64
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="remote-radius-server-groups"></a>远程 RADIUS 服务器组

>适用于：Windows Server（半年通道），Windows Server 2016

作为远程身份验证拨入用户服务 (RADIUS) 代理配置网络策略 Server (NPS) 时，您将使用 NPS 转发给能够处理的连接请求，因为它们可以在用户或计算机的帐户的域中执行身份验证和授权的 RADIUS 服务器连接请求。 例如，如果你想要转发连接到不受信任的域中的一个或多个 RADIUS 服务器的请求，你可以为 RADIUS 代理转发到中不受信任的域远程 RADIUS 服务器请求配置 NPS。

>[!NOTE]
>远程 RADIUS 服务器组是无关和保存在远离 Windows 组。

配置为 RADIUS 代理 NPS，必须创建一个包含所有所需的 NPS 评估哪些邮件转发和发送电子邮件的位置信息的连接请求策略。

当你在 NPS 配置远程 RADIUS 服务器组并以组配置连接请求策略时，你要指定向前连接请求 NPS 所在的位置。

## <a name="configuring-radius-servers-for-a-group"></a>配置为组 RADIUS 服务器

远程 RADIUS 服务器组是组命名，其中包含一个或多个 RADIUS 服务器。 如果多个服务器配置，你可以指定负载平衡任一确定顺序代理使用服务器或 RADIUS 消息流分发以防止重载连接请求太多的一个或多个服务器组中的所有服务器设置。

每个组中的服务器具有以下设置。

- **名称或地址**。 每个组成员必须组中的一个唯一的名称。 该名称可以 IP 地址或为其 IP 地址可解决的名称。

- **身份验证和帐户**。 你可以将转发身份验证请求、记帐请求，或为每个远程 RADIUS 服务器组成员同时。

- **负载平衡**。 优先设置用于表明的组成员的主服务器（的优先级设置为 1）。 对于具有相同的优先级的组成员，重量设置用于计算频率 RADIUS 消息发送到每个服务器。 你可以使用其他设置配置 NPS 服务器检测组成员首先可用时不和它可用时后已确定不可用的方式。

配置远程 RADIUS 服务器组后，你可以在身份验证和连接请求策略的记帐设置指定的组。 出于此原因，你可以首先配置远程 RADIUS 服务器组。 接下来，您可以配置连接请求策略使用新配置远程 RADIUS 服务器组。 或者，你可以使用新的连接要求策略向导创建连接请求策略时创建一个新远程 RADIUS 服务器组。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。
