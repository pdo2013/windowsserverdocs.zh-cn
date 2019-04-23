---
title: 配置连接请求策略
description: 本主题提供有关如何在 Windows Server 2016 中的网络策略服务器中配置连接请求策略的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f80f9fb8be0c44cfb5685e5b9cc489282e4961d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884508"
---
# <a name="configure-connection-request-policies"></a>配置连接请求策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于创建和配置连接请求策略，指定是否本地 NPS 处理连接请求，或将其转发到远程 RADIUS 服务器进行处理。

连接请求策略是一系列的条件和设置，允许网络管理员指定哪些远程身份验证拨入用户服务 (RADIUS) 服务器执行身份验证和授权连接请求运行网络策略服务器的服务器\(NPS\)接收来自 RADIUS 客户端。

默认连接请求策略将 NPS 用作 RADIUS 服务器，并处理所有身份验证请求本地。

若要配置运行 NPS 充当 RADIUS 代理，并将连接请求转发到其他 NPS 或 RADIUS 服务器的服务器，必须配置远程 RADIUS 服务器组除了添加新的连接请求策略，指定条件和设置，连接请求必须匹配。

在使用新的连接请求策略向导创建新的连接请求策略时，可以创建新的远程 RADIUS 服务器组。

如果不希望 NPS 充当 RADIUS 服务器和进程连接本地请求，则可以删除默认连接请求策略。

如果要让 NPS 充当这两个的 RADIUS 服务器，处理本地连接请求和为 RADIUS 代理，将某些连接请求转发到远程 RADIUS 服务器组，添加新策略使用以下过程，然后验证默认值连接请求策略是通过将其放置在策略列表中的最后一次处理的最后一个策略。

## <a name="add-a-connection-request-policy"></a>添加连接请求策略

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

### <a name="to-add-a-new-connection-request-policy"></a>若要添加新的连接请求策略 

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**打开 NPS 控制台。 
2. 在控制台树中，双击**策略**。
3. 右键单击**连接请求策略**，然后单击**新的连接请求策略**。
4. 使用新的连接请求策略向导配置连接请求策略，并且如果不是以前配置的远程 RADIUS 服务器组。


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。

