---
title: 分支缓存部署指南
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7aa35260213a8a236b7c27cf74e36692438cd2a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-deployment-guide"></a>分支缓存部署指南

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本指南若要了解如何部署分支缓存中 Windows Server 2016。  
  
本主题中，除了本指南包含以下部分。  
  
-   [选择分支缓存设计](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [部署分支缓存](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>分支缓存部署概述

分支缓存是一种宽的区域广域网带宽优化技术，Windows Server 2016 年，Windows Server 的某些版本中包含&reg;2012 R2、 Windows Server&reg; 2012、 Windows Server&reg; 2008 R2 和相关的 Windows 客户端操作系统。  
  
若要优化 WAN 带宽，分支缓存将从你的主 office 内容服务器的内容，并缓存内容的访问分支机构，使客户端计算机在本地而不是通过 WAN 访问该内容的分支机构。  
  
在分支机构缓存内容上运行的 Windows Server 2016 的分支缓存功能的服务器、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2-，或如果中分支机构提供了没有服务器，在运行 Windows 10 的客户端计算机上缓存内容&reg;，Windows&reg; 8.1、 Windows 8 或 Windows 7&reg; 。  
  
客户端计算机请求接收主的 office 应用商店或云数据中心中的内容和在分支机构缓存内容后，在同一 branch 办公室中的其他计算机可获得本地的内容，而不是 WAN 的链接上联系内容的服务器。  
  
**部署分支缓存的权益**  
  
分支缓存缓存的文件、 web 和应用程序内容的访问分支机构，使客户端计算机，而不是通过 slow WAN 连接访问内容访问使用本地网络 (LAN) 的数据。  
  
分支缓存减少 WAN 路况和所需的分支机构的用户，若要打开网络上的文件的时间。  分支缓存始终为用户提供的最新的数据和它通过加密缓存和客户端计算机上托管的缓存服务器来保护你的内容的安全。  
  
### <a name="what-this-guide-provides"></a>本指南提供的内容  
此部署指南让您可以在以下模式部署分支缓存：  
  
-   分布式的缓存模式。 在此模式中，branch office 客户端计算机从云中进行构建的主办公室中的内容服务器下载内容，然后缓存相同 branch 办公室中的其他计算机的内容。 分布式的缓存模式不需要的服务器计算机分支机构。  
  
-   托管的缓存模式。 在此模式中，branch office 客户端计算机下载内容从云中进行构建的主办公室中的内容服务器和托管的缓存服务器客户端从检索该内容。 然后，托管的缓存服务器缓存其他客户端计算机的内容。  
  
本指南还提供了有关如何部署三种类型的内容服务器的说明进行操作。 内容服务器包含 branch office 客户端计算机，下载的来源内容，并且部署分支缓存任一型所需的一个或多个内容的服务器。 是内容服务器类型：  
  
-   **Web 服务器基于内容服务器**。 这些内容的服务器发送到分支缓存客户端计算机使用 HTTP 和 HTTPS 协议的内容。 这些内容服务器必须运行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 支持分支缓存的版本，在安装分支缓存的功能。  
  
-   **基于位的应用程序服务器**。 这些内容的服务器发送给分支缓存使用后台智能传输服务 (BITS) 的客户端计算机的内容。 这些内容服务器必须运行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 支持分支缓存的版本，在安装分支缓存的功能。  
  
-   **文件服务器基于内容服务器**。 这些内容服务器必须运行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 支持分支缓存的版本以及哪些服务该文件后安装服务器角色。 此外，**网络文件的分支缓存**必须安装并配置文件服务 server 角色的角色服务。 这些内容的服务器发送到分支缓存客户端计算机使用服务器消息块 (SMB) 协议的内容。  
  
有关详细信息，请参阅[分支缓存的操作系统版本](https://technet.microsoft.com/en-us/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache)。  
  
### <a name="branchcache-deployment-requirements"></a>分支缓存部署的要求。

以下是通过使用本指南部署分支缓存的要求。  
  
-   **文件和 Web 内容服务器**以下操作系统提供分支缓存的功能之一必须运行： Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2。 Windows 8 和更高版本的客户端将继续看到从分支缓存的权益，但是它们将无法访问内容运行的 Windows Server 2008 R2 的服务器时新分块和哈希在 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 的技术的使用。  
  
-   **客户端计算机**必须运行 Windows 10、 Windows 8.1 或 Windows 8，以使使用最新的部署型号和分块和哈希引入与 Windows Server 2012 的改进。  
  
-   **托管缓存服务器**必须运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，以使使用部署改进和缩放本文中所述的功能。  当来提供服务正在运行 Windows 7 中的客户端计算机，但若要执行此操作，可以继续托管的缓存服务器配置计算机运行下列操作系统之一的必须配备适合用于传输层安全性 (TLS)，Windows Server 2008 R2 和 Windows 7 中所述的证书[分支缓存部署指南](https://technet.microsoft.com/en-us/library/ee649232.aspx)。  
  
-   **Active Directory 域**需要充分利用组策略和托管的缓存自动发现，但不是需要使用分支缓存域。  你可以通过使用 Windows PowerShell 配置单独的计算机。 此外，不需要，域控制器正在运行 Windows Server 2012 或更高版本，来利用新分支缓存组策略设置。可以将导入正在运行早期版本的操作系统的域控制器上分支缓存管理模板，或者你可以制作组策略对象远程上运行的 Windows 10、 Windows Server 2016、 Windows 8.1、 Windows Server 2012 R2、 Windows 8 或 Windows Server 2012 的其他计算机。

-   **Active Directory 站点**用于限制托管的缓存服务器自动发现的范围。  若要自动发现托管的缓存服务器的客户端和服务器的计算机必须属于相同的站点。 分支缓存旨在最少对产生的影响客户端和服务器，并且没有限制额外的硬件以外，他们各自的操作系统的运行所需的要求。  

**分支缓存历史记录和文档**

首次在 Windows 7 引入分支缓存&reg;和 Windows Server&reg; 2008 R2 和 Windows Server 2012、 Windows 8 和更高版本操作系统中也已经过改进。

> [!NOTE]
> 如果你要在 Windows Server 2016 以外的操作系统部署分支缓存，有足够的以下的文档资源。
> 
> - 有关分支缓存中 Windows 8、 Windows 8.1、 Windows Server 2012 和 Windows Server 2012 R2 的信息，请参阅[分支缓存概述](https://technet.microsoft.com/en-us/library/hh831696.aspx)。  
> - 有关分支缓存中 Windows 7 和 Windows Server 2008 R2 的信息，请参阅[分支缓存的 Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd996634.aspx)。  
  


