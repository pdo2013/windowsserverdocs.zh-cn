---
title: 部署 BranchCache 托管缓存模式
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49e74132dba2909b7e5b639c95ef50064cf23e8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356377"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>部署 BranchCache 托管缓存模式

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

Windows Server 2016 Core 网络指南提供了有关在新林中规划和部署完全正常运行的网络和新的 Active Directory @ no__t-0 域所需的核心组件的说明。

本指南说明如何在核心网络上进行构建，方法是提供在一个或多个分支机构的托管缓存模式下部署 BranchCache 的说明，其中的客户端计算机运行的是 Windows @ no__t-1 10 Windows 8。1、或 Windows 8，并且加入到域。

>[!IMPORTANT]
>如果你计划部署或已部署运行 Windows Server 2008 R2 的 BranchCache 托管缓存服务器，请不要使用本指南。 本指南提供有关使用运行 Windows Server @ no__t-0 2016、Windows Server 2012 R2 或 Windows Server 2012 的托管缓存服务器部署托管缓存模式的说明。

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

- 参照核心网络指南在总部部署核心网络，或者已经在网络上安装并正常运行核心网络指南中提供的技术。 这些技术包括 TCP @ no__t-0IP v4、DHCP、Active Directory 域服务 \(AD DS @ no__t-2 和 DNS。

    > [!NOTE]
    > Windows server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)在 windows Server 2016 技术库中提供。  

- 在总公司或云数据中心部署运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的 BranchCache 内容服务器。 有关如何部署 BranchCache 内容服务器的信息，请参阅[其他资源](11-Bc-Hcm-additional-resources.md)。

- 使用虚拟专用网络 \(VPN @ no__t、DirectAccess 或其他连接方法，在分支机构、总公司和你的云资源（如果适用）之间建立广域网络 \(WAN @ no__t 连接。

- 在分支机构中部署运行以下操作系统之一的客户端计算机，该操作系统为 BranchCache 提供对后台智能传输服务（BITS）、超文本传输协议（HTTP）和服务器消息块（SMB）的支持.
    - Windows 10 企业版
    - Windows 10 教育版
    - Windows 8.1 企业版
    - Windows 8 企业版

> [!NOTE]
> 在以下操作系统中，BranchCache 不支持 HTTP 和 SMB 功能，但支持 BranchCache 位功能。
>     - Windows 10 专业版，仅支持 BITS
>     - Windows 8.1 Pro，仅支持 BITS
>     - Windows 8 专业版，仅支持 BITS

## <a name="bkmk_about"></a>关于本指南

本指南适用于按照 Windows Server 2016 核心网络指南或 Windows Server 2012 核心网络指南中的说明部署核心网络的网络和系统管理员，或以前部署了核心网络指南中包括的技术，包括 Active Directory 域服务 \(AD DS @ no__t，域名服务 \(DNS @ no__t-3、动态主机配置协议 \(DHCP @ no__t-5 和 TCP @ no__t-6IP v4。

建议查看此部署方案中所用的每项技术的设计和部署指南。 这些指南可帮助你确定此部署方案是否为组织网络提供了所需的服务和配置。

## <a name="bkmk_not"></a>本指南未提供的内容

本指南不提供有关 BranchCache 的概念信息（包括有关 BranchCache 模式和功能的信息）。  

本指南不提供有关如何在分支机构部署 WAN 连接或其他技术（如 DHCP、RODC 或 VPN 服务器）的信息。

此外，本指南也不针对部署托管缓存服务器时应使用的硬件提供指导。 可以在托管缓存服务器上运行其他服务和应用程序，但是必须根据以下因素来做决定：工作负荷，硬件功能，分支机构规模，是否在特定计算机上安装 BranchCache 托管缓存服务器，以及要为缓存分配多少磁盘空间。  
本指南不提供有关配置运行 Windows 7 的计算机的说明。 如果你的客户端计算机运行的是分支机构中的 Windows 7，则必须使用与本指南中提供的步骤不同的过程对运行 Windows 10、Windows 8.1 和 Windows 8 的客户端计算机进行配置。
  
此外，如果你的计算机运行的是 Windows 7，则必须使用由客户端计算机信任的证书颁发机构颁发的服务器证书来配置托管缓存服务器。 @no__t 0If 所有客户端计算机都在运行 Windows 10、Windows 8.1 或 Windows 8，则不需要使用服务器证书配置托管缓存服务器。 \) 
> [!IMPORTANT]
> 如果托管缓存服务器正在运行 Windows Server 2008 R2，请使用 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx)，而不是本指南，以便在托管缓存模式下部署 BranchCache。 将该指南中所述的组策略设置应用于运行 windows 7 到 Windows 10 的 Windows 版本的所有 BranchCache 客户端。 无法使用本指南中的步骤配置运行 Windows Server 2008 R2 的计算机。

## <a name="bkmk_tech"></a>技术概述

在本助理指南中，唯一需要安装和配置的技术就是 BranchCache。 你必须在内容服务器（如 Web 服务器和文件服务器）上运行 Windows PowerShell BranchCache 命令，但无需以其他任何方式更改或重新配置内容服务器。 此外，还必须使用在 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 上运行 AD DS 的域控制器上的组策略来配置客户端计算机。

### <a name="branchcache"></a>BranchCache

BranchCache 是一种广域网络（WAN）带宽优化技术，包含在某些版本的 Windows Server 2016 和 Windows 10 操作系统以及某些版本的 Windows server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8 中。、Windows Server 2008 R2 和 Windows 7。

为了在用户访问远程服务器上的内容时优化 WAN 带宽，BranchCache 从你的总部或托管的云内容服务器下载客户端请求的内容，并将内容缓存在分支机构位置，允许其他客户端计算机分支机构在本地而不是通过 WAN 访问相同内容。

部署 BranchCache 托管缓存模式时，必须在分支机构将客户端计算机配置为托管缓存模式客户端，然后必须在分支机构部署托管缓存服务器。 本指南演示如何从 Web 和文件服务器 @ no__t-0based 内容服务器部署包含 prehashed 和预加载内容的托管缓存服务器。

### <a name="group-policy"></a>组策略

Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的组策略是一种基础结构，用于向 Active Directory 环境中的一组目标用户和计算机交付和应用一个或多个所需的配置或策略设置。 

此基础结构包含一个组策略引擎和多个客户端 @ no__t-0side 扩展 \(CSEs @ no__t-2，负责在目标客户端计算机上读取策略设置。

本方案使用组策略将域成员客户端计算机配置为 BranchCache 托管缓存模式。

若要继续学习本指南，请参阅[BranchCache 托管缓存模式部署概述](2-Bc-Hcm-Deploy-Overview.md)。
