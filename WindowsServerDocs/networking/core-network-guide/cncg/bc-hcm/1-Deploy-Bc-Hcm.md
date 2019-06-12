---
title: 部署 BranchCache 托管缓存模式
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 54991b343623b934118bb62af1bd91871a726996
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446480"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>部署 BranchCache 托管缓存模式

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Windows Server 2016 核心网络指南提供有关规划和部署完全正常运行的网络和新的 Active Directory 所需的核心组件说明&reg;在新林中的域。

本指南介绍如何构建在核心网络上通过提供与读取的一个或多个分支机构中的托管的缓存模式部署 BranchCache 的说明\-只有客户端计算机运行 Windows 的域控制器&reg;10、 Windows 8.1 或 Windows 8 中，并将其联接到域。

>[!IMPORTANT]
>如果你打算部署或已经部署运行 Windows Server 2008 R2 的 BranchCache 托管缓存服务器，则不要使用本指南。 本指南提供了有关部署托管的缓存模式与运行 Windows Server 的托管的缓存服务器的说明&reg;2016 中，Windows Server 2012 R2 或 Windows Server 2012。

本指南包含下列各节。

- [使用本指南的先决条件](#bkmk_pre)

- [关于本指南](#bkmk_about)

- [本指南未提供的内容](#bkmk_not)

- [技术概述](#bkmk_tech)

- [BranchCache 托管缓存模式部署概述](2-Bc-Hcm-Deploy-Overview.md)

- [BranchCache 托管缓存模式部署规划](3-Bc-Hcm-Plan.md)

- [BranchCache 托管缓存模式部署](4-Bc-Hcm-Deployment.md)

- [其他资源](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>使用本指南的先决条件

这是 Windows Server 2016 核心网络指南的助理指南。 若要参照本指南部署 BranchCache 托管缓存模式，必须先执行以下操作。

- 参照核心网络指南在总部部署核心网络，或者已经在网络上安装并正常运行核心网络指南中提供的技术。 这些技术包括 TCP\/IP v4，DHCP，Active Directory 域服务\(AD DS\)，和 DNS。

    > [!NOTE]
    > Windows Server 2016[核心网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)现已推出 Windows Server 2016 技术库。  

- 部署 BranchCache 内容服务器正在运行的 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 在总部或云数据中心中。 有关如何部署 BranchCache 内容服务器的信息，请参阅[其他资源](11-Bc-Hcm-additional-resources.md)。

- 建立广域网\(WAN\)在分支机构、 总部之间的连接并在适当时您的云资源，通过使用虚拟专用网络\(VPN\)，DirectAccess，或其他连接方法。

- 部署分支机构中运行下列操作系统中的 BranchCache 提供支持后台智能传输服务 (BITS)、 超文本传输协议 (HTTP) 和服务器消息块 (SMB) 之一的客户端计算机.
    - Windows 10 企业版
    - Windows 10 教育版
    - Windows 8.1 企业版
    - Windows 8 企业版

> [!NOTE]
> 在以下操作系统中，BranchCache 不支持 HTTP 和 SMB 功能，但支持 BranchCache BITS 功能。
>     - Windows 10 专业版，BITS 仅支持
>     - Windows 8.1 Pro BITS 仅支持
>     - Windows 8 专业版，BITS 仅支持

## <a name="bkmk_about"></a>关于本指南

本指南适用于遵循在 Windows Server 2016 核心网络指南或 Windows Server 2012 核心网络指南中的说明部署核心网络的网络和系统管理员或对于那些以前部署过核心网络指南，包括 Active Directory 域服务中包含的技术\(AD DS\)，域名服务\(DNS\)，动态主机配置协议\(DHCP\)，和 TCP\/IP v4。

建议查看此部署方案中所用的每项技术的设计和部署指南。 这些指南可帮助你确定此部署方案是否为组织网络提供了所需的服务和配置。

## <a name="bkmk_not"></a>本指南未提供的内容

本指南不提供有关 BranchCache 的概念信息（包括有关 BranchCache 模式和功能的信息）。  

本指南不提供有关如何在分支机构部署 WAN 连接或其他技术（如 DHCP、RODC 或 VPN 服务器）的信息。

此外，本指南也不针对部署托管缓存服务器时应使用的硬件提供指导。 可以在托管缓存服务器上运行其他服务和应用程序，但是必须根据以下因素来做决定：工作负荷，硬件功能，分支机构规模，是否在特定计算机上安装 BranchCache 托管缓存服务器，以及要为缓存分配多少磁盘空间。  
本指南不提供有关配置运行 Windows 7 的计算机的说明。 如果您的分支机构中运行 Windows 7 的客户端计算机，则必须配置它们使用不同于本指南中提供的运行 Windows 10、 Windows 8.1 和 Windows 8 客户端计算机的过程。
  
此外，如果您的计算机运行 Windows 7，您必须由客户端计算机信任的证书颁发机构颁发的服务器证书配置托管的缓存服务器。 \(如果所有客户端计算机都运行 Windows 10、 Windows 8.1 或 Windows 8，您不需要使用服务器证书配置托管的缓存服务器。\) 
> [!IMPORTANT]
> 如果你的托管的缓存服务器运行 Windows Server 2008 R2，使用 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx)而不是本指南，以在托管的缓存模式下部署 BranchCache。 将应用到所有 BranchCache 运行的客户端的 Windows 版本从 Windows 7 到 Windows 10 的该指南中所述的组策略设置。 不能通过使用本指南中的步骤配置运行 Windows Server 2008 R2 的计算机。

## <a name="bkmk_tech"></a>技术概述

在本助理指南中，唯一需要安装和配置的技术就是 BranchCache。 你必须在内容服务器（如 Web 服务器和文件服务器）上运行 Windows PowerShell BranchCache 命令，但无需以其他任何方式更改或重新配置内容服务器。 此外，必须在 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 运行 AD DS 的域控制器上使用组策略配置客户端计算机。

### <a name="branchcache"></a>BranchCache

BranchCache 是某些版本的 Windows Server 2016 和 Windows 10 操作系统，以及在某些版本的 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012、 Windows 8 中所包含宽的区域网络 (WAN) 带宽优化技术Windows Server 2008 R2 和 Windows 7。

当用户访问远程服务器上的内容时优化 WAN 带宽，BranchCache 从总部下载客户端请求内容或托管的云内容服务器并将内容缓存在分支机构位置，使其他客户端计算机若要本地而不是通过 WAN 访问相同的内容的分支机构。

部署 BranchCache 托管缓存模式时，必须在分支机构将客户端计算机配置为托管缓存模式客户端，然后必须在分支机构部署托管缓存服务器。 本指南演示如何部署托管的缓存服务器与 Web 服务和文件服务器从 prehashed 和预加载内容\-基于内容的服务器。

### <a name="group-policy"></a>组策略

Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中的组策略是用来交付和应用到一组目标的用户和计算机在 Active Directory 环境中的一个或多个所需的配置或策略设置的基础结构。 

此基础结构组策略引擎和多个客户端组成\-侧扩展\(Cse\)负责读取目标客户端计算机上的策略设置。

本方案使用组策略将域成员客户端计算机配置为 BranchCache 托管缓存模式。

若要继续使用本指南，请参阅[BranchCache 托管缓存模式部署概述](2-Bc-Hcm-Deploy-Overview.md)。
