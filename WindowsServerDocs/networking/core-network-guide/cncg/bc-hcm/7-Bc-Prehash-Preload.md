---
title: 在托管缓存服务器上预哈希和预加载内容（可选）
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b60a1f24b8988d6e394df0faf678467021e0c882
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839388"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>对，并预加载托管的缓存服务器上的内容\(可选\)

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以使用在本部分中的过程在内容服务器上对内容，将内容添加到数据的包，然后在托管的缓存服务器上预加载内容。 

这些过程是可选的因为您并不需要 prehash 和预加载内容托管的缓存服务器上。 

如果不执行预加载内容，数据是自动添加到托管缓存的客户端下载其通过 WAN 连接。

>[!IMPORTANT]
>虽然这些过程是统称为可选的如果你决定 prehash 和预加载内容托管的缓存服务器上执行这两个过程是必需的。

- [创建 Web 内容服务器数据包和文件内容&#40;可选&#41;](8-Bc-Data-Packages.md)
  
- [托管的缓存服务器上的数据程序包导入&#40;可选&#41;](9-Bc-Import-Data.md)

若要继续使用本指南，请参阅[创建内容服务器数据的包适用于 Web 和文件内容&#40;可选&#41;](8-Bc-Data-Packages.md)。