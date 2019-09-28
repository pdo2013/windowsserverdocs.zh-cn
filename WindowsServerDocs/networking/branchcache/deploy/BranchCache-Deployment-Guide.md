---
title: BranchCache 部署指南
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 14eb9e5b4d5a28a64d3cfa0d27b5294ba7168da9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356729"
---
# <a name="branchcache-deployment-guide"></a>BranchCache 部署指南

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本指南了解如何在 Windows Server 2016 中部署 BranchCache。  
  
除了本主题之外，本指南包含以下部分。  
  
-   [选择 BranchCache 设计](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [部署 BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>BranchCache 部署概述

BranchCache 是一种广域网络（WAN）带宽优化技术，包含在某些版本的 Windows Server 2016、Windows Server @ no__t-0 2012 R2、Windows Server @ no__t-1 2012、Windows Server @ no__t-2 2008 R2 和相关的 Windows 客户端中。操作系统。  
  
若要优化 WAN 带宽，分支缓存从你的总部内容服务器复制内容，并将内容缓存在分支机构位置，使分支机构的客户端计算机可以从本地访问内容，而不是从 WAN 访问。  
  
在分支机构，内容缓存在运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows server 2008 R2 的 BranchCache 功能的服务器上，或者，如果分支机构中没有可用的服务器，则内容为 cac运行 Windows 10 @ no__t-0、Windows @ no__t-1 8.1、Windows 8 或 Windows 7 @ no__t-2 的客户端计算机上的 hed。  
  
在客户端计算机请求并接收到总部或云数据中心的内容并将内容缓存在分支机构之后，同一分支机构的其他计算机可以本地获取内容，而不是通过WAN 链接。  
  
**部署 BranchCache 的优点**  
  
BranchCache 将文件、web 和应用程序内容缓存在分支机构位置，使客户端计算机可以使用局域网（LAN）访问数据，而不是通过慢速 WAN 连接访问内容。  
  
BranchCache 同时降低 WAN 流量和分支机构用户在网络上打开文件所需的时间。  BranchCache 始终向用户提供最新数据，并通过加密托管缓存服务器和客户端计算机上的缓存来保护内容的安全性。  
  
### <a name="what-this-guide-provides"></a>本指南提供的内容  
本部署指南允许您在以下模式下部署 BranchCache：  
  
-   分布式缓存模式。 在此模式下，分支机构客户端计算机从总公司或云中的内容服务器下载内容，然后为同一分支机构中的其他计算机缓存内容。 分布式缓存模式不需要分支机构中的服务器计算机。  
  
-   托管缓存模式。 在此模式下，分支机构客户端计算机从总公司或云中的内容服务器下载内容，而托管缓存服务器则从客户端检索内容。 然后，托管缓存服务器将为其他客户端计算机缓存内容。  
  
本指南还提供了有关如何部署三种类型的内容服务器的说明。 内容服务器包含分支机构客户端计算机下载的源内容，并且在任一模式下部署 BranchCache 需要一个或多个内容服务器。 内容服务器类型为：  
  
-   **基于 Web 服务器的内容服务器**。 这些内容服务器使用 HTTP 和 HTTPS 协议将内容发送到 BranchCache 客户端计算机。 这些内容服务器必须运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 版本，这些版本支持 BranchCache 并在其上安装 BranchCache 功能。  
  
-   **基于 BITS 的应用程序服务器**。 这些内容服务器使用后台智能传输服务（BITS）将内容发送到 BranchCache 客户端计算机。 这些内容服务器必须运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 版本，这些版本支持 BranchCache 并在其上安装 BranchCache 功能。  
  
-   **基于文件服务器的内容服务器**。 这些内容服务器必须运行支持 BranchCache 且安装了文件服务服务器角色的 windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 版本。 此外，必须安装并配置文件服务服务器角色的 "**网络文件 BranchCache** " 角色服务。 这些内容服务器使用服务器消息块（SMB）协议将内容发送到 BranchCache 客户端计算机。  
  
有关详细信息，请参阅[BranchCache 的操作系统版本](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache)。  
  
### <a name="branchcache-deployment-requirements"></a>BranchCache 部署要求

下面是使用本指南部署 BranchCache 的要求。  
  
-   **文件和 Web 内容服务器**必须运行以下操作系统之一才能提供 BranchCache 功能：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2。 在访问运行 Windows Server 2008 R2 的内容服务器时，windows 8 和更高版本的客户端将继续看到 BranchCache 的权益，但无法利用 Windows Server 2016、Windows Server 2012R2 和 Windows Server 2012。  
  
-   **客户端计算机**必须运行 windows 10、Windows 8.1 或 windows 8 才能使用最新的部署模型以及 Windows Server 2012 引入的分块和哈希改进。  
  
-   **托管缓存服务器**必须运行 windows server 2016、windows Server 2012 R2 或 windows server 2012 才能使用本文档中介绍的部署改进和缩放功能。  运行配置为托管缓存服务器的其中一种操作系统的计算机可以继续为运行 Windows 7 的客户端计算机提供服务，但为此，必须配备适用于传输层安全性（TLS）的证书），如 Windows Server 2008 R2 和 Windows 7 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)中所述。  
  
-   需要**Active Directory 域**才能利用组策略和托管缓存自动发现，但不要求域使用 BranchCache。  可以使用 Windows PowerShell 配置单个计算机。 此外，不要求域控制器运行 Windows Server 2012 或更高版本以利用新的 BranchCache 组策略设置;你可以将 BranchCache 管理模板导入到运行早期版本操作系统的域控制器上，也可以在运行 Windows 10、Windows Server 2016、Windows 8.1 的其他计算机上远程创作组策略对象。Windows Server 2012 R2、Windows 8 或 Windows Server 2012。

-   **Active Directory 站点**用于限制被自动发现的托管缓存服务器的作用域。  若要自动发现托管缓存服务器，客户端计算机和服务器计算机必须属于同一站点。 BranchCache 的设计目的是对客户端和服务器的影响最小，并且除了运行其各自操作系统所需的硬件要求外，不需要额外的硬件要求。  

**BranchCache 历史记录和文档**

BranchCache 是在 Windows 7 @ no__t-0 和 Windows Server @ no__t-1 2008 R2 中首次引入的，已在 Windows Server 2012、Windows 8 及更高版本的操作系统中得到改进。

> [!NOTE]
> 如果在 Windows Server 2016 以外的操作系统中部署 BranchCache，则可以使用以下文档资源。
> 
> - 有关 Windows 8、Windows 8.1、Windows Server 2012 和 Windows Server 2012 R2 中的 BranchCache 的信息，请参阅[Branchcache 概述](https://technet.microsoft.com/library/hh831696.aspx)。  
> - 有关 Windows 7 和 Windows Server 2008 R2 中的 BranchCache 的信息，请参阅[Windows Server 2008 r2 的 branchcache](https://technet.microsoft.com/library/dd996634.aspx)。  
  


