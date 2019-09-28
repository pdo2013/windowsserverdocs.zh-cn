---
title: 移动并调整托管缓存的大小（可选）
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b0e3b6b490dead32071d99becccd9dca937f1f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406362"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>移动并调整托管缓存 \(Optional @ no__t-1

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

可以使用此过程将托管缓存移到所需的驱动器和文件夹，并指定托管缓存服务器可用于托管缓存的磁盘空间量。

此过程是可选的。 如果默认缓存位置 \(% windir% \\ServiceProfiles @ no__t-2NetworkService @ no__t-3AppData @ no__t-4Local @ no__t-5PeerDistPub @ no__t-6 和 size –这是总硬盘空间的 5% –适用于你的部署，你不需要更改它们。

您必须是 Administrators 组的成员才能执行此过程。

### <a name="to-move-and-resize-the-hosted-cache"></a>移动托管缓存并调整其大小

1. 以管理员权限打开 Windows PowerShell。

2. 键入以下命令，将托管缓存移到本地计算机上的其他位置，然后按 ENTER。

    > [!IMPORTANT]
    > 运行以下命令之前，请将参数值（如– Path 和– MoveTo）替换为适用于你的部署的值。

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  键入以下命令来调整托管缓存的大小–具体来说就是本地计算机上的 datacache \-。 按 Enter。

    > [!IMPORTANT]
    > 运行以下命令之前，请将参数值（如 @no__t 0Percentage）替换为适用于你的部署的值。  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  若要验证托管缓存服务器配置，请键入以下命令，然后按 ENTER。

    ``` 
    Get-BCStatus
    ``` 

    命令的结果显示 BranchCache 安装的所有方面的状态。 下面是一些 BranchCache 设置，以及每个项的正确值：

    -   DataCache |CacheFileDirectoryPath:显示与通过 SetBCCache 命令的– MoveTo 参数一起提供的值匹配的硬盘位置。 例如，如果提供的值为 D： \\datacache，则该值将显示在命令输出中。

    -   DataCache |MaxCacheSizeAsPercentageOfDiskVolume:显示与通过 SetBCCache 命令的–百分率参数一起提供的值匹配的数字。 例如，如果提供的值为20，则该值将显示在命令输出中。

若要继续学习本指南，请参阅[Prehash 并在托管缓存服务器&#40;上预&#41;加载内容（可选](7-Bc-Prehash-Preload.md)）。