---
title: 管理客户端访问许可证
description: 了解如何使用 MultiPoint Services 中的 Cal
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: f5f78d3d2387d3b95177a6a8a40fb9b16d8ed8e2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446120"
---
# <a name="manage-client-access-licenses"></a>管理客户端访问许可证
连接到 MultiPoint 服务系统，包括运行用作工作站的 MultiPoint 服务的计算机的每个工作站必须具有有效的每个用户的远程桌面*客户端访问许可证 (CAL)* 。

如果使用的虚拟工作站桌面而不物理的工作站，则必须安装每个工作站虚拟桌面的 CAL。  
  
1.  购买每个工作站连接到 MultiPoint 服务计算机或服务器的客户端许可证。 有关购买 Cal 的详细信息，请访问远程桌面授权的文档。 <!--@Liza: add link to RDS licensing here-->

2.  从**启动**屏幕上，打开**MultiPoint 管理器**。  
  
3.  单击**主页**选项卡，然后依次**添加客户端访问许可证**。  这将打开 CAL 许可的管理工具。

# <a name="set-the-licensing-mode-manually"></a>手动设置的授权模式
如果未正确配置 MultiPoint 服务设置将提示提供有关宽限期即将到期的通知。 请执行以下步骤来设置的授权模式：

1. 启动**本地组策略编辑器**(gpedit.msc)。

2. 在左窗格中，导航到**本地计算机策略-> 计算机配置-> 管理模板-> Windows 组件-> 远程桌面服务-> 远程桌面会话主机-> 许可**。

3. 在右窗格中，右键单击**使用指定的远程桌面许可证服务器**，然后选择**编辑**:
   - 在组策略编辑器对话框中，选择**已启用**
   - 输入中的本地计算机名称**许可证服务器使用**字段。
   - 选择**确定**
  
4. 在右窗格中，右键单击**设置远程桌面授权模式**，然后选择**编辑**
   - 在组策略编辑器对话框中，选择**已启用**
   - 设置**许可模式**到每个设备 / 每个用户
   - 选择**确定** 

  
## <a name="see-also"></a>请参阅  
[使用 MultiPoint 管理器管理系统任务](Manage-System-Tasks-Using-MultiPoint-Manager.md)
