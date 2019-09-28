---
title: 配置连接请求策略
description: 本主题提供有关如何在 Windows Server 2016 中的网络策略服务器中配置连接请求策略的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d62beb3106141d4683c957020bc96e4a7dfb306f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405476"
---
# <a name="configure-connection-request-policies"></a>配置连接请求策略

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来创建和配置连接请求策略，以指定本地 NPS 是否处理连接请求或将其转发到远程 RADIUS 服务器进行处理。

连接请求策略是一组条件和设置，这些设置允许网络管理员指定哪些远程身份验证拨入用户服务（RADIUS）服务器执行身份验证和授权。运行网络策略服务器的服务器 \(NPS @ no__t-1 从 RADIUS 客户端接收。

默认连接请求策略将 NPS 用作 RADIUS 服务器，并在本地处理所有身份验证请求。

若要将运行 NPS 的服务器配置为充当 RADIUS 代理，并将连接请求转发到其他 NPS 或 RADIUS 服务器，你必须配置远程 RADIUS 服务器组，并添加新的连接请求策略，以指定连接请求必须匹配。

使用新建连接请求策略向导创建新的连接请求策略时，可以创建新的远程 RADIUS 服务器组。

如果你不希望 NPS 作为 RADIUS 服务器，并在本地处理连接请求，则可以删除默认连接请求策略。

如果希望 NPS 同时作为 RADIUS 服务器，并在本地处理连接请求，并将某些连接请求转发到远程 RADIUS 服务器组，请使用以下过程添加新策略，然后验证默认值连接请求策略是通过将最后一个策略放置在策略列表中进行处理的最后一个策略。

## <a name="add-a-connection-request-policy"></a>添加连接请求策略

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

### <a name="to-add-a-new-connection-request-policy"></a>添加新的连接请求策略 

1. 在服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**" 以打开 NPS 控制台。 
2. 在控制台树中，双击 "**策略**"。
3. 右键单击 "**连接请求策略**"，然后单击 "**新建连接请求策略**"。
4. 使用新建连接请求策略向导来配置连接请求策略，如果之前未配置，则配置远程 RADIUS 服务器组。


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。

