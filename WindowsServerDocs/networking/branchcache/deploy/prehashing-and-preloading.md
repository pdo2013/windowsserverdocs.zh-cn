---
title: Prehashing 和预加载内容上托管的缓存服务器（可选）
description: 本主题介绍分支缓存部署指南为 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中的 WAN 带宽使用量部署分支缓存的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3d1ed62c6dca5b1de0ff560fde0a2e43ed0d080
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Prehashing 和预加载内容上托管的缓存服务器（可选）

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程强制的内容的信息的也称为分支缓存启用网站和文件服务器的哈希-创建。 你可以将传输到远程托管的缓存服务器的程序包到文件和 web 服务器上收集的数据。  这为您提供预加载远程托管的缓存服务器上的内容，以便适用于第一次的客户端访问数据的能力。  
  
你必须成员**管理员**，或相当要执行此过程。  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>若要 prehash 内容以及预装托管的缓存服务器上的内容  
  
1.  登录到的文件或 Web 服务器，其中包含你希望预加载的数据，并找出的文件夹和你希望加载的一个或多个远程托管的缓存服务器的文件。  
  
2.  以管理员身份运行的 Windows PowerShell。 对于每个文件夹和文件，或者运行`Publish-BCFileContent`命令或`Publish-BCWebContent`命令，具体取决于的内容服务器上，触发哈希代和添加到数据包的数据类型。  
  
3.  已添加到数据包的所有数据后，将其导出使用`Export-BCCachePackage`命令来繁衍数据包文件。  
  
4.  使用你选择的文件传输技术，将数据包文件移动到远程托管的缓存服务器上。  FTP、SMB、HTTP、DVD 以及便携硬盘是所有可行传输。  
  
5.  通过使用导入远程托管的缓存服务器上的数据包文件`Import-BCCachePackage`命令。  
  

