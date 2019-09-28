---
title: 迁移到 Windows Server 2016 中的 MultiPoint 服务
description: 了解如何从 MultiPoint Services 的以前版本进行迁移
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 297de9ee2450856e24b9196a8bfb312991657e6d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389038"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Windows Server 2016 中的 MultiPoint Services 迁移
>适用于：Windows Server 2016

可以从以前版本的 Windows Server 2016 MultiPoint 服务迁移到 MultiPoint Services 的 RTM 版本。 以下信息提供了准备信息、迁移和验证步骤。

迁移文档和工具简化了将服务器角色设置和数据从现有服务器迁移到运行 Windows Server 2016 的目标服务器的任务。 通过使用本指南中介绍的过程，你可以简化迁移过程、减少迁移时间、提高迁移过程的准确性，并帮助消除在迁移过程中可能出现的冲突。 

## <a name="what-to-know-before-you-begin"></a>开始之前需要了解的内容
在开始迁移过程之前，请注意以下事项：

- 迁移过程不会自动收集或记录 MultiPoint 服务角色上的应用程序的设置。 应为要迁移的任何应用程序创建自定义迁移计划。 在 MultiPoint Services 中使用虚拟桌面功能时也是如此。
- 本指南不提供有关移动在 MultiPoint server 上的用户或共享文件夹中保存的数据的指南。 这适用于常规工作站和虚拟桌面工作站。
- 本指南不包含有关如何在源服务器运行多个角色时进行迁移的说明。 如果你的服务器运行多个角色，则需要根据角色迁移指南中提供的信息设计特定于你的服务器环境的自定义迁移过程。
- 本指南不包含用于迁移远程桌面服务 CAL 的信息。 有关此信息，请参阅[迁移远程桌面服务客户端访问许可证（RDS cal）](https://technet.microsoft.com/library/dd851844.aspx)。

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Windows Server 2016 中的 MultiPoint Services 支持的迁移方案
Windows Server 2016 Standard 和 Datacenter 中提供了 MultiPoint 服务角色服务。 本迁移指南介绍如何将 Multipoint 服务角色服务从运行 Windows Server 2016 的源服务器迁移到运行相同版本的目标服务器。

## <a name="scenarios-that-are-not-supported"></a>不受支持的方案

不支持以下迁移方案：

- 从 Windows MultiPoint Server 2012 和2011进行迁移或升级。
- 从源服务器迁移到安装了不同系统 UI 语言的操作系统上运行的目标服务器。
- 将 MultiPoint 服务角色服务从物理服务器迁移到虚拟机。
- 从 MultiPoint 服务器迁移任何应用程序或应用程序设置。

## <a name="the-impact-of-migration-on-multipoint-services"></a>迁移对 MultiPoint 服务的影响
请注意，在迁移期间不会提供 MultiPoint 服务角色。 若要使停机时间最短并降低对用户的影响，请计划在非高峰时段进行数据迁移。 通知用户在这段时间内无法使用资源。

## <a name="migration-information-and-steps"></a>迁移信息和步骤
使用以下信息来计划和执行 MultiPoint 服务迁移：

- [收集迁移所需的信息。](multipoint-services-migration-preparation.md)
- [迁移 MultiPoint 服务角色服务。](multipoint-services-migration-steps.md)
- [验证迁移并执行任何迁移后清理任务](multipoint-services-post-migration-steps.md)