---
title: 配置适用于非域成员的防火墙规则，以允许 BranchCache 流量
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a96b67b235b813ad455d5b289b7238f671e4c547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356705"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>配置适用于非域成员的防火墙规则，以允许 BranchCache 流量

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题中的信息来配置第三方防火墙产品，并手动配置客户端计算机，使其具有允许 BranchCache 在分布式缓存模式下运行的防火墙规则。  
  
> [!NOTE]  
> -   如果已使用组策略配置 BranchCache 客户端计算机，则组策略设置将覆盖应用策略的客户端计算机的任何手动配置。  
> -   如果已使用 DirectAccess 部署 BranchCache，则可以使用本主题中的设置来配置 IPsec 规则以允许 BranchCache 流量。  
  
**Administrators**中的成员身份或同等身份是进行这些配置更改所需的最低要求。  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[PCCRD]：对等内容缓存和检索发现协议  
分布式缓存客户端必须允许入站和出站 PCCRD 流量，该流量以 Web 服务动态发现（WS 发现）协议传输。  
  
除了入站流量和出站流量外，防火墙设置还必须允许多播流量。 你可以使用以下设置来配置分布式缓存模式的防火墙例外。  
  
IPv4 多播：239.255.255.250  
  
IPv6 多播：FF02：： C  
  
入站流量：本地端口：3702，远程端口：暂时  
  
出站流量：本地端口：临时、远程端口：3702  
  
Program：%systemroot%\system32\svchost.exe （BranchCache 服务 [PeerDistSvc]）  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[PCCRR]：对等内容缓存和检索：检索协议  
分布式缓存客户端必须允许入站和出站 PCCRR 流量，如请求注释（RFC） 2616 1.1 中所述。  
  
防火墙设置必须允许入站和出站流量。 你可以使用以下设置来配置分布式缓存模式的防火墙例外。  
  
入站流量：本地端口：80，远程端口：暂时  
  
出站流量：本地端口：临时、远程端口：80  
  


