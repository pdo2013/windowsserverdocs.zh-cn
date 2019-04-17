---
title: 禁用 NAS 通知转移 NPS 中
description: 本主题提供并行网络策略服务器的身份验证配置 Windows Server 2016 的说明进行操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c25f3b5a94624a35099e84ede3296f7ab860da7c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>禁用 NAS 通知转移 NPS 中

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程禁用转发的开始和停止从网络的访问权限服务器 (Nas) 的消息，配置在 NPS 远程 RADIUS 服务器组中的成员。

当远程配置 RADIUS 服务器组时，在 NPS**连接请求策略**，清除**转发给远程 RADIUS 服务器该组记帐请求**复选框，这些组仍发送 NAS 开始和停止通知消息。 

这将创建不必要的网络流量。 若要消除该通信，禁用转发针对每个远程 RADIUS 服务器组中的个别服务器 NAS 通知。

若要完成此过程，你必须**管理员**组。

### <a name="to-disable-nas-notification-forwarding"></a>若要禁用 NAS 通知转移

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开 NPS 主机。

2. 在 NPS 控制台中，双击**RADIUS 客户端和服务器**，单击**远程 RADIUS 服务器组**，然后双击所需配置远程 RADIUS 服务器组。 远程 RADIUS 服务器组**属性**对话框中打开。

3. 双击要配置，然后单击的组成员**身份验证/记帐**选项卡。

4. 在**记帐**，清除**网络的访问权限 server 启动和停止对此服务器通知转发**复选框，然后依次单击**确定**。

5. 所有你想要配置的组成员的重复步骤 3 和 4。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。
