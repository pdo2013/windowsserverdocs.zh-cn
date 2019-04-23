---
title: BranchCache 托管缓存模式部署规划
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7232f8732e7476b955115741b5582a585dc6068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890678"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache 托管缓存模式部署规划

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题可用于规划的 BranchCache 托管缓存模式中的部署。

>[!IMPORTANT]
>托管的缓存服务器必须运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。

部署托管的缓存服务器之前，必须规划以下各项：

- [规划基本服务器配置](#bkmk_basic)

- [规划域访问](#bkmk_domain)

- [计划的位置和大小的托管缓存](#bkmk_cachelocation)

- [规划内容服务器的包将复制到的共享](#bkmk_package)

- [计划执行哈希和数据内容服务器上的创建包](#bkmk_prehash)

## <a name="bkmk_basic"></a>规划基本服务器配置
  
如果您计划在分支机构中使用现有的服务器作为托管的缓存服务器，您不需要执行此计划的步骤，因为计算机已命名为，并且具有一个 IP 地址配置。

在托管的缓存服务器上安装 Windows Server 2016 后，必须重命名计算机和分配并配置本地计算机的静态 IP 地址。

>[!NOTE]
>在本指南中，托管的缓存服务器名为 HCS1，但是您应使用适合于你的部署的服务器名称。

## <a name="bkmk_domain"></a>规划域访问

如果您计划在分支机构中使用现有的服务器作为托管的缓存服务器，你不必执行此计划的步骤，除非计算机当前未加入到域。
  
若要登录到域，计算机必须是域成员计算机，必须在之前尝试登录 AD DS 中创建的用户帐户。 此外，您必须在计算机使用加入域具有相应的组成员身份的帐户。

## <a name="bkmk_cachelocation"></a>计划的位置和大小的托管缓存

在 HCS1，确定要在其中托管的缓存服务器上您放置的托管的缓存中。 例如，决定硬盘、 卷和计划来存储缓存的文件夹位置。

此外，决定你想要为托管缓存分配的磁盘空间百分比。

## <a name="bkmk_package"></a>规划内容服务器的包将复制到的共享

在内容服务器上创建数据包后，您必须它们通过网络复制到共享托管的缓存服务器上。

计划的文件夹位置和共享文件夹的共享权限。 此外，如果内容服务器承载大量的数据，并且你创建的包将大型文件，计划执行复制操作在 off\ – 高峰时段，以便复制操作期间时其他人需要使用一次不使用 WAN 带宽 常规业务运营的带宽。

## <a name="bkmk_prehash"></a>计划执行哈希和数据内容服务器上的创建包

在内容服务器上对内容之前，必须确定的文件夹和文件包含你想要添加到数据包的内容。 

此外，您必须计划，然后将其复制到托管的缓存服务器存储的数据程序包的本地文件夹位置。

若要继续使用本指南，请参阅[BranchCache 托管缓存模式部署](4-Bc-Hcm-Deployment.md)。
