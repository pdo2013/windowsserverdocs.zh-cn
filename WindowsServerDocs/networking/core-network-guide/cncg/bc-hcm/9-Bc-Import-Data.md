---
title: 在托管缓存服务器上导入数据包（可选）
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 61a6b1ac1dede4caf8d5633ce6a75e2005e190df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356203"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>导入托管缓存服务器上的数据包 \(Optional @ no__t-1

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

您可以使用此过程在托管缓存服务器上导入数据包和预加载内容。

此过程是可选的，因为你不需要在托管缓存服务器上 prehash 和预加载内容。

如果没有预先 no__t-0load 内容，则当客户端通过 WAN 连接下载数据时，会自动将数据添加到托管缓存。

您必须是 Administrators 组的成员才能执行此过程。

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>导入托管缓存服务器上的数据包  

1. 在服务器计算机上，打开具有管理员权限的 Windows PowerShell。

2. 键入以下命令，将– Path 参数的值替换为存储数据包的文件夹位置，然后按 ENTER。

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. 如果你有多个要预加载内容的托管缓存服务器，请在每个托管缓存服务器上执行此过程。

若要继续学习本指南，请参阅[配置客户端通过服务连接点自动托管缓存发现](10-Bc-Client-By-Scp.md)。
