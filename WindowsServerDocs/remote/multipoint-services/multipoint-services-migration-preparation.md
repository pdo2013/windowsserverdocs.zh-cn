---
title: 准备迁移至 MultiPoint 服务
description: 描述信息以收集之前迁移到 Windows Server 2016 中的 MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 9650ebcae7e6207a226617d401d892049405901f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824378"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>准备迁移到 Windows Server 2016 中的 MultiPoint Services

>适用于：Windows Server 2016

使用以下信息来收集迁移到运行 Windows Server 2016 RTM 的目标服务器运行 Windows Server 2016 的早期版本的源服务器从 MultiPoint 服务角色服务所需的信息。

至少，您必须是源服务器和目标服务器上的 Administrators 组的成员才能安装、 删除或设置 MultiPoint 服务。

>[!NOTE]
> 此处的步骤将数据迁移保存到用户文件夹或共享文件夹不提供指导。 请确保在开始迁移之前，将用户备份其数据。

使用 MultiPoint 管理器检索迁移所需的信息。 你将需要使用 MultiPoint 管理器的服务器管理员权限。

记录中的多点服务器、 用户和环境设置[迁移数据收集工作表](multipoint-services-migration-worksheet.md)。 使用以下步骤来获取该信息。

## <a name="multipoint-server-settings-for-the-local-server"></a>本地服务器的多点服务器设置
1. 启动 MultiPoint 管理器。
2. 上**主页**选项卡上，选择本地服务器，然后单击**编辑服务器设置。**
3. 数据工作表中记录的设置。
4. 关闭设置窗口。

## <a name="managed-servers-and-computers"></a>托管的服务器和计算机

有关托管的服务器和计算机的名称**主页**MultiPoint 管理器中的选项卡。

## <a name="station-settings"></a>工作站设置
如果为工作站配置了自动登录或显示方向，使用以下步骤来检索该信息。 否则可以跳过此步骤。

若要检索的工作站设置：

1. 转到**工作站**MultiPoint 管理器中的选项卡。
2. 在中查找具有"是"工作站**自动登录**列。
3. 选择该工作站，然后单击**配置工作站**。
4. 记录用于自动登录的用户。

若要检索的显示方向设置，请查看**工作站设置**为每个工作站。

## <a name="list-of-users"></a>用户的列表
1. 单击**用户**MultiPoint 管理器中的选项卡。
2. 记录**管理员**并**MultiPoint 仪表板用户**accoutns。
3. 记录标准用户。

## <a name="vdi-template-location"></a>VDI 模板位置
 如果你先前启用了 VDI 模板功能，记录 VDI 模板的位置。 只要源和目标服务器位于同一网络上，可以通过使用 MultiPoint 管理器模板导入。
 
## <a name="next-step"></a>下一步
你现在已准备好[迁移到 MultiPoint Services](multipoint-services-migration-steps.md)在 RTM 版本中的 Windows Server 2016。