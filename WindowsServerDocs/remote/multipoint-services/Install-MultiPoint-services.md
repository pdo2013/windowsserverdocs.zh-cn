---
title: 安装 MultiPoint 服务
description: 了解如何安装和配置 Windows Server 2016 中的 MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 52a824bbca3e9f2e1c7823601f6208ae19ae50ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866218"
---
# <a name="install-multipoint-services"></a>安装 MultiPoint 服务
如果要从零开始安装服务器按照这些说明安装 MultiPoint 服务。  

已成功安装 Windows Server 2016 后请以管理员身份登录。 使用服务器管理器可以在其中启用 MultiPoint 服务。 服务器管理器在启动时将自动打开。 在仪表板选择**添加角色和功能**若要启用 MultiPoint 服务，并按照向导中的说明。

在安装类型部分中可能会使用 
- 基于角色或基于功能的安装或
- 远程桌面服务安装

对于标准 MultiPoint Services 部署，我们建议选择允许方便地选择部署类型在 MultiPoint 服务角色的远程桌面服务安装。 对于基于角色的安装将需要选择**MultiPoint 服务**角色列表中。 安装成功后，服务器将重新启动。  
  
## <a name="configure-your-primary-station"></a>配置主工作站  
  
1.  上**创建 MultiPoint Server 工作站**页上，键入该监视器从键盘所指定的信函。 键盘和鼠标以该工作站，将相关联的正确密钥条目。  
2.  以管理员身份登录。  