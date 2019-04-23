---
title: 在托管缓存服务器上导入数据包（可选）
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 440ef1e04143cba09213ffea634aa9d4fea51dab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887998"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>托管的缓存服务器上的数据程序包导入\(可选\)

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

此过程可用于导入数据的包和托管的缓存服务器上预加载内容。

此过程是可选的因为您不需要 prehash 和预加载内容托管的缓存服务器上。

如果执行不预\-负载内容，数据添加到托管缓存自动通过 WAN 连接的客户端下载它。

必须是要执行此过程的管理员组的成员。

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>若要导入数据的托管的缓存服务器上的包  

1. 在服务器上，使用管理员特权打开 Windows PowerShell。

2. 键入以下命令，并将值替换为的文件夹位置，其中存储了数据包中，数据，然后按 ENTER 在 – Path 参数。

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. 如果你有想要预加载内容的多个托管的缓存服务器，每个托管的缓存服务器上执行此过程。

若要继续使用本指南，请参阅[配置客户端自动托管缓存发现通过服务连接点](10-Bc-Client-By-Scp.md)。
