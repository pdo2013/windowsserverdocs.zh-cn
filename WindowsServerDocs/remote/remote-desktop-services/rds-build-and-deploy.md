---
title: RDS-生成和部署
description: 若要生成的远程桌面部署的步骤
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0ab4be6ba326db05e8fc6a9c14e84c8cbb855926
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814748"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>生成和部署远程桌面服务部署

远程桌面服务部署是用于与用户共享应用和资源的基础结构。 具体取决于您想要提供的体验，您可以使其作为小型或复杂所需。 能够轻松地缩放远程桌面部署。 可以增加和减少远程桌面 Web 访问，将在网关、 连接代理和会话主机服务器。 可以使用远程桌面连接代理分配工作负荷。 Active Directory 基于身份验证提供了高度安全环境。 

[远程桌面客户端](clients\remote-desktop-clients.md)启用访问权限从任何 Windows、 Apple 或 Android 计算机、 平板电脑或电话。

请参阅[远程桌面服务体系结构](desktop-hosting-logical-architecture.md)有关的不同部分，它们共同协作以组成一个远程桌面服务部署的详细讨论。

拥有现有的远程桌面部署基于早期版本的 Windows Server？ 请查看迁移到 WIndows Server 2016，其中您可以充分利用新功能和更好功能在性能和规模的选项：

- [RDS 部署迁移到 Windows Server 2016](migrate-rds-role-services.md)
- [将 RDS 部署升级到 Windows Server 2016](upgrade-to-rds-2016.md)

要创建新的远程桌面部署？ 使用以下信息来部署 Windows Server 2016 中的远程桌面：

- [部署远程桌面服务基础结构](rds-deploy-infrastructure.md)
- [创建会话集合来保存应用和你想要共享的资源](rds-create-collection.md)
- [许可证 RDS 部署](rds-client-access-license.md)
- 让用户安装[远程桌面客户端](clients/remote-desktop-clients.md)以便他们可以访问应用和资源。 
- 通过添加其他连接代理和会话主机启用高可用性：
   - [横向扩展现有的 RDS 集合与 RD 会话主机服务器场](rds-scale-rdsh-farm.md)
   - [RD 连接代理基础结构中添加高可用性](rds-connection-broker-cluster.md)
   - [将可用性组添加到 RD Web 和 RD 网关的 web 前端](rds-rdweb-gateway-ha.md)
   - [部署为 UPD 存储的双节点存储空间直通的文件系统](rds-storage-spaces-direct-deployment.md)


如果您希望使用远程桌面与客户或查找的人来承载您的应用程序的客户提供应用和资源的托管合作伙伴，请查看[远程桌面服务托管合作伙伴](rds-hosting-partners.md)有关的信息评估您可能需要在 Azure 中使用 RDS，作为宿主环境，以及已传递它的合作伙伴的列表。
