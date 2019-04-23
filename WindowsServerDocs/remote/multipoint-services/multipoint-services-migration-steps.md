---
title: MultiPoint 服务迁移的步骤
description: 将指导你完成将迁移到 Windows Server 2016 中的 MultiPoint 服务的步骤
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: b63ef4bf63ce990aa0b0ba7624905ba8f14dde98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854808"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>迁移到 Windows Server 2016 中的 MultiPoint Services

>适用于：Windows Server 2016

使用以下步骤加上迁移规划工作表将迁移到 Windows Server 2016 中的 MultiPoint 服务中收集的信息。

## <a name="transfer-server-settings"></a>传输服务器设置
在目标服务器上，打开 MultiPoint 管理器。 单击**编辑服务器设置**。 应用根据迁移计划工作表中的设置。

> [!NOTE]
> 如果你需要启用在目标服务器上的磁盘保护，请一直等到配置 MultiPoint 服务。

## <a name="transfer-station-settings"></a>将工作站设置传输
确保工作站已连接到目标服务器和所有映射之前将工作站设置应用。 将自动检测到工作站。 若要定义的用户工作站和连接的 USB 设备的服务器映射每个工作站屏幕上按照的说明。 迁移规划工作表中所述，将应用你首选的工作站的设置。

## <a name="migrate-the-vdi-template"></a>迁移 VDI 模板

导入源服务器的 VDI 模板之前，请使用 MultiPoint 管理器启用在目标服务器上的虚拟桌面：

1. 转到**虚拟桌面**MultiPoint 管理器中的选项卡。
2. 单击**启用虚拟桌面**。 服务器将安装 HYPER-V 角色，然后重新启动。
3. 打开 MultiPoint 管理器并导航回到**虚拟桌面**。
4. 单击**导入虚拟桌面模板**。 按照说明从源服务器中导入模板。

> [!NOTE]
> 导入虚拟桌面模板时，将重置应用于模板进行任何自定义。 

## <a name="next-step"></a>下一步
[验证新的 MultiPoint Services 部署。](multipoint-services-post-migration-steps.md)