---
title: 配置连接请求策略
description: 本主题提供有关如何在 Windows Server 2016 中的网络策略 Server 配置连接请求策略的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9677e147bdaea4de71a054cd6c52d81126e005d1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-connection-request-policies"></a>配置连接请求策略

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以创建和配置指定是否本地 NPS 服务器处理连接请求，或将它们转发给远程 RADIUS 服务器进行处理的连接要求策略。

连接请求策略是组条件和允许网络管理员指定执行哪些远程身份验证拨入用户服务 (RADIUS) 服务器的身份验证和的连接请求运行网络策略服务器 \(NPS\) 服务器接收 RADIUS 客户端从授权的设置。

默认连接请求策略使用 NPS RADIUS 服务器，并处理本地所有身份验证请求。

若要配置服务器运行 NPS RADIUS 代理和前进连接到其他 NPS 或 RADIUS 服务器的请求用作，必须配置远程 RADIUS 服务器组除了添加新的连接要求策略指定条件和连接请求必须匹配的设置。

在使用新的连接要求策略向导创建新的连接要求策略，则可以创建一个新远程 RADIUS 服务器组。

如果你不希望 NPS RADIUS 服务器和进程连接本地请求充当服务器，你可以删除默认连接请求策略。

如果你想要用作 RADIUS 服务器 NPS 服务器，处理连接请求本地，并为 RADIUS 代理，转移到远程 RADIUS 服务器组中，某些连接请求添加新的策略使用下面的过程，然后验证将其放策略的列表中的最后一次由处理的最后一个策略默认连接请求策略。

## <a name="add-a-connection-request-policy"></a>添加连接请求策略

在会员**域管理员**，或等效的最低要求完成此过程。

### <a name="to-add-a-new-connection-request-policy"></a>若要添加新的连接要求策略 

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**打开 NPS 主机。 
2. 控制台树中，双击**策略**。
3. 右键单击**连接请求策略**，然后单击**新连接请求策略**。
4. 使用新的连接申请策略向导将你连接配置请求策略，并且如果不之前配置，远程 RADIUS 服务器组。


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。

