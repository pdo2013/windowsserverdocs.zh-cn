---
title: 添加或删除计算机
description: 了解如何在 MultiPoint Services 中添加和删除计算机。
ms.custom: na
ms.date: 07/13/2017
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1c37739d-7ab0-4b80-8d05-0330e79fd631
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 5f46e5795b2669bacef4a941506a27fe5737b87b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395620"
---
# <a name="add-or-remove-computers"></a>添加或删除计算机
你可以使用 MultiPoint 管理器添加其他计算机或从你的 MultiPoint 服务系统中删除计算机。 将其他电脑添加到 MultiPoint 管理器表示，当登录该电脑时，允许 MultiPoint 仪表板为该电脑安排任何用户会话，安排方式与为 MultiPoint 工作站安排会话相同。  
  
## <a name="to-add-or-remove-servers-in-multipoint"></a>添加或删除 MultiPoint 中的服务器  
  
1.  在工作站模式下打开 "MultiPoint 管理器"，然后单击 "**主页**" 选项卡。  
  
2.  下*计算机名* **任务**，单击**添加或删除 MultiPoint server**。 "**添加或删除 MultiPoint 服务器**" 屏幕将启动并开始查找本地网络子网上的其他可通过 MultiPoint 管理器管理的服务器。  
  
3.  执行下列操作之一：  
  
    -   **添加服务器：** 在 "**可用**" 列表中，单击要使用 MultiPoint 管理器管理的服务器，然后单击 "**添加**"。 如果服务器的管理员用户帐户和密码与你当前登录所用的帐户不同，系统会提示你提供帐户信息。  
  
    -   **添加不在子网中的服务器：** 在 " **MultiPoint server 名称**" 字段中，键入要添加的服务器的名称，然后单击 "**手动添加**"。  
  
    -   **删除服务器：** 在 "**托管**" 列表中，单击要从管理中删除的服务器，然后单击 "**删除**"。  
  
## <a name="to-add-or-remove-other-computers"></a>添加或删除其他计算机  
  
1.  在工作站模式下打开 "MultiPoint 管理器"，然后单击 "**主页**" 选项卡。  
  
2.  在“主页任务”下，单击“添加或删除个人计算机”。 "**添加或删除个人计算机**" 屏幕将启动并开始查找本地网络子网上的其他可通过 MultiPoint 服务进行管理的计算机。  
  
3.  执行下列操作之一：  
  
    -   **添加计算机：** 在 "**可用**" 列表中，单击要使用 MultiPoint 服务管理的计算机，然后单击 "**添加**"。 添加计算机时，系统会提示你提供帐户信息。  
  
    -   **添加不在子网中的计算机：** 在 "**个人计算机名称**" 字段中，键入要添加的计算机的名称，然后单击 "**手动添加**"。  
  
    -   **删除计算机：** 在 "**托管**" 列表中，单击要从管理中删除的计算机，然后单击 "**删除**"。  
  
## <a name="see-also"></a>请参阅  
[使用 MultiPoint 管理器管理系统任务](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
[编辑服务器设置](Edit-Server-Settings.md)