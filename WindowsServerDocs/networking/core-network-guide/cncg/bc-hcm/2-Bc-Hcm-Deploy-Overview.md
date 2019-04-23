---
title: BranchCache 托管缓存模式部署概述
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 930a9b4872a7a79351055841a5d716dd99df0fa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829508"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache 托管缓存模式部署概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本指南可用于部署 BranchCache 托管缓存服务器，分支机构位置将计算机加入到域中。 可以使用本主题以获取 BranchCache 托管缓存模式部署过程的概述。

此概述包括的分支缓存基础结构的需要以及部署的简单分步概述。

## <a name="bkmk_components"></a>托管的缓存服务器部署基础结构

在此部署中，托管的缓存服务器部署在 Active Directory 域服务中使用服务连接点\(AD DS\)，并且可以选择是否使用 Windows Server 2016、 Windows Server 2012 R2 和 Windows 中的 BranchCacheServer 2012 中，对 Web 和文件共享的内容进行基于内容的服务器，然后预加载的托管的缓存服务器上的内容。

下图显示了部署 BranchCache 托管缓存服务器所需的基础结构。

![BranchCache 托管缓存模式概述](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> 尽管此部署所描述的云数据中心内的内容服务器，可以使用此指南部署 BranchCache 托管缓存服务器而不考虑在总部或云位置中部署内容服务器 –。

### <a name="hcs1-in-the-branch-office"></a>分支机构中 HCS1

必须将此计算机配置为托管的缓存服务器。 如果你选择对内容服务器的数据，以便可以在托管的缓存服务器上预加载内容，可以导入包含你的 Web 和文件服务器中的内容的数据包。

### <a name="web1-in-the-cloud-data-center"></a>在云数据中心中的 WEB1

WEB1 是 BranchCache\-启用内容服务器。 如果你选择对内容服务器的数据，以便可以在托管的缓存服务器上预加载内容，你可以对共享的内容在 WEB1 上，然后创建你将复制到 HCS1 的数据包。

### <a name="file1-in-the-cloud-data-center"></a>FILE1 的云数据中心中

FILE1 是 BranchCache\-启用内容服务器。 如果你选择对内容服务器的数据，以便可以在托管的缓存服务器上预加载内容，可以对 FILE1 上的共享的内容，然后创建你将复制到 HCS1 的数据包。
  
### <a name="dc1-in-the-main-office"></a>总部中的 DC1

DC1 指域控制器，并且您必须配置默认域策略或更适合你的部署，与 BranchCache 组策略设置以启用通过服务连接点的自动托管缓存发现的另一个策略。

当在分支中的客户端计算机刷新的组策略和应用此策略设置时，它们会自动找到并开始在分支机构中使用托管的缓存服务器。

### <a name="client-computers-in-the-branch-office"></a>分支机构中的客户端计算机

你必须应用新的 BranchCache 组策略设置，而以允许客户端查找并使用托管的缓存服务器的客户端计算机上刷新组策略。

## <a name="bkmk_overview"></a>托管的缓存服务器部署过程概述

>[!NOTE]
>节中提供了如何执行这些步骤的详细信息[BranchCache 托管缓存模式部署](4-Bc-Hcm-Deployment.md)。

部署 BranchCache 托管缓存服务器的过程是在这些阶段：

>[!NOTE]
>一些执行以下步骤是可选的例如，演示如何对并预加载托管的缓存服务器上的内容的步骤。 在托管的缓存模式下部署 BranchCache 时，您不需要对 prehash 内容在 Web 和文件内容服务器上，若要创建数据包，并将数据包导入预加载内容在托管的缓存服务器。 在本部分和部分中所述为可选步骤[BranchCache 托管缓存模式部署](4-Bc-Hcm-Deployment.md)，以便可以如果想跳过。

1. 若要将计算机配置为托管的缓存服务器，并在 Active Directory 中注册服务连接点，请在 HCS1，使用 Windows PowerShell 命令。

2. \(可选\)上 HCS1，如果 BranchCache 默认值不匹配您的部署目标服务器和托管的缓存，配置想要分配的托管缓存中的磁盘空间量。 此外配置倾向于使用托管缓存的磁盘位置。

3. \(可选\)对内容的服务器上的内容、 创建数据的包，并预加载托管的缓存服务器上的内容。

    > [!NOTE]
    > 托管的缓存服务器上的哈希和预加载内容是可选的但是如果您选择对并且预加载，您必须执行下面的步骤的所有功能都适用于你的部署。 \(例如，如果还没有 Web 服务器，你不必执行任何与哈希和预加载 Web 服务器内容相关的步骤。\)

    1. 在 WEB1 上，对 Web 服务器内容和创建数据的包。

    2. 在 FILE1，对文件服务器内容和创建数据的包。

    3. 从 WEB1 和 FILE1 将复制到托管的缓存服务器 HCS1 的数据包。

    4. 在 HCS1，导入数据程序包预加载数据缓存。

4. 在 DC1 上，通过与 BranchCache 策略设置配置组策略配置已加入域分支机构为托管的缓存模式的客户端计算机。

5. 客户端计算机上刷新组策略。

若要继续使用本指南，请参阅[BranchCache 托管缓存模式部署规划](3-Bc-Hcm-Plan.md)。