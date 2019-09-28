---
title: 在托管缓存服务器上预哈希和预加载内容（可选）
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe206576278b09e4a360c7bb27f5ff076af97be7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356253"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>托管缓存服务器上的 Prehash 和预加载内容 \(Optional @ no__t-1

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

你可以使用本部分中的过程来 prehash 内容服务器上的内容、将内容添加到数据包，然后在托管缓存服务器上预加载内容。 

这些过程是可选的，因为你不需要在托管缓存服务器上 prehash 和预加载内容。 

如果未预加载内容，则当客户端通过 WAN 连接下载数据时，数据会自动添加到托管缓存。

>[!IMPORTANT]
>尽管这些过程是可选的，但如果你决定在托管缓存服务器上 prehash 和预加载内容，则需要执行这两个过程。

- [为 Web 和文件内容&#40;创建内容服务器数据包（可选）&#41;](8-Bc-Data-Packages.md)
  
- [在托管缓存服务器&#40;上导入数据包（可选）&#41;](9-Bc-Import-Data.md)

若要继续学习本指南，请参阅[为 Web 和文件内容&#40;创建内容服务器数据包&#41;（可选](8-Bc-Data-Packages.md)）。