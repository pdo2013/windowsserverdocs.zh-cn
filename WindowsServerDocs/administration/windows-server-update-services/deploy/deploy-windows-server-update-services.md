---
title: 部署 Windows Server Update Services
description: Windows Server Update Service (WSUS) 主题-其中包含指向四个步骤来完成该部署过程的概述
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51972ad352f6530c8ee2aa84aec57b62784da728
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873178"
---
# <a name="deploy-windows-server-update-services"></a>部署 Windows Server Update Services

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Windows Server Update Service (WSUS) 启用信息技术管理员部署最新的 Microsoft 产品更新。 WSUS 是可安装以管理和分配更新的 Windows Server 服务器角色。 WSUS 服务器可以作为组织内其他 WSUS 服务器的更新源。 充当更新源的 WSUS 服务器称为上游服务器。  

在 WSUS 实现过程中，网络中必须至少有一台 WSUS 服务器连接到 Microsoft 更新以获取可用的更新信息。 您可以确定，根据网络安全和配置，其他服务器如何直接连接到 Microsoft Update。  

本指南提供有关规划和部署 Windows Server 更新服务的概念性信息。  

-   [计划你的 WSUS 部署](../plan/plan-your-wsus-deployment.md)  

-   [步骤 1：安装 WSUS 服务器角色](1-install-the-wsus-server-role.md)  

-   [步骤 2：将 WSUS 配置](2-configure-wsus.md)  

-   [步骤 3：批准和部署 WSUS 中的更新](3-approve-and-deploy-updates-in-wsus.md)  

-   [步骤 4：配置为自动更新的组策略设置](4-configure-group-policy-settings-for-automatic-updates.md)  
