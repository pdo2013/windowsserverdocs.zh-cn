---
title: BranchCache 部署指南
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9bccf69f0a913159a395fabc670a63e2c159bd91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888178"
---
# <a name="branchcache-deployment-guide"></a>BranchCache 部署指南

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本指南，了解如何部署 Windows Server 2016 中的 BranchCache。  
  
除了本主题，本指南包含以下各节。  
  
-   [选择一个 BranchCache 设计](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Deploy BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>BranchCache 部署概述

BranchCache 是某些版本的 Windows Server 2016 中，Windows Server 中包含一广域网 (wan) 带宽优化技术&reg;2012 R2 中，Windows Server&reg; Windows Server 2012&reg; 2008 R2 及相关Windows 客户端操作系统。  
  
若要优化 WAN 带宽，分支缓存从你的总部内容服务器复制内容，并将内容缓存在分支机构位置，使分支机构的客户端计算机可以从本地访问内容，而不是从 WAN 访问。  
  
在分支机构，内容已缓存上运行的 BranchCache 功能的 Windows Server 2016 的服务器、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2-或者，如果有分支机构，内容中的没有可用的服务器为 cac运行 Windows 10 的客户端计算机上的 hed&reg;，Windows&reg; 8.1，Windows 8 或 Windows 7&reg; 。  
  
位于同一分支机构的其他计算机的客户端计算机请求并从 main office 或云数据中心中接收内容而该内容缓存在分支机构后，可以获得本地内容，而不是无需通过联系内容服务器WAN 链接。  
  
**部署 BranchCache 的优势**  
  
BranchCache 缓存文件、 web 和应用程序内容在分支机构位置，使客户端计算机可以使用局域网 (LAN) 来访问数据而不是通过慢速 WAN 连接访问的内容。  
  
BranchCache 可减少 WAN 流量和所需的分支机构用户若要打开网络上的文件的时间。  BranchCache 始终向用户提供了最新的数据，并且通过加密托管的缓存服务器和客户端计算机上的缓存来保护内容的安全性。  
  
### <a name="what-this-guide-provides"></a>本指南提供的内容  
本部署指南，可在以下模式部署 BranchCache:  
  
-   分布式的缓存模式。 在此模式下，分支机构的客户端计算机从总部或云中的内容服务器下载内容，然后将其缓存内容的同一分支机构中的其他计算机。 分布式的缓存模式下不需要在分支机构中的服务器计算机。  
  
-   托管的缓存模式。 在此模式下，分支 office 客户端计算机下载内容从总部或云中的内容服务器和托管的缓存服务器从客户端检索内容。 然后，托管的缓存服务器缓存内容的其他客户端计算机。  
  
本指南还提供有关如何部署三种类型的内容服务器的说明。 内容服务器包含源内容下载的分支机构的客户端计算机，并且一个或多个内容服务器需要在每种模式下部署 BranchCache。 内容服务器类型包括：  
  
-   **Web 服务器基于内容服务器**。 这些内容服务器将内容发送到使用 HTTP 和 HTTPS 协议的 BranchCache 客户端计算机。 这些内容的服务器必须运行支持 BranchCache 的 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 版本中后安装 BranchCache 功能。  
  
-   **基于 BITS 的应用程序服务器**。 这些内容服务器将内容发送到使用后台智能传输服务 (BITS) 的 BranchCache 客户端计算机。 这些内容的服务器必须运行支持 BranchCache 的 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 版本中后安装 BranchCache 功能。  
  
-   **文件基于服务器的内容服务器**。 这些内容的服务器必须运行支持 BranchCache 的 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 版本和服务器角色安装后的哪些文件服务。 此外，**网络文件 BranchCache**必须安装和配置文件服务服务器角色的角色服务。 这些内容服务器将内容发送到使用服务器消息块 (SMB) 协议的 BranchCache 客户端计算机。  
  
有关详细信息，请参阅[BranchCache 的操作系统版本](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache)。  
  
### <a name="branchcache-deployment-requirements"></a>BranchCache 部署的要求。

以下是通过使用本指南部署 BranchCache 的要求。  
  
-   **文件和 Web 内容服务器**必须运行以下操作系统提供 BranchCache 功能之一：Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2。 Windows 8 和更高版本的客户端仍然可以看到从 BranchCache 的优势，但无法进行访问正在运行 Windows Server 2008 R2 的内容服务器时使用的新区块和哈希技术在 Windows Server 2016 中，Windows Server 2012R2 和 Windows Server 2012。  
  
-   **客户端计算机**必须运行 Windows 10、 Windows 8.1 或 Windows 8，以便使用最新的部署模型以及分块和哈希与 Windows Server 2012 引入了改进。  
  
-   **托管缓存服务器**必须运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，若要使用的部署改进并扩展此文档中所述的功能。  若要为运行 Windows 7 的客户端计算机提供服务，但若要执行此操作，它必须配备有适用的传输层安全性 (TLS 的证书，可以继续运行这些配置为托管的缓存服务器的操作系统之一的计算机)，Windows Server 2008 R2 和 Windows 7 中所述[BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)。  
  
-   **Active Directory 域**需要充分利用组策略和托管的缓存自动发现，但域不需要使用 BranchCache。  可以使用 Windows PowerShell 来配置单独的计算机。 此外，不需要你的域控制器都运行 Windows Server 2012 或更高版本，以利用新的 BranchCache 组策略设置;可以导入到运行早期操作系统的域控制器上的 BranchCache 管理模板也可以编写其他正在运行 Windows 10、 Windows Server 2016、 Windows 8.1 的计算机上远程的组策略对象Windows Server 2012 R2、 Windows 8 或 Windows Server 2012。

-   **Active Directory 站点**用于限制托管的缓存服务器自动发现的作用域。  若要自动发现托管的缓存服务器，客户端和服务器计算机必须属于同一个站点。 BranchCache 旨在将影响降到客户端和服务器上的并不会施加之外运行它们各自的操作系统所需的额外的硬件要求。  

**BranchCache 历史记录和文档**

在 Windows 7 中首次引入 BranchCache&reg;和 Windows Server&reg; 2008 R2 和 Windows Server 2012 中，Windows 8 和更高版本操作系统中改进了。

> [!NOTE]
> 如果在非 Windows Server 2016 操作系统中部署 BranchCache，提供了以下的文档资源。
> 
> - 有关 Windows 8、 Windows 8.1、 Windows Server 2012 和 Windows Server 2012 R2 中的 BranchCache 的信息，请参阅[BranchCache 概述](https://technet.microsoft.com/library/hh831696.aspx)。  
> - 有关 Windows 7 和 Windows Server 2008 R2 中的 BranchCache 的信息，请参阅[BranchCache for Windows Server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx)。  
  


