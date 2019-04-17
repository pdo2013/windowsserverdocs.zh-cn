---
title: 增加同时处理 nps 的身份验证
description: 本主题提供并行网络策略服务器的身份验证配置 Windows Server 2016 的说明进行操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa70c1a26e2c22d26545e1b46a6151d71a2b4095
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>增加同时处理 nps 的身份验证

>适用于：Windows Server（半年通道），Windows Server 2016

有关说明配置并行网络策略服务器的身份验证，你可以使用本主题。

如果域控制器之外的计算机上安装网络策略服务器 \(NPS\) NPS 服务器接收大量每秒进行身份验证请求，你可以通过增加并行身份验证之间 NPS 服务器和域控制器允许的数量来提高 NPS 性能。

若要执行此操作，你必须编辑以下注册表项： 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

添加了一个名为新值**MaxConcurrentApi**和为其指定值从至 5 2。 

>[!CAUTION]
>如果你分配到值**MaxConcurrentApi**过高，NPS 服务器可能的负担过多域控制器。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。