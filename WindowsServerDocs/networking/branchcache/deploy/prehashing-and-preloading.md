---
title: 在托管的缓存服务器上预哈希和预加载内容（可选）
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式缓存模式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a0694ebe30c5a927fb2f1152adc792f8b0c9157
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356389"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>在托管的缓存服务器上预哈希和预加载内容（可选）

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此过程来强制创建内容信息（也称为哈希），启用 BranchCache 的 Web 和文件服务器。 你还可以将文件和 web 服务器上的数据收集到可传输到远程托管缓存服务器的包中。  这使你能够在远程托管缓存服务器上预加载内容，使数据可供第一次客户端访问。  
  
你必须是**管理员**的成员或等效于执行此过程。  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Prehash 内容并在托管缓存服务器上预加载内容  
  
1.  登录到包含要预加载的数据的文件或 Web 服务器，并标识要在一个或多个远程托管缓存服务器上加载的文件夹和文件。  
  
2.  以管理员身份运行 Windows PowerShell。 对于每个文件夹和文件，根据内容服务器的类型运行 `Publish-BCFileContent` 命令或 @no__t，以触发哈希生成并向数据包添加数据。  
  
3.  将所有数据添加到数据包后，请使用 `Export-BCCachePackage` 命令将其导出，以生成数据包文件。  
  
4.  使用所选的文件传输技术将数据包文件移动到远程托管缓存服务器。  FTP、SMB、HTTP、DVD 和便携式硬盘均为可行传输。  
  
5.  使用 `Import-BCCachePackage` 命令将数据包文件导入远程托管缓存服务器。  
  

