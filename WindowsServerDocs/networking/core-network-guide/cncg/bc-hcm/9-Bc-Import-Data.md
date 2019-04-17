---
title: 导入 （可选） 托管的缓存服务器上的数据包
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0bd8f12ab76c8e2bf03ba79ce46a4cbea2f4dc5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>导入上托管数据包缓存服务器 \(Optional\)

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以使用此过程导入数据包和预先加载托管的缓存服务器上的内容。

此过程是可选的因为你无需均为 prehash 和预先加载内容托管的缓存服务器上。

如果你不是 pre\ 加载内容，数据是自动添加到托管缓存通过 WAN 连接的客户端下载它。

你必须是管理员组要执行此过程的成员。

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>若要导入托管的缓存服务器上的数据包  

1. 在服务器计算机上，打开具有管理员权限的 Windows PowerShell。

2. 键入以下命令，并将值为 – 路径参数的文件夹位置，您已存储到您的数据程序包，并按 ENTER。

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. 如果你有多个托管的缓存服务器你想要预加载的内容，则每个托管的缓存服务器上执行此过程。

若要继续使用本指南，请参阅[将配置客户端自动托管缓存发现服务连接点](10-Bc-Client-By-Scp.md)。
