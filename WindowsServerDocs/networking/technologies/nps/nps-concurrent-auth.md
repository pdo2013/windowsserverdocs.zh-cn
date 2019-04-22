---
title: 增加由 NPS 处理的并发身份验证
description: 本主题说明了在 Windows Server 2016 中配置网络策略服务器的并发身份验证。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd930e34a4adf6c55812385b691df3e3575a4280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818258"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>增加由 NPS 处理的并发身份验证

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于配置网络策略服务器的并发身份验证的说明。

如果已安装网络策略服务器\(NPS\)域之外的计算机上控制器和 NPS 正在接收大量的每秒的身份验证请求，您可以通过增加的数量提高 NPS 性能NPS 和域控制器之间允许的并发身份验证。

若要执行此操作，必须编辑以下注册表项： 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

添加一个名为的新值**MaxConcurrentApi**并向其分配一个值，从 2 到 5。 

>[!CAUTION]
>如果你将值赋给**MaxConcurrentApi**太高，在 NPS 可能在域控制器上放置过量负载。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。