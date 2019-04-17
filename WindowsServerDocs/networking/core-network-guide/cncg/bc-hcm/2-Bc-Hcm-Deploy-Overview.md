---
title: 分支缓存托管缓存模式部署概述
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: feab3156b89637f64d1af0250df459533b1662c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>分支缓存托管缓存模式部署概述

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以使用本指南部署的计算机连接到域的位置 branch 办公室中分支缓存托管缓存服务器。 你可以使用本主题以获取分支缓存托管的缓存模式部署过程的概述。

本概述包括需要简单的分步部署概述以及分支缓存的基础结构。

## <a name="bkmk_components"></a>托管的缓存服务器部署基础结构

在此部署，托管的缓存服务器部署使用 Active Directory 域服务 \(AD DS\) 在服务的连接点和你与分支缓存的选项中有 Windows Server 2016，Windows Server 2012 R2，并且基于内容服务器、 Windows Server 2012 以 prehash 上 Web 和文件的共享的内容，然后预加载托管的缓存服务器上的内容。

下图显示部署分支缓存托管缓存服务器所需的基础结构。

![分支缓存托管的缓存模式概述](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> 尽管此部署描述的内容服务器云数据中心中的，你可以使用本指南部署无论为在你的主 office 或云位置部署内容的服务器 – 分支缓存托管缓存服务器。

### <a name="hcs1-in-the-branch-office"></a>HCS1 分支办公室中

必须将该计算机配置为托管的缓存服务器。 如果你选择 prehash 内容服务器数据，以便你可以在托管的缓存服务器上预加载内容，你可以导入数据包包含从你的网站和文件服务器的内容。

### <a name="web1-in-the-cloud-data-center"></a>云数据中心中的 WEB1

WEB1 是 BranchCache\ 启用内容服务器。 如果你选择 prehash 内容服务器数据，以便你可以在托管的缓存服务器上预加载内容，你可以 prehash 上 WEB1 的共享的内容，然后创建数据包复制到 HCS1。

### <a name="file1-in-the-cloud-data-center"></a>云数据中心中的文件 1

文件 1 是 BranchCache\ 启用内容服务器。 如果你选择 prehash 内容服务器数据，以便你可以在托管的缓存服务器上预加载内容，你可以 prehash 共享的内容上文件 1，然后创建数据包复制到 HCS1。
  
### <a name="dc1-in-the-main-office"></a>在主 office DC1

DC1 域控制器，并且你必须配置默认域策略或其他更多适用于您使用分支缓存组策略设置以启用服务连接点的自动托管缓存发现的部署的策略。

当 branch 中的客户端计算机刷新的组策略，并且该策略设置应用时，它们会自动找到并开始使用托管的缓存的服务器，在分支机构。

### <a name="client-computers-in-the-branch-office"></a>客户端计算机分支办公室中

客户端计算机以将新分支缓存组策略设置的应用并使客户端地找到并使用托管的缓存的服务器上，你必须刷新组策略。

## <a name="bkmk_overview"></a>托管的缓存服务器部署进程概述

>[!NOTE]
>如何执行以下步骤操作的详细信息部分提供[分支缓存托管的缓存模式部署](4-Bc-Hcm-Deployment.md)。

在这些阶段发生部署分支缓存托管的缓存服务器的过程：

>[!NOTE]
>一些以下步骤是可选的如演示如何 prehash 和预先加载托管的缓存服务器上的内容这些步骤。 在分支缓存部署托管的缓存模式中时，你无需均为 prehash 内容 Web 和文件内容服务器上，以创建数据包，并导入数据包，以便预加载内容与您托管的缓存的服务器。 在此部分中和部分中的步骤会标注为可选[分支缓存托管的缓存模式部署](4-Bc-Hcm-Deployment.md)，以便你可以跳过这些如果你希望。

1. 在 HCS1，使用 Windows PowerShell 命令，将计算机配置为服务器托管的缓存以及 Active Directory 中注册服务连接点。

2. \(Optional\) 上 HCS1，如果不匹配分支缓存默认值部署目标的服务器和托管的缓存，将配置你想要为托管缓存分配的磁盘空间量。 此外配置的磁盘位置你喜欢的托管的缓存。

3. 在服务器上托管的缓存 \(Optional\) prehash 内容，内容在服务器上，创建一个数据包和预先加载的内容。

    > [!NOTE]
    > Prehashing 和预加载托管的缓存服务器上的内容是可选的但是如果你选择 prehash 和预加载，你必须执行以下步骤的所有功能都适用于你部署。 \ (例如，如果你没有 Web 服务器，你不需要执行任何与 prehashing 预加载的 Web 服务器内容相关的步骤。 \)

    1. 上 WEB1，prehash Web 服务器内容，然后创建数据包。

    2. 在文件 1，prehash 文件服务器的内容，然后创建数据包。

    3. 从 WEB1 和文件 1，将复制到托管的缓存服务器 HCS1 的数据包。

    4. 在 HCS1，将导入预先加载的数据缓存的数据包。

4. 在 DC1 上，使用分支缓存策略设置配置组策略配置加入域 branch office 客户端计算机托管的缓存模式。

5. 客户端计算机上刷新组策略。

若要继续使用本指南，请参阅[分支缓存托管缓存模式部署规划](3-Bc-Hcm-Plan.md)。