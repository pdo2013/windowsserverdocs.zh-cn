---
title: 移动和调整托管的缓存 （可选）
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a3ed1d6de1281575b33cdf75a5970d2ed033085
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>移动和调整托管的缓存 \(Optional\)

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

移动到驱动器和您愿意的文件夹托管的缓存和指定托管的缓存 server 可用于托管缓存的磁盘空间量，可以使用此过程。

此过程是可选的。 适用于你的部署默认缓存位置 \(%windir%\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) 和大小，即 5%的总硬盘空间 – 是否，你不需要进行更改。

你必须是管理员组要执行此过程的成员。

### <a name="to-move-and-resize-the-hosted-cache"></a>移动和调整托管的缓存

1. 打开具有管理员权限的 Windows PowerShell。

2. 键入以下命令以托管的缓存移动到其他位置，在本地计算机上，然后按 ENTER。

    > [!IMPORTANT]
    > 运行以下命令之前, 值替换为参数，– 路径，–，如适用于你的部署的值。

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  键入以下命令以调整托管缓存 – 专门 datacache \-在本地计算机上。 按 ENTER。

    > [!IMPORTANT]
    > 运行以下命令之前, 值替换为参数，\-Percentage，如适用于你的部署的值。  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  若要验证托管的缓存服务器配置，请键入以下命令，然后按 ENTER。

    ``` 
    Get-BCStatus
    ``` 

    命令的结果中显示分支缓存安装的所有方面的状态。 以下是几个分支缓存设置和正确的值为每个项目：

    -   DataCache |CacheFileDirectoryPath： 显示相匹配的值附带 SetBCCache 命令的 – MoveTo 参数硬盘位置。 例如，如果提供值 D:\\datacache，此值所示命令输出。

    -   DataCache |MaxCacheSizeAsPercentageOfDiskVolume： 显示相匹配的值随附的 SetBCCache 命令 – 百分比参数数。 例如，如果提供值 20，此值所示命令输出。

若要继续使用本指南，请参阅[Prehash 和预加载内容上托管缓存服务器 #40; 可选 & #41;](7-Bc-Prehash-Preload.md).