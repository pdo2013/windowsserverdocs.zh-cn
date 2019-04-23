---
title: 远程桌面服务的高可用性
description: 规划高度可用的 RDS 部署设置的信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: b5a2bd38c8831063d6fd2ba525b71a10403b8fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839258"
---
# <a name="remote-desktop-services---high-availability"></a>远程桌面服务的高可用性

>适用于：Windows 服务器 （半年频道），Windows Server 2016

故障和限制是不可避免的大型系统中。 也很容易设置远程桌面基础结构角色，以支持高可用性，并允许最终用户可无缝，连接每次。

在远程桌面服务，以下各项表示具有其各自的指导来建立高可用性的远程桌面基础结构角色：
- [远程桌面连接代理](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [远程桌面网关](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- 远程桌面授权
- [远程桌面 Web 访问](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

通过复制每个角色服务在第二个计算机上建立高可用性。 在 Azure 中，可以通过将组 （承载相同的角色） 的两个虚拟机放置在可用性接收保证正常运行时间设置。

可用性集，以及您现在可以利用 Azure SQL 数据库和其支持 Azure 的 SLA 的强大功能，以确保您始终将连接信息并可以将用户重定向到其台式计算机和应用程序。

有关创建 RDS 环境的最佳实践，请参阅[桌面托管体系结构](desktop-hosting-reference-architecture.md)。