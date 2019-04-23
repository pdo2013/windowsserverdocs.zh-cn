---
title: 在托管的缓存服务器上预哈希和预加载内容（可选）
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b421132a44240520e3e3ba294623584c36b18ab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866998"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>在托管的缓存服务器上预哈希和预加载内容（可选）

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于强制内容信息-也称为哈希-启用了 BranchCache 的 Web 和文件服务器上的创建。 此外可以为可以传输到远程的托管的缓存服务器的包文件和 web 服务器上收集数据。  这提供了功能，以便可用于在首次客户端访问的数据预加载远程托管的缓存服务器上的内容。  
  
您必须是属于**管理员**，或相当于执行此过程。  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>若要对内容和预加载的托管的缓存服务器上的内容  
  
1.  登录到文件或包含你想要预加载，数据的 Web 服务器并确定的文件夹和想要加载一个或多个远程托管的缓存服务器的文件。  
  
2.  以管理员身份运行 Windows PowerShell。 对于每个文件夹和文件，请运行`Publish-BCFileContent`命令或`Publish-BCWebContent`命令，具体取决于内容服务器，以触发哈希生成，并将数据添加到数据包类型。  
  
3.  所有数据已都添加到数据包后，导出使用`Export-BCCachePackage`命令以生成数据的包文件。  
  
4.  使用所选的文件传输技术将数据包文件移动到远程的托管的缓存服务器。  FTP、 SMB、 HTTP、 DVD 和便携式硬盘是所有可行的传输。  
  
5.  使用导入远程托管的缓存服务器上的数据包文件`Import-BCCachePackage`命令。  
  

