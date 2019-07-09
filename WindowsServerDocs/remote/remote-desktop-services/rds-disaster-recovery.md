---
title: 保护 RDS 部署 - 灾难恢复
description: 了解远程桌面服务的灾难恢复选项
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: a6eac3a50999633d15b1b6dc28608f60f6fef6c7
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743816"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>为远程桌面服务配置灾难恢复

将远程桌面服务部署到你的环境中时，它将成为基础结构的重要组成部分，特别是与用户共享的应用和资源。 如果 RDS 部署由于从网络故障到自然灾难的任何原因而不可用，则用户无法访问这些应用和资源，并且你的业务受到负面影响。 若要避免此问题，可以配置允许你故障转移部署的灾难恢复解决方案 - 如果 RDS 部署由于任何原因而不可用，则可以自动进行备份。

若要在单个组件或计算机不可用的情况下使 RDS 部署继续运行，我们建议配置 RDS 部署以实现高可用性。 可以通过设置 [RDSH 场](rds-scale-rdsh-farm.md)并确保[群集连接代理以实现高可用性](rds-connection-broker-cluster.md)来实现此目的。 

此处推荐的灾难恢复解决方案是保护部署免受毁灭性灾难，这些灾难会关闭整个 RDS 部署（包括配置以实现高可用性的冗余角色）。 如果遇到此类灾难，将灾难恢复解决方案内置于部署中将允许你故障转移整个部署并快速为用户启动并运行应用和资源。

使用以下信息可部署 RDS 中的灾难恢复解决方案：

- [利用多个 Azure 数据中心确保用户可以访问 RDS 部署，即使一个 Azure 数据中心不可用（异地冗余）时也是如此](rds-multi-datacenter-deployment.md)
- [部署 Azure Site Recovery 以在站点到站点或站点到 Azure 故障转移中提供 RDS 组件的故障转移](rds-disaster-recovery-with-azure.md)


