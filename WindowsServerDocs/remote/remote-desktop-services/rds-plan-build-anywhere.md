---
title: 远程桌面服务的任意位置生成
description: 若要帮助您确定在何处托管 RDS 部署的规划信息。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cbb8e73d753b1fe4f0293cf4427c634020a23a42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869508"
---
# <a name="remote-desktop-services---build-anywhere"></a>远程桌面服务的任意位置生成

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本地部署，在云中或二者的混合。 根据你的业务需求的变化，请修改你的部署。

无论您身处何地，基础[体系结构](desktop-hosting-logical-architecture.md)远程桌面服务的环境保持不变：
- 您仍然必须具有面向 internet 的服务器为外部用户使用 RD Web 访问和 RD 网关
- 您仍必须有一个 Active Directory 和--为高度可用的环境-SQL 数据库复制到内部用户和远程桌面属性
- 您仍然必须具有远程桌面基础结构角色 （RD 连接代理、 RD 网关、 RD 授权和 RD Web 访问） 和结束 RDSH 之间的通信访问或 RDVH 主机在能够连接到其台式计算机或应用程序的最终用户。

这种灵活性，可获取系统的这两个优势：
- 在云中和联机世界与关联的简单性和即用即付方法。
- 熟悉度及已利用最繁忙的资源的轻松方式存在于本地。

有关其他信息，看看如何[生成和部署远程桌面服务部署](rds-build-and-deploy.md)。