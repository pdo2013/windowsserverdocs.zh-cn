---
title: 迁移 MultiPoint 服务的步骤
description: 指导你完成迁移到 Windows Server 2016 中的 MultiPoint 服务的步骤
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 862e9b70cfafa9de0928a4789c5d23dfa0fbb530
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389029"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>迁移到 Windows Server 2016 中的 MultiPoint 服务

>适用于：Windows Server 2016

使用以下步骤以及在迁移规划工作表中收集的信息，以迁移到 Windows Server 2016 中的 MultiPoint 服务。

## <a name="transfer-server-settings"></a>传输服务器设置
在目标服务器上，打开 "MultiPoint 管理器"。 单击 "**编辑服务器设置**"。 根据迁移规划工作表应用设置。

> [!NOTE]
> 如果需要在目标服务器上启用磁盘保护，请等待，直到配置了 MultiPoint 服务。

## <a name="transfer-station-settings"></a>传输工作站设置
请确保在应用工作站设置之前，工作站连接到目标服务器并全部映射。 将自动检测到工作站。 按照每个工作站屏幕上的说明定义用户工作站和连接的 USB 设备的服务器映射。 如迁移规划工作表中所述，应用首选工作站设置。

## <a name="migrate-the-vdi-template"></a>迁移 VDI 模板

在从源服务器导入 VDI 模板之前，请使用 MultiPoint 管理器启用目标服务器上的虚拟机：

1. 在 MultiPoint 管理器中转到 "**虚拟机**" 选项卡。
2. 单击 "**启用的虚拟桌面**"。 服务器将安装 Hyper-v 角色，然后重新启动。
3. 打开 MultiPoint 管理器并导航回 "**虚拟桌面**"。
4. 单击 "**导入虚拟桌面模板**"。 按照说明从源服务器导入模板。

> [!NOTE]
> 导入虚拟桌面模板时，应用于该模板的任何自定义都将重置。 

## <a name="next-step"></a>下一步
[验证新的 MultiPoint Services 部署。](multipoint-services-post-migration-steps.md)