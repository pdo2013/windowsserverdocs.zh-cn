---
title: 远程桌面服务 - 任意位置生成
description: 规划信息，帮助确定在何处托管 RDS 部署。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 16f85531f8ac58ed80d4d3f666692b307977c68c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403946"
---
# <a name="remote-desktop-services---build-anywhere"></a>远程桌面服务 - 任意位置生成

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

在本地部署、云中部署或二者混合部署。 随着业务需求变化而修改部署。

无论身处何地，远程桌面服务环境的基础[体系结构](desktop-hosting-logical-architecture.md)都保持不变：
- 仍然必须具有面向 Internet 的服务器，以将 RD Web 访问和 RD 网关用于外部用户
- 仍然必须具有 Active Directory 以及（对于高度可用的环境）SQL 数据库来容纳用户和远程桌面属性
- 仍然必须在 RD 基础结构角色（RD 连接代理、RD 网关、RD 授权和 RD Web 访问）与终端 RDSH 或 RDVH 主机之间具有通信访问，才能将最终用户连接到其桌面或应用程序。

这种灵活性使你可以充分利用以下两个优势：
- 与云和在线领域关联的简单性和即用即付方法。
- 利用本地已存在的大量资源的熟悉且轻松的方式。

有关其他信息，请了解如何[生成和部署远程桌面服务部署](rds-build-and-deploy.md)。