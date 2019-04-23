---
title: MultiPoint 服务的迁移后任务
description: 了解如何验证和关闭出迁移到 MultiPoint 服务
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: e3fa3c812355a14289ea4eeff3ab1e7e92e00d97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863498"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint 服务的迁移后任务

>适用于：Windows Server 2016

迁移到 Windows Server 2016 中的 MultiPoint 服务后，使用以下信息来验证迁移并执行清理步骤。

## <a name="validate-the-migration-by-running-a-pilot-program"></a>通过运行试验程序来验证迁移

您可以通过在生产环境中创建一个试验项目验证 MultiPoint 服务迁移。 之前已迁移的角色服务投入生产以验证你的部署按预期正常工作的服务器上运行试验项目。 请考虑限制一开始，慢慢增加用户访问 MultiPoint 服务的连接数。

> [!NOTE] 
> 始终使用测试帐户来测试迁移。 使用具有管理权限和帐户为有效用户的帐户。

## <a name="retire-the-source-server"></a>停用源服务器
验证迁移之后，可以关闭或断开源服务器与你的网络连接。 如果该服务器已加入域的它从域中删除之前断开其连接。

