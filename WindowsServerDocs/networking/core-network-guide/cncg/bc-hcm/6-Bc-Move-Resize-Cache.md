---
title: 移动并调整托管缓存的大小（可选）
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cb75e06b5da8ff95fcf763b22c5160ea200035f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853528"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>移动和调整托管的缓存\(可选\)

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

若要将托管的缓存移动到的驱动器和文件夹，您愿意，并指定托管的缓存服务器可用于托管缓存的磁盘空间量，可以使用此过程。

此过程是可选的。 如果缓存位置，默认值\(%windir%\\ServiceProfiles\\NetworkService\\AppData\\本地\\PeerDistPub\)和大小 – 这是 5%的总硬盘空间 – 适合您的部署，不需要更改它们。

必须是要执行此过程的管理员组的成员。

### <a name="to-move-and-resize-the-hosted-cache"></a>若要移动和调整托管的缓存

1. 使用管理员特权打开 Windows PowerShell。

2. 键入以下命令在本地计算机上，将托管的缓存移动到另一个位置，然后按 ENTER。

    > [!IMPORTANT]
    > 运行以下命令之前，替换为参数值，如 – 路径和 – MoveTo，适用于你的部署的值。

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  键入以下命令来调整大小所承载缓存-特别是 datacache\-本地计算机上。 按 Enter。

    > [!IMPORTANT]
    > 运行以下命令之前，替换参数值，如\-百分比，使用适用于你的部署的值。  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  若要验证托管的缓存服务器配置，请键入以下命令并按 ENTER。

    ``` 
    Get-BCStatus
    ``` 

    命令的结果显示 BranchCache 安装的所有方面的状态。 以下是几个 BranchCache 设置和正确的值为每个项：

    -   DataCache |CacheFileDirectoryPath:显示与使用 SetBCCache 命令 – MoveTo 参数提供的值相匹配的硬盘位置。 例如，如果提供的值为 d:\\datacache，命令输出中显示值。

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume:显示与使用 SetBCCache 命令 – 百分比参数提供的值相匹配的数目。 例如，如果提供的值为 20，命令输出中显示的值。

若要继续使用本指南，请参阅[Prehash 和托管缓存服务器上预加载内容&#40;可选&#41;](7-Bc-Prehash-Preload.md)。