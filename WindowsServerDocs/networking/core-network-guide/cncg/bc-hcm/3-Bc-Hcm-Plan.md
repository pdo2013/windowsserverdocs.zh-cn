---
title: BranchCache 托管缓存模式部署规划
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0fe55bc9971606559af652d592a91db7a89544a7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356373"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache 托管缓存模式部署规划

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

你可以使用本主题来计划在托管缓存模式下部署 BranchCache。

>[!IMPORTANT]
>托管缓存服务器必须运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

在部署托管缓存服务器之前，必须计划以下各项：

- [规划基本服务器配置](#bkmk_basic)

- [规划域访问](#bkmk_domain)

- [规划托管缓存的位置和大小](#bkmk_cachelocation)

- [规划要将内容服务器包复制到的共享](#bkmk_package)

- [在内容服务器上计划执行哈希和数据包创建](#bkmk_prehash)

## <a name="bkmk_basic"></a>规划基本服务器配置
  
如果打算使用分支机构中的现有服务器作为托管缓存服务器，则无需执行此规划步骤，因为计算机已经命名并且具有 IP 地址配置。

在托管缓存服务器上安装 Windows Server 2016 后，你必须重命名计算机，并为本地计算机分配和配置静态 IP 地址。

>[!NOTE]
>在本指南中，托管缓存服务器的名称为 HCS1，但应使用适用于你的部署的服务器名称。

## <a name="bkmk_domain"></a>规划域访问

如果打算使用分支机构中的现有服务器作为托管缓存服务器，则无需执行此规划步骤，除非计算机当前未加入域。
  
若要登录到域，计算机必须是域成员计算机，并且必须在登录尝试之前 AD DS 中创建用户帐户。 此外，您必须使用具有相应组成员身份的帐户将计算机加入域。

## <a name="bkmk_cachelocation"></a>规划托管缓存的位置和大小

在 HCS1 上，确定你要在托管缓存服务器上查找托管缓存的位置。 例如，决定你计划存储缓存的硬盘、卷和文件夹位置。

此外，确定要为托管缓存分配的磁盘空间百分比。

## <a name="bkmk_package"></a>规划要将内容服务器包复制到的共享

在内容服务器上创建数据包后，必须通过网络将它们复制到托管缓存服务器上的共享中。

为共享文件夹规划文件夹位置和共享权限。 此外，如果您的内容服务器托管大量数据，而您创建的包将是大文件，则计划在非高峰时间执行复制操作，以便复制操作在其他人需要使用时不会消耗 WAN 带宽 正常业务运营的带宽。

## <a name="bkmk_prehash"></a>在内容服务器上计划执行哈希和数据包创建

在内容服务器上 prehash 内容之前，必须确定包含要添加到数据包的内容的文件夹和文件。 

此外，还必须在将数据包复制到托管缓存服务器之前，在本地文件夹位置上进行规划。

若要继续学习本指南，请参阅[BranchCache 托管缓存模式部署](4-Bc-Hcm-Deployment.md)。
