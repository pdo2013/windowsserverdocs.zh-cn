---
title: BranchCache 托管缓存模式部署概述
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc6ade92eb5fe04271033973911ccb98e871d236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406381"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache 托管缓存模式部署概述

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

你可以使用本指南在将计算机加入到域中的分支机构部署 BranchCache 托管缓存服务器。 您可以使用本主题获取 BranchCache 托管缓存模式部署过程的概述。

此概述包括所需的 BranchCache 基础结构，以及部署的简单分步概述。

## <a name="bkmk_components"></a>托管缓存服务器部署基础结构

在此部署中，托管缓存服务器使用 Active Directory 域服务 \(AD DS @ no__t 中的服务连接点进行部署，并且在 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中具有 BranchCache 选项。若要 prehash 基于 Web 和文件的内容服务器上的共享内容，请在托管缓存服务器上预加载内容。

下图显示了部署 BranchCache 托管缓存服务器所需的基础结构。

![BranchCache 托管缓存模式概述](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> 尽管此部署描述了云数据中心内的内容服务器，但你可以使用本指南部署 BranchCache 托管缓存服务器，而不管你将内容服务器部署在哪个位置-在你的总部或云位置。

### <a name="hcs1-in-the-branch-office"></a>分支机构中的 HCS1

您必须将此计算机配置为托管缓存服务器。 如果选择 prehash 内容服务器数据以便可以在托管缓存服务器上预加载内容，则可以导入包含 Web 和文件服务器内容的数据包。

### <a name="web1-in-the-cloud-data-center"></a>云数据中心的 WEB1

WEB1 是 BranchCache @ no__t-0enabled 内容服务器。 如果选择 prehash 内容服务器数据以便可以在托管缓存服务器上预加载内容，则可以在 WEB1 上 prehash 共享内容，然后创建一个复制到 HCS1 的数据包。

### <a name="file1-in-the-cloud-data-center"></a>云中的数据中心

FILE1 是 BranchCache @ no__t-0enabled 内容服务器。 如果选择 prehash 内容服务器数据以便可以在托管缓存服务器上预加载内容，则可以在 FILE1 上 prehash 共享内容，然后创建复制到 HCS1 的数据包。
  
### <a name="dc1-in-the-main-office"></a>总公司中的 DC1

DC1 是一个域控制器，您必须配置默认域策略，或其他更适合于您的部署的策略，并使用 BranchCache 组策略设置来通过服务连接点启用自动托管缓存发现。

当分支中的客户端计算机组策略刷新并且应用此策略设置时，它们会自动定位并开始使用分支机构中的托管缓存服务器。

### <a name="client-computers-in-the-branch-office"></a>分支机构中的客户端计算机

你必须在客户端计算机上刷新组策略以应用新的 BranchCache 组策略设置，并允许客户端查找并使用托管缓存服务器。

## <a name="bkmk_overview"></a>托管缓存服务器部署过程概述

>[!NOTE]
>[BranchCache 托管缓存模式部署](4-Bc-Hcm-Deployment.md)部分提供了有关如何执行这些步骤的详细信息。

部署 BranchCache 托管缓存服务器的过程出现在以下阶段：

>[!NOTE]
>下面的某些步骤是可选的，例如演示如何在托管缓存服务器上 prehash 和预加载内容的步骤。 当你在托管缓存模式下部署 BranchCache 时，无需 prehash Web 和文件内容服务器上的内容，创建数据包，并导入数据包，以便预加载包含内容的托管缓存服务器。 在此部分和[BranchCache 托管缓存模式部署](4-Bc-Hcm-Deployment.md)部分中，这些步骤都是可选的，因此，如果愿意，可以跳过这些步骤。

1. 在 HCS1 上，使用 Windows PowerShell 命令将计算机配置为托管缓存服务器，并在 Active Directory 中注册服务连接点。

2. @no__t HCS1 上的 no__t-1，如果 BranchCache 默认值与服务器和托管缓存的部署目标不匹配，请配置要为托管缓存分配的磁盘空间量。 同时，为托管缓存配置所需的磁盘位置。

3. \(Optional @ no__t-1 Prehash 内容服务器上的内容，创建数据包，并在托管缓存服务器上预加载内容。

    > [!NOTE]
    > 托管缓存服务器上的执行哈希和预加载内容是可选的，但是，如果你选择 prehash 和预加载，则必须执行以下适用于你的部署的所有步骤。 @no__t 0For 示例中，如果你没有 Web 服务器，则无需执行与执行哈希和预加载 Web 服务器内容相关的任何步骤。 \)

    1. 在 WEB1 上，prehash Web 服务器内容并创建数据包。

    2. 在 FILE1 上，prehash 文件服务器内容并创建数据包。

    3. 从 WEB1 和 FILE1，将数据包复制到托管缓存服务器 HCS1。

    4. 在 HCS1 上，导入数据包，预加载数据缓存。

4. 在 DC1 上，通过使用 BranchCache 策略设置配置组策略，为托管缓存模式配置已加入域的分支机构的客户端计算机。

5. 在客户端计算机上，刷新组策略。

若要继续学习本指南，请参阅[BranchCache 托管缓存模式部署规划](3-Bc-Hcm-Plan.md)。