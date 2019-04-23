---
title: 禁用 NAS 通知转发，在 NPS 中
description: 本主题说明了在 Windows Server 2016 中配置网络策略服务器的并发身份验证。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bc4c6afdcb02eb2bbab1f0373a5b3a28236269bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882258"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>禁用 NAS 通知转发，在 NPS 中

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于禁用转发开始和停止的消息从网络访问服务器 (Nas) 为 NPS 中配置的远程 RADIUS 服务器组的成员。

如果必须配置远程 RADIUS 服务器组，并在 NPS**连接请求策略**，清除**记帐请求转发到此远程 RADIUS 服务器组**复选框，这些组是仍然发送的 NAS 启动和停止通知消息。 

这将创建不必要的网络流量。 若要消除此流量，禁用 NAS 通知转发的每个远程 RADIUS 服务器组中的单个服务器。

若要完成此过程，你必须是**Administrators**组的成员。

### <a name="to-disable-nas-notification-forwarding"></a>若要禁用 NAS 通知转发

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开 NPS 控制台。

2. 在 NPS 控制台中，双击**RADIUS 客户端和服务器**，单击**远程 RADIUS 服务器组**，然后双击你想要配置远程 RADIUS 服务器组。 远程 RADIUS 服务器组**属性**对话框随即打开。

3. 双击你想要配置，然后单击的组成员**身份验证/记帐**选项卡。

4. 在中**记帐**，清除**转发网络访问服务器启动和停止通知发送到此服务器**复选框，然后依次**确定**。

5. 你想要配置的所有组成员都重复步骤 3 和 4。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。
