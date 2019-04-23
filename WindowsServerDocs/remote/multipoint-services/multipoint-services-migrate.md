---
title: 迁移到 Windows Server 2016 中的 MultiPoint Services
description: 了解如何从以前版本的 MultiPoint 服务迁移
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 24c35c31bf920c41bafa16901ee30a023565dad8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861198"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Windows Server 2016 中的 multiPoint 服务迁移
>适用于：Windows Server 2016

可以从以前版本的 Windows Server 2016 MultiPoint Services 迁移到 MultiPoint 服务的 RTM 版本。 以下信息提供准备的信息和迁移和验证步骤。

迁移文档和工具简化了迁移服务器角色设置和数据从现有服务器到运行 Windows Server 2016 的目标服务器。 通过使用本指南中介绍的过程，你可以简化迁移过程、减少迁移时间、提高迁移过程的准确性，并帮助消除在迁移过程中可能出现的冲突。 

## <a name="what-to-know-before-you-begin"></a>在开始之前须知
在开始迁移过程之前，请注意以下：

- 迁移过程不会不会自动收集或记录的 MultiPoint 服务角色上的应用程序的设置。 应创建任何你想要迁移的应用程序的自定义的迁移计划。 此外，使用 MultiPoint Services 中的虚拟桌面功能时对这也是，则返回 true。
- 本指南不提供用于将数据移指南保存在用户或多点服务器上的共享文件夹。 这适用于正则工作站和虚拟桌面工作站。
- 本指南不包含有关如何进行迁移时源服务器正在运行多个角色的说明。 如果你的服务器正在运行多个角色，您需要设计特定于服务器环境中，基于角色的迁移指南中提供的信息的自定义迁移过程。
- 本指南不包含迁移远程桌面服务 CAL 的信息。 此信息，请参阅[迁移远程桌面服务客户端访问许可证 (RDS Cal)](https://technet.microsoft.com/library/dd851844.aspx)。

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Windows Server 2016 中的 MultiPoint 服务的支持的迁移方案
MultiPoint 服务角色服务现已推出 Windows Server 2016 Standard 和 Datacenter。 本迁移指南介绍如何将 Multipoint 服务角色服务迁移到运行相同版本的目标服务器运行 Windows Server 2016 的源服务器从。

## <a name="scenarios-that-are-not-supported"></a>不支持的方案

不支持以下迁移方案：

- 从迁移或升级 Windows MultiPoint Server 2012 和 2011 年。
- 从源服务器迁移到使用不同的系统 UI 语言的操作系统运行的目标服务器。
- 将 MultiPoint 服务角色服务从物理服务器迁移到虚拟机。
- 从 MultiPoint Server 进行迁移的任何应用程序或应用程序设置。

## <a name="the-impact-of-migration-on-multipoint-services"></a>迁移对 MultiPoint 服务的影响
请注意，MultiPoint 服务角色将不能在迁移过程。 若要使停机时间最短并降低对用户的影响，请计划在非高峰时段进行数据迁移。 通知用户在这段时间内无法使用资源。

## <a name="migration-information-and-steps"></a>迁移信息和步骤
使用以下信息来规划和实施你的 MultiPoint 服务迁移：

- [收集所需的迁移的信息。](multipoint-services-migration-preparation.md)
- [迁移 MultiPoint 服务角色服务。](multipoint-services-migration-steps.md)
- [验证迁移并执行任何迁移后清理任务](multipoint-services-post-migration-steps.md)