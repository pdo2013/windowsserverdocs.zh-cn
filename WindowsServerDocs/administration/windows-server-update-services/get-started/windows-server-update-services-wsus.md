---
title: 开始使用 Windows Server Update Services (WSUS)
description: Windows Server Update Service (WSUS) 主题的服务器角色和实际应用程序的概述
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 7a6c64e0a4321553162b426e3d6857ff6ac3581c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830258"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Windows Server Update Service (WSUS) 启用信息技术管理员部署最新的 Microsoft 产品更新。 可以使用 WSUS 来完全管理通过 Microsoft Update 发布到你的网络上的计算机的更新的分发。 本主题概述了此服务器角色并提供了有关如何部署和维护 WSUS 的详细信息。

## <a name="wsus-server-role-description"></a>WSUS 服务器角色描述
WSUS 服务器提供了可用于管理和分配更新通过管理控制台的功能。 WSUS 服务器也可以是组织内其他 WSUS 服务器的更新源。 充当更新源的 WSUS 服务器称为上游服务器。 在 WSUS 实现中，在网络上的至少一台 WSUS 服务器必须能够连接到 Microsoft 更新以获取可用的更新信息。 作为管理员，您可以确定的基于网络安全和配置-多少台 WSUS 服务器直接连接到 Microsoft 更新。

### <a name="practical-applications"></a>实际应用程序
更新管理是控制在生产环境中部署和维护中期软件版本的过程。 它帮助你维护操作效率，解决安全漏洞问题以及维持你的生产环境的稳定性。 如果你的组织无法确定和维持其操作系统和应用软件中已知的信任级别，则可能存在许多安全漏洞，一旦被利用，则会引发收入和知识产权损失。 最大限度降低这种威胁需要你拥有正确配置的系统、使用最新软件和安装推荐的软件更新。

WSUS 为你的企业增加价值的核心方案：

-   集中式更新管理

-   更新管理自动化

### <a name="new-and-changed-functionality"></a>新增功能和更改的功能

> [!NOTE]
> 从支持 WSUS 3.2 到 Windows Server 2012 R2 的 Windows Server 任何版本的升级需要首先卸载 WSUS 3.2。
> 
> 在 Windows Server 2012 中，从装有 WSUS 3.2 的 Windows Server 任何版本升级则将无法在安装过程中检测到 WSUS 3.2。 在这种情况下，系统会提示首先卸载 Windows Server Update Services，然后再升级你的服务器。
> 
> 但是，由于在此版本的 Windows Server 和 Windows Server 2012 R2，从任何版本的 Windows Server 和 WSUS 3.2 升级时的更改，安装是不会被阻止。 未能执行 Windows Server 2012 R2 升级之前不卸载 WSUS 3.2 将会导致帖子的 WSUS 安装任务失败的 Windows Server 2012 R2 中。 在这种情况下，唯一已知的纠正措施就是格式化硬盘驱动器并重新安装 Windows Server。

Windows Server Update Service (WSUS) 是包含以下增强功能的内置服务器角色。

-   可使用服务器管理器进行添加和删除

-   包括 Windows PowerShell cmdlet 管理 WSUS 中最重要的管理任务

-   添加 SHA256 哈希功能，提高安全性

-   提供客户端和服务器之间的分离： Windows 更新代理 (WUA) 的版本可以提供与 WSUS 独立开来

### <a name="using-windows-powershell-to-manage-wsus"></a>使用 Windows PowerShell 管理 WSUS
如果系统管理员想自动化其操作，则通过命令行自动化确定操作范围。 主要目标是通过允许系统管理员自动化其日常操作，促进 WSUS 管理。

**这一更改增添了什么价值？**

通过 Windows PowerShell 陈列核心 WSUS 操作，系统管理员可以提高工作效率，缩短新工具的学习时间，并减少由于相似操作之间缺乏一致性而未能达到预期效果所产生的错误。

**工作原理的不同之处是什么？**

在早期版本的 Windows Server 操作系统中，不存在 Windows PowerShell cmdlet，更新管理自动化充满挑战。 适用于 WSUS 操作的 Windows PowerShell cmdlet 为系统管理员增加灵活性和敏捷性。

## <a name="in-this-collection"></a>在此集合中
有关规划的以下指南、 部署和管理 WSUS 位于此集合中：

-   [部署 Windows Server 更新服务](../deploy/deploy-windows-server-update-services.md)

-   [使用 Windows Server Update Services 管理更新](../manage/update-management-with-windows-server-update-services.md)


