---
title: RDS - 生成和部署
description: 生成远程桌面部署的步骤
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/18/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 176ae424-96e9-4c78-88f5-da418e76c3d7
author: lizap
manager: dongill
ms.openlocfilehash: 9c3962a830d9544e915a96f061e32bb9e037949b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404034"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>生成和部署远程桌面服务部署

远程桌面服务部署是用来与用户共享应用和资源的基础结构。 根据你要提供的体验，可视需要生成小型部署或复杂部署。 可以轻松缩放远程桌面部署。 可按需增加和减少远程桌面 Web 访问、网关、连接代理和会话主机服务器的数目。 可以使用远程桌面连接代理来分配工作负荷。 基于 Active Directory 的身份验证提供高度安全的环境。 

使用[远程桌面客户端](clients/remote-desktop-clients.md)可从任何 Windows、Apple 或 Android 计算机、平板电脑或手机进行访问。

有关相互协作以构成远程桌面服务部署的各个组成部分的详细介绍，请参阅[远程桌面服务体系结构](desktop-hosting-logical-architecture.md)。

现有的远程桌面部署是否是在旧版 Windows Server 上生成的？ 请查看以下用于过渡到 WIndows Server 2016 的选项，以利用对性能与规模有利的新功能和改进的功能：

- [将 RDS 部署迁移到 Windows Server 2016](migrate-rds-role-services.md)
- [将 RDS 部署升级到 Windows Server 2016](upgrade-to-rds-2016.md)

想要创建新的远程桌面部署？ 请使用以下信息在 Windows Server 2016 中部署远程桌面：

- [部署远程桌面服务基础结构](rds-deploy-infrastructure.md)
- [创建一个会话集合用于保存你要共享的应用和资源](rds-create-collection.md)
- [为 RDS 部署授权](rds-client-access-license.md)
- 让用户安装[远程桌面客户端](clients/remote-desktop-clients.md)，以便可以访问应用和资源。 
- 添加其他连接代理和会话主机来实现高可用性：
   - [通过 RD 会话主机场横向扩展现有 RDS 集合](rds-scale-rdsh-farm.md)
   - [为 RD 连接代理基础结构提供高可用性](rds-connection-broker-cluster.md)
   - [向 RD Web 和 RD 网关 Web 前端添加高可用性](rds-rdweb-gateway-ha.md)
   - [为 UPD 存储部署两个节点的存储空间直通文件系统](rds-storage-spaces-direct-deployment.md)


如果你是一家托管合作伙伴并想要使用远程桌面向客户提供应用和资源，或者，如果你正在寻找某家提供商来托管你的应用，请查看[远程桌面服务托管合作伙伴](rds-hosting-partners.md)，了解在使用 Azure 中的 RDS 作为托管环境时可以做出的评估，以及已通过该项评估的合作伙伴列表。
