---
title: 保护 RDS 部署的灾难恢复
description: 了解远程桌面服务在灾难恢复选项
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834078"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>为远程桌面服务配置灾难恢复

在远程桌面服务部署到您的环境时，它将成为基础结构，特别是应用程序和与用户共享的资源的关键部分。 如果 RDS 部署发生故障由于任何内容从网络故障到自然灾难，用户无法访问这些应用和资源，和你的业务产生负面影响。 若要避免此问题，可以配置灾难恢复解决方案，允许你向故障转移你的部署-如果由于任何原因而不可用，RDS 部署，则备份中自动接管。

若要保留在 RDS 部署中运行在单个组件或计算机宕机的情况下，我们建议配置 RDS 部署，以实现高可用性。 您可以执行此操作通过设置[RDSH 服务器场](rds-scale-rdsh-farm.md)，并确保你[连接代理群集以实现高可用性](rds-connection-broker-cluster.md)。 

我们在此推荐的灾难恢复解决方案是从灾难性灾难的内容都会关闭整个 RDS 部署 （包括配置以实现高可用性的冗余角色） 保护你的部署。 如果遇到此类灾难时，你的部署中内置的灾难恢复解决方案将允许您为故障转移整个部署和快速应用和资源并为你的用户运行。

使用以下信息来部署 RDS 中的灾难恢复解决方案：

- [利用多个 Azure 数据中心，以确保用户可以访问您的 RDS 部署，即使一个 Azure 数据中心发生服务中断 （异地冗余）](rds-multi-datacenter-deployment.md)
- [部署 Azure Site Recovery 为 RDS 组件在站点到站点或站点到 Azure 故障转移提供故障转移](rds-disaster-recovery-with-azure.md)


