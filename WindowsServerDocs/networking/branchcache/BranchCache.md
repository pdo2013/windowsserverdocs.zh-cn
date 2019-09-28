---
title: BranchCache
description: 本主题概述了 Windows Server 2016 中的 BranchCache
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7fe8d948a5f43fdab394490f543f3583167bdfe9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406769"
---
# <a name="branchcache"></a>BranchCache

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题旨在为信息技术 (IT) 专业人员提供 BranchCache 的概述信息，包括 BranchCache 模式、特性、功能和不同操作系统中可用的 BranchCache 功能。

> [!NOTE]
> 除了本主题，以下 BranchCache 文档也可用。
> 
> - [BranchCache Network Shell 和 Windows PowerShell 命令](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [BranchCache 部署指南](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**谁会对 BranchCache 感兴趣？**

如果你是系统管理员、网络或存储解决方案架构师或其他 IT 专业人士，BranchCache 可能会在以下情况中引起你的兴趣：

- 你为某个组织设计或支持 IT 基础结构，该组织拥有两个或多个物理位置以及一个从分支机构到总部的广域网 (WAN) 连接。

- 你为某个组织设计或支持 IT 基础结构，该组织部署了云技术，并且工作人员使用 WAN 连接访问位于远程位置的数据和应用程序。

- 你希望通过减少分支机构和总部之间的网络通信优化 WAN 带宽使用率。

- 你已在总部部署或正计划部署匹配本主题中所描述配置的内容服务器。

- 分支机构中的客户端计算机运行的是 Windows 10、Windows 8.1、Windows 8 或 Windows 7。

本主题包含以下各节：

-   [什么是 BranchCache？](#bkmk_what)

-   [BranchCache 模式](#BKMK_2)
  
-   [启用 BranchCache 的内容服务器](#BKMK_3)
  
-   [BranchCache 和云](#BKMK_3a)
  
-   [内容信息版本](#bkmk_version)  
  
-   [BranchCache 如何处理文件中的内容更新](#bkmk_handles)  
  
-   [BranchCache 安装指南](#BKMK_4)  
  
-   [BranchCache 的操作系统版本](#bkmk_os)  
  
-   [BranchCache 安全](#bkmk_security)  
  
-   [内容流和进程](#bkmk_flow)  
  
-   [缓存安全性](#bkmk_cache)  
  
## <a name="bkmk_what"></a>什么是 BranchCache？

BranchCache 是一种广域网络（WAN）带宽优化技术，包含在某些版本的 Windows Server 2016 和 Windows 10 操作系统以及某些版本的 Windows server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8 中。、Windows Server 2008 R2 和 Windows 7。 为了在用户访问远程服务器上的内容时优化 WAN 带宽，BranchCache 从总部或托管的云内容服务器复制内容，并将内容缓存在分支机构位置，使分支机构的客户端计算机可以从本地访问内容，而不是从 WAN 访问。
  
在分支机构，内容存储在配置为托管缓存的服务器上，或在分支机构中没有可用服务器的情况下，在运行 Windows 10、Windows 8.1、Windows 8 或 Windows 7 的客户端计算机上存储。 当某客户端计算机请求并接收到来自总部的内容，而该内容又缓存在分支机构时，位于同一分支机构的其他计算机可以从本地获得内容，而不是通过 WAN 链接从内容服务器下载内容。

客户端计算机后续请求同一内容时，客户端将从服务器下载*内容信息*，而不是实际内容。 内容信息包括使用大量原始内容计算的哈希，并且哈希和原始数据中内容相比极其地小。 客户端计算机随后会使用内容信息从分支机构内的缓存找到内容，无论缓存是位于客户端计算机还是服务器上。 客户端计算机和服务器还会使用内容信息确保缓存内容的安全，以防未经授权的用户访问这些内容。

BranchCache 通过改善分支机构中客户端和服务器的内容查询响应时间提高最终用户的工作效率，还有助于减少通过 WAN 链接的流量以提升网络性能。

## <a name="BKMK_2"></a>BranchCache 模式
BranchCache 有两种操作模式：分布式缓存模式和托管缓存模式。

当你在分布式缓存模式中部署 BranchCache 时，会在客户端计算机之间分布分支机构的内容缓存。

当你在托管缓存模式中部署 BranchCache 时，分支机构的内容缓存会托管在一个或多个称为托管缓存服务器的服务器计算机上。

> [!NOTE]
> 你可使用两种模式部署 BranchCache，但每个分支机构只能使用一种模式。 例如，如果你有两个分支机构，其中一个拥有一个服务器，另一个没有服务器，你可在包含一个服务器的机构中以托管缓存模式部署 BranchCache，再在只包含客户端计算机的机构中以分布式缓存模式部署 BranchCache。

下图中，BranchCache 是以两种模式部署。  

![BranchCache 模式](../media/BranchCache/bc_modes.jpg)

分布式缓存模式最适合没有充当托管缓存服务器的本地服务器的小型分支机构。 分布式缓存模式允许你不需使用分支机构中任何其他硬件就能部署 BranchCache。

如果你希望为其部署 BranchCache 的分支机构包含其他基础结构，例如一个或多个运行其他工作负载的服务器，那么以托管缓存模式部署 BranchCache 会因为以下原因有所裨益：

### <a name="increased-cache-availability"></a>提高的缓存可用性

因为即使最初请求和缓存数据的客户端为脱机，内容也可用，所以托管缓存模式提高了缓存效率。 由于托管缓存服务器始终可用，更多内容得到缓存，从而节省更多 WAN 带宽，并提高 BranchCache 效率。

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>多个子网分支机构的集中缓存


分布式缓存模式在单一子网上操作。 在一个配置为分布式缓存模式的多子网分支机构中，下载到某个子网的文件就无法与其他子网上的客户端计算机共享。 

因为这一点，其他子网上的客户端无法发现已下载的文件，需要在进程中使用 WAN 带宽从总部内容服务器获得文件。

但当你部署托管缓存模式时，情况并非如此 - 多子网分支机构中的所有客户端都可以访问一个存储在托管缓存服务器上的缓存，即使客户端位于不同子网上。 此外，Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的 BranchCache 提供了为每个分支机构部署多个托管缓存服务器的功能。

> [!CAUTION]
> 如果将 BranchCache 用于文件和文件夹的 SMB 缓存，请不要禁用脱机文件。 如果禁用脱机文件，BranchCache SMB 缓存将无法正确运行。

## <a name="BKMK_3"></a>启用 BranchCache 的内容服务器

部署 BranchCache 时，源内容会存储在总部中启用了 BranchCache 的内容服务器上，或存储在云数据中心。 BranchCache 支持以下类型的内容服务器：

> [!NOTE]
> 只有源内容-即客户端计算机最初从启用了 BranchCache 的内容服务器获取的内容-由 BranchCache 加速。 客户端计算机直接从其他源（例如 Internet 上的 Web 服务器或 Windows 更新）获得的内容不会得到客户端计算机或托管缓存服务器缓存，也不会与分支机构内其他计算机共享。 不过，如果想要加速 Windows 更新内容，则可以在总公司或云数据中心安装 Windows Server Update Services （WSUS）应用程序服务器，并将其配置为 BranchCache 内容服务器。

### <a name="web-servers"></a>Web 服务器

支持的 Web 服务器包括运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 并使用超文本传输协议（HTTP）或 HTTP Secure 的计算机（HTTPS）。

另外，Web 服务器必须安装 BranchCache 功能。

### <a name="file-servers"></a>文件服务器

支持的文件服务器包括运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 且安装了文件服务服务器角色和网络文件 BranchCache 角色服务的计算机。 

这些文件服务器使用服务器消息块 (SMB) 在计算机之间交换信息。 在你安装完文件服务器后，还必须通过使用组策略或本地计算机策略来共享文件夹并启用共享文件夹哈希生成才能启用 BranchCache。

### <a name="application-servers"></a>应用程序服务器

支持的应用程序服务器包括运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 且已安装并启用后台智能传输服务（BITS）的计算机。 

另外，应用程序服务器必须安装 BranchCache 功能。 作为应用程序服务器的示例，你可以将 Microsoft Windows Server Update Services （WSUS）和 Microsoft System Center Configuration Manager 分支分发点服务器部署为 BranchCache 内容服务器。

## <a name="BKMK_3a"></a>BranchCache 和云

云具备减少操作开支并使规模达到新高度的巨大潜力，但为依赖这些因素的工作人员减少工作负荷会增加网络成本并影响工作效率。 用户需要高性能，并不关心其应用程序和数据的托管位置。 

BranchCache 可以改善网络应用程序的性能并通过共享数据缓存减少带宽使用。  这就能提升分支机构和总部的工作效率，工作人员可在其中使用部署在云中的服务器。

由于 BranchCache 不需要对硬件或网络拓扑进行新的更改，所以它是用于改善各机构位置以及公有与私有云之间通信的卓越解决方案。

> [!NOTE]
> 由于某些 Web 代理无法处理非标准内容编码标头，因此建议你将 BranchCache 与超文本传输协议（HTTPS）和 not HTTP 配合使用。
  
= = = = = = =。有关 Windows Server 2016 中的云技术的详细信息，请参阅[软件定义的网络&#40;SDN&#41;](../sdn/Software-Defined-Networking--SDN-.md)。
  
## <a name="bkmk_version"></a>内容信息版本

内容信息有以下两个版本：

- 与运行 Windows Server 2008 R2 和 Windows 7 的计算机兼容的内容信息称为版本1或 V1。 如果使用 V1 BranchCache 文件分段，文件分段会比 V2 中的分段大且大小固定。 由于分段大小大且固定，因此当用户进行修改（修改文件长度）时，不仅进行该更改的分段无效，而且文件末尾的所有分段都无效。 因此，分支机构中的其他用户对所更改文件的下一次调用会导致 WAN 带宽节省降低，原因是更改的内容和更改后的所有内容都会通过 WAN 链接发送。

- 与运行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012 和 Windows 8 的计算机兼容的内容信息称为版本2或 V2。 V2 内容信息使用较小且大小可变的分段，这些分段更易于应对文件中的更改。 这样增加了当用户访问更新版本时可重复使用较旧文件版本中分段的可能性，从而导致它们仅从内容服务器检索文件的更改部分以及使用的 WAN 带宽更少。

下表中提供了有关内容信息版本的信息，该信息的使用取决于你在 BranchCache 部署中使用的客户端、内容服务器以及托管缓存服务器操作系统。

> [!NOTE]
> 在下表中，首字母缩略词 "OS" 表示操作系统。

|客户端 OS|内容服务器 OS|托管缓存服务器 OS|内容信息版本|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 和 Windows 7 |Windows Server 2012 或更高版本|Windows Server 2012 或更高版本;对于分布式缓存模式为 none|V1|
|Windows Server 2012 或更高版本;Windows 8 或更高版本|Windows Server 2008 R2 |Windows Server 2012 或更高版本;对于分布式缓存模式为 none|V1|
|Windows Server 2012 或更高版本;Windows 8 或更高版本| Windows Server 2012 或更高版本| Windows Server 2008 R2 |V1|
|Windows Server 2012 或更高版本;Windows 8 或更高版本|Windows Server 2012 或更高版本|Windows Server 2012 或更高版本;对于分布式缓存模式为 none|V2|

如果你有运行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的内容服务器和托管缓存服务器，它们会根据 BranchCache 客户端的操作系统使用适当的内容信息版本，请求信息。 

当运行 Windows Server 2012 和 Windows 8 或更高版本操作系统的计算机请求内容时，内容和托管缓存服务器使用 V2 内容信息;当运行 Windows Server 2008 R2 和 Windows 7 请求内容的计算机时，内容和托管缓存服务器使用 V1 内容信息。

>[!IMPORTANT]
>当你在分布式缓存模式下部署 BranchCache 时，使用其他内容信息版本的客户端未彼此共享内容。 例如，运行 Windows 7 的客户端计算机以及运行安装在同一分支机构中的 Windows 10 的客户端计算机不会彼此共享内容。
  
## <a name="bkmk_handles"></a>BranchCache 如何处理文件中的内容更新

当分支机构用户修改或更新文档内容时，其更改将直接写入总部的内容服务器，而不会涉及 BranchCache。 无论用户是从内容服务器下载了该文档还是从分支机构中的托管缓存或分布缓存获取了该文档，都是如此。

当分支机构中的另一客户端对已修改文件发出请求时，将从总部服务器下载该文件的新增分段并将其添加到该分支中的分布缓存或托管缓存。 因此，分支机构用户总是会收到缓存内容的最新版本。

## <a name="BKMK_4"></a>BranchCache 安装指南

你可以使用 Windows Server 2016 中的服务器管理器来安装 BranchCache 功能或文件服务服务器角色的网络文件 BranchCache 角色服务。 你可使用下表确定安装角色服务还是功能。

|功能|计算机位置|安装此 BranchCache 元素|
|-----------------|---------------------|------------------------------------|
|内容服务器 @no__t 基于0BITS 的应用程序服务器 @ no__t-1|总部或云数据中心|BranchCache 功能|
|内容服务器 \(Web server @ no__t-1|总部或云数据中心|BranchCache 功能|
|内容服务器 @no__t-使用 SMB 协议 @ no__t-1 的0file 服务器|总部或云数据中心|文件服务服务器角色的网络文件 BranchCache 角色服务|
|托管缓存服务器|分支机构|启用托管缓存服务器模式的 BranchCache 功能|
|启用 BranchCache 的客户端计算机|分支机构|无需安装;只需在客户端上启用 BranchCache 和 BranchCache 模式 \(distributed 或托管 @ no__t-1|

若要安装角色服务或功能，打开“服务器管理器”，选择你希望为其启用 BranchCache 功能的计算机。 在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 这将打开“添加角色和功能”向导。 在你运行向导时，进行以下选择：

- 在向导页“选择安装类型”上，选择“基于角色或基于功能的安装”。

- 在向导页 "**选择服务器角色**" 中，如果要安装支持 BranchCache 的文件服务器，请展开 "**文件和存储服务**"、"**文件和 iSCSI 服务**"，然后选择 "**网络文件 BranchCache**"。  若要节省磁盘空间，还可以选择 "**重复数据删除**" 角色服务，然后继续完成向导的安装和完成。 如果你不想安装启用 BranchCache 的文件服务器，请不要使用网络文件的 BranchCache 角色服务安装文件和存储服务角色。

- 在向导页 "**选择功能**" 上，如果要安装不是文件服务器的内容服务器或要安装托管缓存服务器，请选择 " **BranchCache**"，然后继续完成向导的安装和完成。 如果你不想安装除了文件服务器以外的内容服务器或托管缓存服务器，请勿安装 BranchCache 功能。
  
## <a name="bkmk_os"></a>BranchCache 的操作系统版本

下面是支持不同类型 BranchCache 功能的操作系统列表。

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>支持 BranchCache 客户端计算机功能的操作系统

以下操作系统提供 BranchCache 支持后台智能传输服务（BITS）、超文本传输协议（HTTP）和服务器消息块（SMB）。

- Windows 10 企业版

- Windows 10 教育版

- Windows 8.1 企业版

- Windows 8 企业版

- Windows 7 企业版

- Windows 7 旗舰版

在以下操作系统中，BranchCache 不支持 HTTP 和 SMB 功能，但支持 BranchCache 位功能。

-   Windows 10 专业版，仅支持 BITS

-   Windows 8.1 Pro，仅支持 BITS

-   Windows 8 专业版，仅支持 BITS

-   Windows 7 专业版，仅支持 BITS

> [!NOTE]
> 默认情况下，BranchCache 在 Windows Server 2008 或 Windows Vista 操作系统中不可用。 但在这些操作系统中，如果下载并安装 Windows Management Framework 更新，BranchCache 功能仅适用于后台智能传输服务（BITS）协议。 有关详细信息和下载 Windows Management Framework，请参阅[Windows Management framework （Windows PowerShell 2.0、WinRM 2.0 和 BITS 4.0）](https://go.microsoft.com/fwlink/?LinkId=188677) https://go.microsoft.com/fwlink/?LinkId=188677 。
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>支持 BranchCache 内容服务器功能的操作系统

可以使用 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 系列的操作系统作为 BranchCache 内容服务器。

此外，Windows Server 2008 R2 系列操作系统可用作 BranchCache 内容服务器，但以下情况例外：

- 在带有 Hyper-v 的 Windows Server 2008 R2 Enterprise 的服务器核心安装中不支持 BranchCache。

- 在带有 Hyper-v 的 Windows Server 2008 R2 Datacenter 的服务器核心安装中不支持 BranchCache。

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>可实现 BranchCache 托管缓存服务器功能的操作系统

可以使用 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 系列操作系统作为 BranchCache 托管缓存服务器。

此外，以下 Windows Server 2008 R2 操作系统可用作 BranchCache 托管缓存服务器：

- Windows Server 2008 R2 企业版

- 带有 Hyper-v 的 Windows Server 2008 R2 Enterprise

- Windows Server 2008 R2 企业服务器核心安装

- 带有 Hyper-v 的 Windows Server 2008 R2 Enterprise Server Core 安装

- Windows Server 2008 R2 for Itanium-Based Systems

- Windows Server 2008 R2 数据中心版

- 带有 Hyper-v 的 Windows Server 2008 R2 Datacenter

- 带有 Hyper-v 的 Windows Server 2008 R2 Datacenter Server Core 安装

## <a name="bkmk_security"></a>BranchCache 安全

BranchCache 可实现设计安全方法，与你现有的网络安全体系结构无缝合作，无需其他设备或复杂的额外安全配置。
  
BranchCache 为非入侵性，不会更改任何 Windows 身份验证或授权过程。 完成部署 BranchCache 后，仍会使用域凭据执行身份验证，也不会更改使用访问控制列表 (ACL) 功能进行授权的方法。 另外，其他配置如 BranchCache 部署前的操作一样继续执行。

BranchCache 安全模型是以元数据的创建为基础，即采用一系列哈希的形式。 这些哈希也称为内容信息。

完成创建内容信息后，该内容信息用于 BranchCache 信息交换中，而不是实际数据中，并且该内容信息使用支持的协议（HTTP、HTTPS 以及 SMB）进行交换。

缓存数据以加密方式保存，并且不允许没有访问原始源内容权限的客户端访问。  必须在由原始内容源进行身份验证和授权后，客户端才能检索内容元数据，并且必须拥有内容元数据才能访问本地机构中的缓存。

### <a name="how-branchcache-generates-content-information"></a>BranchCache 如何生成内容信息

因为内容信息是从多个元素创建的，内容信息的值始终唯一。 这些元素包括：

- 派生出哈希的实际内容（例如网页或共享文件）。  

- 配置参数，例如哈希算法和块大小。 若要生成内容信息，内容服务器会将内容划分为多个分段，再将这些分段细分为块。 BranchCache 使用安全的加密哈希标识和验证每个块和分段，从而支持 SHA256 哈希算法。

- 服务器机密。 所有内容服务器都必须使用服务器机密（即任意长度的二进制值）配置。

> [!NOTE]
> 使用服务器机密可确保客户端计算机无法自行生成内容信息。 这就可在客户端可以访问以前版本但不能访问当前版本的情况中阻止恶意用户通过对启用 BranchCach 的客户端计算机采用暴力攻击破解各版本内容中的小变动。

### <a name="content-information-details"></a>内容信息细节

BranchCache 使用服务器机密作为密钥，从而派生一个特定于内容的哈希发送给授权客户端。 将哈希算法应用于组合服务器机密，且数据的哈希可生成此哈希。

此哈希称为分段机密。 BranchCache 使用分段机密确保通信安全。 此外，BranchCache 会创建一个块哈希列表（即哈希数据块列表）和数据的哈希（通过对块哈希列表执行哈希操作生成的）。

内容信息包括：

- 块哈希列表：

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- 数据的哈希 (HoD)：

    `HoD = Hash(BlockHashList)`

- 分段机密 (Kp)：

    `Kp = HMAC(Ks, HoD)`

BranchCache 使用对等端内容缓存协议和检索框架协议实现确保缓存安全和内容缓存间数据检索所需的过程。

此外，BranchCache 能以在处理和传输实际内容时用到的相同的安全度处理内容信息。

## <a name="bkmk_flow"></a>内容流和进程

内容信息和实际内容的流动划分为四个阶段：

1.  @no__t 0BranchCache 的进程：请求内容 @ no__t-0

2.  @no__t 0BranchCache 的进程：查找内容 @ no__t

3.  @no__t 0BranchCache 的进程：检索内容 @ no__t

4.  @no__t 0BranchCache 的进程：缓存内容 @ no__t

以下部分介绍这些阶段。

## <a name="BKMK_8"></a>BranchCache 进程：请求内容

在第一个阶段，分支机构中的客户端计算机请求来自远程位置（例如总部）中内容服务器的内容（例如文件或网页）。 内容服务器会验证客户端计算机是否经授权可以接收请求内容。 如果客户端计算机已获得授权，并且内容服务器和客户端均为 BranchCache @ no__t-0enabled，则内容服务器将生成内容信息。

随后内容服务器使用用于实际内容的相同协议将内容信息发送至客户端计算机。 

例如，如果客户端计算机通过 HTTP 请求了网页，内容服务器就会使用 HTTP 发送内容信息。 因此，内容的线路级安全保障和内容信息是相同的。

收到初始部分的内容信息（数据的哈希 + 分段机密）后，客户端计算机会执行以下操作：

- 使用分段机密 (Kp) 作为加密密钥 (Ke)。

- 从 HoD 和 Kp 生成分段 ID (HoHoDk)：

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

这一层次的主要威胁在于分段机密面临的风险，但是 BranchCache 会对内容数据块进行加密来保护分段机密。 BranchCache 通过使用内容块所在内容分段的分段机密派生的加密密钥来实现这一点。

此方法能确保没有服务器机密的实体无法发现数据块中的实际内容。 会使用和纯文本分段本身相同的安全度处理分段机密，因为对于给定分段的分段机密的知识可帮助实体从对等机获取分段再将其加密。 服务器机密的知识不会立即生成任何特殊纯文本，但可用于从密码文本派生特定类型的数据，然后就有可能使某些部分已知的数据面临暴力猜测攻击的风险。 因此服务器机密应保密。
  
## <a name="BKMK_9"></a>BranchCache 进程：查找内容

客户端计算机收到内容信息后，客户端会使用分段 ID 在本地分支机构缓存中找到请求的内容，无论缓存是在客户端计算机间分布还是位于托管缓存服务器上。

如果客户端计算机是为托管缓存模式配置的，就会使用托管缓存服务器的计算机名称进行配置并与服务器联系检索内容。

但如果客户端计算机是为分布式缓存模式配置的，内容就可能存储在分支机构多台计算机的多个缓存中。 客户端计算机必须发现内容的位置后才能检索内容。

如果它们是为分布式缓存模式配置的，客户端计算机会通过使用以 Web 服务动态发现（WS-Discovery）协议为基础的发现协议找到内容。 客户端会发送 WS-Discovery 多播探测消息，从而通过网络发现缓存内容。 探测消息包括分段 ID，它允许客户端检查请求内容是否与缓存中存储的内容匹配。 如果分段 ID 和本地缓存的内容匹配，接收初始探测消息的客户端会使用单播探测-匹配消息答复查询客户端。  

WS-Discovery 过程是否成功取决于执行发现的客户端是否拥有内容服务器提供的、关于其请求内容的正确内容信息。

请求内容阶段当中对数据的主要威胁就在于信息泄露，因为访问内容信息就暗示对内容进行授权访问。 为缓解这一风险，发现过程不会泄露内容信息，除了分段 ID，它不会泄露有关包含内容的纯文本分段的任何信息。

此外，相同网络子网上恶意用户运行的其他客户计算机可以查看到通过路由器的原始内容源的 BranchCache 发现流量。

如果没有在分支机构中发现请求的内容，客户端会通过 WAN 链接直接请求来自内容服务器的内容。

收到内容后，它就会被添加到客户端计算机或托管缓存服务器上的本地缓存中。 在这种情况下，内容信息会阻止客户端或托管缓存服务器将任何不匹配哈希的内容添加到本地缓存。 通过匹配哈希验证内容的过程可确保仅有效内容会添加到缓存中，这样可以保护本地缓存的完整性。

## <a name="BKMK_10"></a>BranchCache 进程：检索内容

客户端计算机在内容主机（可能是托管缓存服务器或分布式缓存模式客户端计算机）上找到所需内容后，客户端计算机会开始检索内容的过程。

首先，客户端计算机会向内容主机发出有关其需要第一个块的请求。 请求会包含标识所需内容的分段 ID 和块范围。 因为只会返回一个块，块范围就只会包含单独一个块。 （目前不支持多个块的请求。）客户端还会将请求存储在其本地“未完成请求列表”中。  

收到来自客户端的有效请求消息后，内容主机会检查内容主机的内容缓存中是否存在请求中指定的块。

如果内容主机拥有该内容块，则内容主机会发送一个包含分段 ID、块 ID、加密数据块以及用于对块进行加密的初始化向量的响应。

如果内容主机没有该内容块，内容主机就会发送一个空响应消息。 这可告知客户计算机内容主机没有所请求的块。 空响应消息包含所请求块的分段 ID 和块 ID 以及一个大小为零的数据块。

当客户端计算机收到来自内容主机的响应时，客户端会验证消息是否与其“未完成请求列表”中的请求消息对应。 （分段 ID 和块索引必须和未完成请求的相应内容匹配。）

如果此验证过程不成功，而且客户端计算机的“未完成请求列表”中没有相应的请求消息，客户端计算机会放弃该消息。

如果此验证过程成功，而且客户端计算机的“未完成请求列表”中拥有相应的请求消息，客户端计算机会解密这个块。 随后，客户端会对照内容消息获得的适当的块哈希（客户端最初从原始内容服务器获取的）验证被解密的块。

如果块验证成功，解密的块会存储在缓存中。

该过程会一直重复到客户端获得所有需要的块为止。

> [!NOTE]
> 如果完整的内容分段不存在于一台计算机上，检索协议就会从源的组合（一组分布式缓存模式客户端计算机、托管缓存服务器和总部中的原始内容服务器 - 如果分支机构缓存不包含完整内容的话）检索和组装内容。

在 BranchCache 发送内容信息或内容之前，数据会被加密。 BranchCache 会加密响应消息内的块。 在 Windows 7 中，BranchCache 使用的默认加密算法为 AES-128、加密密钥为 Ke，密钥大小为128位（由加密算法规定）。 

BranchCache 会生成一个适合于加密算法的初始化向量，并使用加密密钥来加密块。 随后 BranchCache 会将加密算法和初始化向量记录到消息中。 

服务器和客户端绝不会互相交换、共享或发送加密密钥。 客户端从托管源内容的内容服务器收到加密密钥。 然后，它会使用从服务器收到的加密算法和初始化向量解密此块。 没有其他显式身份验证或授权内置到下载协议中。

### <a name="security-threats"></a>安全威胁

这一层次的主要安全威胁包括：

- 篡改数据

  *向请求者提供数据的客户端篡改数据*。 BranchCache 安全模型使用哈希来确定客户端和服务器都没有更改数据。  

- 信息泄露：  

    *BranchCache 将加密内容发送给任何指定了相应分段 ID 的客户端*。 分段 ID 是公用的，所以任何客户端都可接收加密内容。 但是，如果恶意用户获得加密的内容，他们必须知道加密密钥才能解密内容。 上层协议执行身份验证，然后将内容信息提供给通过身份验证和经授权的客户端。 内容信息的安全性与赋予内容本身的安全性相当，BranchCache 绝不会泄露内容信息。  

    *攻击者探查线路以获取内容*。  BranchCache 通过使用 AES128（其中机密密钥为 Ke）加密客户端之间的所有传输，防止数据透过线路被查探。  从内容服务器下载的内容信息会得到和数据本身进行保护的方式完全一样的保护，因此如果完全没有使用 BranchCache，这些内容信息就会得到恰如其分的保护，免遭信息泄露。

-   拒绝服务：

    *客户端因大量数据请求而阻塞*。 BranchCache 协议会合并队列管理计数器和计时器以防客户端超载。

## <a name="BKMK_11"></a>BranchCache 进程：缓存内容

在位于分支机构内的分布式缓存模式客户端计算机和托管缓存服务器上，内容缓存会随着通过 WAN 链接检索内容不断积聚。

使用托管缓存模式配置客户端计算机时，它们会将内容添加到其自己的本地缓存中，也能为托管缓存服务器提供数据。 托管缓存协议为客户端提供一个将内容和分段可用性通知缓存服务器的机制。

若要将内容上载到托管缓存服务器，客户端会通知服务器它拥有一个可用的分段。 随后托管缓存服务器会检索与所提供分段相关联的所有内容信息，并在分段内下载自己实际需要的块。 此过程会一直重复到客户端没有分段可以提供给托管缓存服务器为止。

若要使用托管缓存协议更新托管缓存服务器，就必须满足以下要求：

- 客户端计算机需要拥有分段内的一组块，从而能将这些内容提供给托管缓存服务器。 客户端必须为所提供的分段提供内容信息；这包含分段 ID、分段数据的哈希、分段机密和分段内包含的所有块哈希的列表。

- 对于运行 Windows Server 2008 R2 的托管缓存服务器，需要托管缓存服务器证书和关联私钥，且分支机构中的客户端计算机必须信任颁发证书的证书颁发机构（CA）。 这允许客户端和服务器成功参与 HTTPS 服务器身份验证。

    > [!IMPORTANT]
    > 运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的托管缓存服务器不需要托管缓存服务器证书和关联的私钥。  

- 客户端计算机是使用托管缓存服务器的计算机名称和传输控制协议 (TCP) 端口号（托管缓存服务器依赖它侦听 BranchCache 流量）配置的。 托管缓存服务器的证书绑定到此端口。 如果托管缓存服务器是域成员计算机，托管缓存服务器的计算机名称可以是完全限定的域名 (FQDN)；或者，如果托管缓存服务器不是域成员，它可以是计算机的 NetBIOS 名称。

- 客户端计算机主动侦听传入的块请求。 它正在侦听的端口会作为提供消息的一部分从客户端传送到托管缓存服务器。 这就允许托管缓存服务器使用 BranchCache 协议连接客户端计算机，以便在分段内检索数据块。

- 在初始化后，托管缓存服务器将开始侦听传入的 HTTP 请求。

- 如果托管缓存服务器配置为需要客户端计算机身份验证，那么支持 HTTPS 身份验证就同时需要客户端和托管缓存服务器。

### <a name="hosted-cache-mode-cache-population"></a>托管缓存模式缓存人口

向分支机构中的托管缓存服务器的缓存添加内容的过程在客户端发送 INITIAL_OFFER_MESSAGE 时开始，其中包括段 ID。 INITIAL_OFFER_MESSAGE 请求中的分段 ID 用于从托管缓存服务器的块缓存中检索相应的分段数据哈希、块哈希列表和分段机密。 如果托管缓存服务器已经获取了特定分段的所有内容信息，对于 INITIAL_OFFER_MESSAGE 的响应将为 OK（确定），也就不会发出对下载块的请求。

如果托管缓存服务器没有获得所有与分段内块哈希相关联的已提供数据块，对于 INITIAL_OFFER_MESSAGE 的响应将为 INTERESTED（感兴趣）。 随后客户端会发送一个描述正在提供的单个分段的 SEGMENT_INFO_MESSAGE。 托管缓存服务器会使用一条 OK（确定）消息进行响应，并启动从提供信息的客户端计算机下载缺少块的过程。

分段数据哈希、块哈希列表和分段机密用于确保正在下载的内容免遭篡改或更改。 随后会将下载的块添加到托管缓存服务器的块缓存。

## <a name="bkmk_cache"></a>缓存安全性  
本节提供有关 BranchCache 如何确保客户端计算机和托管缓存服务器上缓存数据安全的信息。

### <a name="client-computer-cache-security"></a>客户端计算机缓存安全
BranchCache 中所存储数据面临的最大威胁就是篡改。 如果攻击者可以篡改缓存中存储的内容和内容信息，那么就有可能利用这一点尝试和启动对于使用 BranchCache 的计算机的攻击。 攻击者可通过插入恶意软件代替其他数据来启动攻击。 BranchCache 可通过使用内容信息内的块哈希验证所有内容来缓解这一威胁。 如果攻击者尝试篡改此数据，就会放弃此数据并替换为来自原始源的有效数据。

BranchCache 中所存储数据面临的第二个大威胁就是信息泄露。 在分布式缓存模式中，客户端只会缓存自己已经请求的内容，但是此数据是以纯文本形式存储的，所以可能有风险。 为了帮助将缓存访问仅限于 BranchCache 服务，本地缓存将由 ACL 中指定的文件系统权限保护。 

尽管 ACL 对于阻止未经授权的使用者访问缓存有效，但拥有管理权限的用户有可能通过手动更改 ACL 中指定的权限获得对缓存的访问权限。 BranchCache 不能防止管理员帐户恶意使用的情况。

存储在内容缓存中的数据没有加密，所以如果担心数据泄露，你可使用 BitLocker 或加密文件系统 (EFS) 等加密技术。 BranchCache 使用的本地缓存不会增加分支机构中计算机面临的信息泄露威胁；缓存只包含以未加密状态位于磁盘其他位置的文件副本。 

加密整个磁盘在难以确保客户端物理安全的环境中特别重要。 例如，加密整个磁盘有助于确保有可能从分支机构环境删除的移动计算机上敏感数据的安全。

### <a name="hosted-cache-server-cache-security"></a>托管缓存服务器缓存安全

在托管缓存模式中，托管缓存服务器安全性面临的最大威胁就是信息泄露。 BranchCache 在托管缓存环境中运作的方式和分布式缓存模式相似，其中的文件系统权限可以保护缓存数据。 差别就在于，托管缓存服务器存储分支机构内任何启用 BranchCache 的计算机所请求的所有内容，而不仅仅是单独一个客户端所请求的数据。 未经授权入侵缓存的后果可能就严重得多，因为面临风险的数据要多很多。  
  
在托管缓存服务器运行 Windows Server 2008 R2 的托管缓存环境中，如果分支机构中的任何客户端都可以通过 WAN 链接访问敏感数据，则建议使用 BitLocker 或 EFS 等加密技术。 也有必要阻止对托管缓存的物理访问，因为当攻击者获得物理访问权限时，磁盘加密只会在计算机关机时起效果。  如果计算机开机或处于睡眠模式，则磁盘加密提供的保护就微乎其微。

> [!NOTE]
> 默认情况下，运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的托管缓存服务器将加密缓存中的所有数据，因此不需要使用附加加密技术。

即使客户端是在托管缓存模式中配置的，它仍然会将数据本地缓存，你可能会想采取措施保护除托管缓存服务器上的缓存以外的本地数据。
