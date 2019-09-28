---
title: 禁用 NPS 中的 NAS 通知转发
description: 本主题提供有关在 Windows Server 2016 中配置网络策略服务器并发身份验证的说明。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b8ae0ab02a5c14675d543087f635d53ee63e0423
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396250"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>禁用 NPS 中的 NAS 通知转发

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此过程禁用从网络访问服务器（Nas）到在 NPS 中配置的远程 RADIUS 服务器组成员的启动和停止消息转发。

如果已配置远程 RADIUS 服务器组，并在 NPS**连接请求策略**中清除了 "将**请求转发到此远程 RADIUS 服务器组**" 复选框，则这些组仍将发送 NAS 启动和停止通知讯息. 

这会产生不必要的网络流量。 若要消除此流量，请针对每个远程 RADIUS 服务器组中的各个服务器禁用 NAS 通知转发。

若要完成此过程，你必须是**Administrators**组的成员。

### <a name="to-disable-nas-notification-forwarding"></a>禁用 NAS 通知转发

1. 在服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 此时将打开 NPS 控制台。

2. 在 NPS 控制台中，双击 " **RADIUS 客户端和服务器**"，单击 "**远程 radius 服务器组**"，然后双击要配置的远程 radius 服务器组。 "远程 RADIUS 服务器组**属性**" 对话框将打开。

3. 双击要配置的组成员，然后单击 "**身份验证/记帐**" 选项卡。

4. 在 "**记帐**" 中，清除 "将**网络访问服务器启动和停止通知发送到此服务器**" 复选框，然后单击 **"确定"** 。

5. 对于要配置的所有组成员，重复步骤3和4。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。
