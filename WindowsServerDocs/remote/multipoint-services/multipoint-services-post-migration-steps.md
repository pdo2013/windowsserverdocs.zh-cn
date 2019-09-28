---
title: MultiPoint 服务-迁移后任务
description: 了解如何验证和关闭到 MultiPoint 服务的迁移
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: 3102a442b4668856050f603f30f57f6bbed20654
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389016"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint 服务-迁移后任务

>适用于：Windows Server 2016

迁移到 Windows Server 2016 中的 MultiPoint 服务后，请使用以下信息来验证迁移并执行清理步骤。

## <a name="validate-the-migration-by-running-a-pilot-program"></a>通过运行试验程序来验证迁移

可以通过在生产环境中创建试点项目来验证 MultiPoint 服务迁移。 将迁移的角色服务投入生产之前，在服务器上运行试点项目，以验证你的部署是否按预期工作。 请考虑先限制连接的数量，从而慢慢增加访问 MultiPoint 服务的用户的数量。

> [!NOTE] 
> 始终使用测试帐户来测试迁移。 使用具有管理权限的帐户和有效用户的帐户。

## <a name="retire-the-source-server"></a>停用源服务器
验证迁移之后，可以关闭源服务器或将其从网络断开连接。 如果服务器已加入域，则将其从域中删除，然后再断开连接。

