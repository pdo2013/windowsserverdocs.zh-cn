---
title: 部署分支缓存托管的缓存模式
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 326a1f1edfe6cb763a33ebfc8fd5abdd5b6aab3a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>部署分支缓存托管的缓存模式

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

Windows Server 2016 Core 网络指南提供说明规划和部署完全正常网络和新的 Active Directory 所需的核心组件&reg;新建森林中的域。

本指南介绍了如何通过提供的一个或多个与 Read\-Only 域控制器在客户端计算机正在运行 Windows 的分支机构型托管的缓存部署分支缓存说明版本 core 网络上&reg;10、 Windows 8.1 或 Windows 8 和加入域。

>[!IMPORTANT]
>如果你打算部署或已经部署运行的 Windows Server 2008 R2 分支缓存托管缓存服务器，则不得使用本指南。 本指南提供部署托管的缓存模式运行的 Windows Server 托管的缓存服务器指导&reg;2016，Windows Server 2012 R2 或 Windows Server 2012。

本指南包含以下部分。

- [使用本指南先决条件](#bkmk_pre)

- [有关此指南](#bkmk_about)

- [本指南不提供的内容](#bkmk_not)

- [技术概述](#bkmk_tech)

- [分支缓存托管缓存模式部署概述](2-Bc-Hcm-Deploy-Overview.md)

- [分支缓存托管缓存模式部署计划](3-Bc-Hcm-Plan.md)

- [分支缓存托管缓存模式部署](4-Bc-Hcm-Deployment.md)

- [更多资源](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>使用本指南先决条件

这是 Windows Server 2016 Core 网络指南助手指南。 若要使用本指南托管的缓存型部署分支缓存，你必须先执行以下操作。

- 通过使用 Core 网络指南，部署主要办公室中的核心网络，或者已技术提供安装，并且运行正常网络上的核心网络指南中。 这些技术包括 IP TCP\/v4，DHCP、 Active Directory 域服务 \(AD DS\)，并 DNS。

    > [!NOTE]
    > Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)适用于 Windows Server 2016 Technical 库。  

- 部署分支缓存正在运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 在你的主 office 或云数据中心中的内容服务器。 有关如何部署分支缓存内容服务器信息，请参阅[其他资源](11-Bc-Hcm-additional-resources.md)。

- 宽的区域网络 \(WAN\) 之间建立连接你的分支机构，你的主 office 和，如果适用，你云资源，通过使用虚拟专用网络 \(VPN\)、 DirectAccess 或其他连接方式。

- 部署在你的分支机构的客户端计算机运行下列的操作系统，支持后台智能传输服务 (BITS)、 Hyper 文本传输协议 (HTTP) 和服务器消息块 (SMB) 提供分支缓存之一。
    - Windows 10 企业版
    - Windows 10 教育版
    - Windows 8.1 企业版
    - Windows 8 企业版

>[!NOTE]
>在以下操作系统，分支缓存不支持 HTTP 和 SMB 功能，但分支缓存位功能支持。
>     - Windows 10 专业版，位仅支持
>     - Windows 8.1 专业版，位仅支持
>     - Windows 8 专业版，位仅支持

## <a name="bkmk_about"></a>有关此指南

本指南专，该网络和系统管理员已遵循部署 core 网络、 Windows Server 2016 Core 网络指南或 Windows Server 2012 Core 网络指南中的说明或对于之前已部署 Core 网络指南，包括 Active Directory 域服务 \(AD DS\)、 域名服务 \(DNS\)、 动态主机配置协议 \(DHCP\) 中包括的技术并 TCP\/IP v4。

建议你查看的每个此部署方案中采用的技术设计和部署的指南。 这些指南可以帮助你确定是否此部署方案提供服务和你的组织的网络所需的配置。

## <a name="bkmk_not"></a>本指南不提供的内容

本指南中不提供有关分支缓存，包括有关分支缓存模式和功能的信息的概念信息。  

本指南中不提供有关如何部署 WAN 连接或在你的分支机构，如 DHCP、 RODC 或 VPN 服务器其他技术的信息。

此外，此指南不提供部署托管的缓存服务器时，你应使用的硬件上指南。 也可以运行其他应用程序和服务托管的缓存服务器，但是你必须进行判断，根据工作负载、 硬件功能和 branch office 大小、 是否分支缓存托管缓存服务器计算机上安装特定，以及多少磁盘空间以用于缓存分配上。  
本指南中不提供有关配置运行的 Windows 7 的计算机的说明进行操作。 如果你有正在运行 Windows 7，在你的分支机构的客户端计算机，你必须将它们配置使用本指南中提供的客户端计算机正在运行 Windows 10、 Windows 8.1 和 Windows 8 的不同的过程。
  
此外，如果你有运行 Windows 7 的计算机，你必须由未经认证颁发的客户端计算机信任服务器证书配置托管的缓存服务器。 \ (如果所有客户端计算机运行的 Windows 10、 Windows 8.1 或 Windows 8，不需要将托管的缓存服务器配置为服务器证书。 \) 
> [!IMPORTANT]
> 如果你托管的缓存服务器运行的 Windows Server 2008 R2、 使用 Windows Server 2008 R2[分支缓存部署指南](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx)而不是型托管的缓存部署分支缓存本指南。 适用于运行版本的 Windows 从 Windows 7 到 Windows 10 中的所有分支缓存客户端该指南中所述的组策略设置。 使用本指南中的步骤，不能配置计算机正在运行 Windows Server 2008 R2。

## <a name="bkmk_tech"></a>技术概述

有关此助手指南，分支缓存是你需要安装和配置仅技术。 你必须运行 Windows PowerShell 分支缓存命令内容服务器，如 Web 以及文件服务器，不过你不需要更改或以其他方式内容服务器重新配置。 此外，你必须使用组策略，在你正在运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的广告 DS 的域控制器上配置客户端计算机。

### <a name="branchcache"></a>分支缓存

分支缓存是一种宽的区域网络 (WAN) 带宽优化技术，包含在某些版本的 Windows Server 2016 和 Windows 10 操作系统，以及 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012、 Windows 8、 Windows Server 2008 R2 和 Windows 7 的某些版本中。

用户访问远程服务器上的内容时，优化 WAN 带宽，分支缓存下载从你的主办公室客户端请求内容或托管云内容服务器和缓存分支 office 地点，内容允许其他客户端计算机以分支机构本地而不是通过 WAN 访问相同的内容。

托管的缓存型部署分支缓存时，你必须将配置客户端计算机在分支机构为托管的缓存模式客户端，，然后，你必须部署托管的缓存服务器中分支机构。 本指南演示如何将与 prehashed 和预先加载的内容，从你的 Web 服务器托管的缓存部署和文件 server\ 基于内容的服务器。

### <a name="group-policy"></a>组策略

在 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 的组策略是一个用于提供和适用于一组有针对性的用户和 Active Directory 环境中的计算机的一个或多个所需的配置策略设置的基础结构。 

此基础结构和组成的组策略引擎多个 client\ 侧扩展 \(CSEs\) 负责阅读目标客户端计算机上的策略设置。

在此情况下使用组策略用来配置域成员客户端计算机托管分支缓存缓存模式。

若要继续使用本指南，请参阅[分支缓存托管缓存模式部署概述](2-Bc-Hcm-Deploy-Overview.md)。
