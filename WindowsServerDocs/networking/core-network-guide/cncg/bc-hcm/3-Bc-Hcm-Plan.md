---
title: 分支缓存托管缓存模式部署计划
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e645dd96ec85e3a23df6717cfa43d7627cb938e7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>分支缓存托管缓存模式部署计划

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以使用本主题规划分支缓存的部署型托管的缓存。

>[!IMPORTANT]
>托管的缓存 server 必须运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。

部署托管的缓存 server 之前，你必须计划以下各项：

- [套餐基本服务器配置](#bkmk_basic)

- [套餐域访问](#bkmk_domain)

- [计划的位置和托管缓存的大小](#bkmk_cachelocation)

- [计划内容服务器包要复制的共享](#bkmk_package)

- [套餐 prehashing 和数据程序包内容服务器上的创建](#bkmk_prehash)

## <a name="bkmk_basic"></a>套餐基本服务器配置
  
如果你打算作为托管的缓存服务器，在你的分支机构使用现有的服务器上，你不需要执行此计划的步骤，因为计算机已经名为，并且具有 IP 地址配置。

托管的缓存服务器上安装了 Windows Server 2016 后，你必须重命名的计算机和指定并配置本地计算机的静态 IP 地址。

>[!NOTE]
>在本指南，托管的缓存 server 名为 HCS1，但是你应使用适用于你的部署服务器名称。

## <a name="bkmk_domain"></a>套餐域访问

如果你打算作为托管的缓存服务器，在你的分支机构使用现有的服务器上，你不需要执行此计划的步骤，除非计算机当前未加入域。
  
登录到域，该计算机必须域成员计算机，并且必须在之前的登录尝试广告 DS 创建用户帐户。 此外，你必须计算机加入域具有适当的组成员的帐户。

## <a name="bkmk_cachelocation"></a>计划的位置和托管缓存的大小

在 HCS1，确定托管的缓存服务器上要查找托管的缓存的位置。 例如，决定硬盘、 音量和打算用来存储缓存的文件夹位置。

此外，确定你想要为托管缓存分配的磁盘空间哪些百分比。

## <a name="bkmk_package"></a>计划内容服务器包要复制的共享

在你的内容服务器上创建数据程序包后，你必须将它们复制网络到共享托管的缓存服务器上。

计划的文件夹位置和共享文件夹的共享权限。 此外，如果你内容服务器主机大量数据和您所创建的包将较大的文件，计划副本期间执行操作 off\ 高峰，以便其他人何时需要用于正常的业务运营带宽期间复制操作不使用 WAN 带宽。

## <a name="bkmk_prehash"></a>套餐 prehashing 和数据程序包内容服务器上的创建

在你的内容服务器上 prehash 内容之前，你必须识别文件夹和文件包含你想要添加到数据包的内容。 

此外，你必须计划在你可以将它们复制到托管的缓存服务器之前存储的数据包本地文件夹位置。

若要继续使用本指南，请参阅[分支缓存托管的缓存模式部署](4-Bc-Hcm-Deployment.md)。
