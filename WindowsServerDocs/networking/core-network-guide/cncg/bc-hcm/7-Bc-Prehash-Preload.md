---
title: Prehash 以及预装 （可选） 托管的缓存服务器上的内容
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0e7ffaac4e427222d5539195ecef91768f61c4a3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash 和预上托管内容缓存服务器 \(Optional\)

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

可以使用在此部分中过程 prehash 内容服务器上的内容、 到数据包添加的内容和然后预托管的缓存服务器上的内容。 

这些步骤是可选的因为你无需均为 prehash 和预先加载的内容上托管的缓存服务器。 

如果你未执行预加载的内容，数据将自动添加到托管缓存通过 WAN 连接的客户端下载它。

>[!IMPORTANT]
>尽管这些过程统称可选的如果你决定 prehash 和预先加载内容托管的缓存的服务器，在执行这两个步骤是必需的。

- [创建用于 Web 和文件内容 & #40; 内容服务器数据包可选 & #41;](8-Bc-Data-Packages.md)
  
- [导入上托管的缓存服务器 & #40; 数据包可选 & #41;](9-Bc-Import-Data.md)

若要继续使用本指南，请参阅[创建内容服务器数据包用于 Web 和文件内容 #40; 可选 & #41;](8-Bc-Data-Packages.md).