---
title: 准备迁移至 MultiPoint 服务
description: 描述在迁移到 Windows Server 2016 中的 MultiPoint 服务之前要收集的信息
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: d0f1fd22b00bdb2e5e3684a541dd14532fd885e6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394653"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>准备迁移到 Windows Server 2016 中的 MultiPoint 服务

>适用于：Windows Server 2016

使用以下信息来收集将 MultiPoint 服务角色服务从运行早期版本的 Windows Server 2016 的源服务器迁移到运行 Windows Server 2016 RTM 的目标服务器所需的信息。

至少必须是源服务器和目标服务器上 "管理员" 组的成员，才能安装、删除或设置 MultiPoint 服务。

>[!NOTE]
> 此处的步骤不提供迁移保存到用户文件夹或共享文件夹的数据的指导。 在开始迁移之前，请确保用户备份其数据。

使用 MultiPoint 管理器检索迁移所需的信息。 你将需要服务器管理员权限才能使用 MultiPoint 管理器。

记录[迁移数据收集工作表](multipoint-services-migration-worksheet.md)中的 MultiPoint 服务器、用户和环境设置。 使用以下步骤收集该信息。

## <a name="multipoint-server-settings-for-the-local-server"></a>本地服务器的 MultiPoint Server 设置
1. 启动 MultiPoint 管理器。
2. 在 "**主页**" 选项卡上，选择本地服务器，然后单击 "**编辑服务器设置"。**
3. 记录数据工作表中的设置。
4. 关闭 "设置" 窗口。

## <a name="managed-servers-and-computers"></a>托管服务器和计算机

可以在 MultiPoint 管理器的 "**主页**" 选项卡上找到托管服务器和计算机的名称。

## <a name="station-settings"></a>工作站设置
如果为工作站配置了自动登录或显示方向，请使用以下步骤检索该信息。 否则，可以跳过此步骤。

检索工作站设置：

1. 在 MultiPoint 管理器中转到 "**工作站**" 选项卡。
2. 在**自动登录**列中查找 "是" 的工作站。
3. 选择该工作站，然后单击 "**配置工作站**"。
4. 记录用于自动登录的用户。

若要检索显示方向设置，请查看每个工作站的**工作站设置**。

## <a name="list-of-users"></a>用户列表
1. 单击 "MultiPoint 管理器" 中的 "**用户**" 选项卡。
2. 记录**管理员**和**MultiPoint 仪表板用户**accoutns。
3. 记录标准用户。

## <a name="vdi-template-location"></a>VDI 模板位置
 如果以前启用了 VDI 模板功能，请记录 VDI 模板的位置。 如果源服务器和目标服务器位于同一网络上，则可以使用 MultiPoint 管理器导入该模板。
 
## <a name="next-step"></a>下一步
你现在已准备好在 Windows Server 2016 的 RTM 版本中[迁移到 MultiPoint 服务](multipoint-services-migration-steps.md)。