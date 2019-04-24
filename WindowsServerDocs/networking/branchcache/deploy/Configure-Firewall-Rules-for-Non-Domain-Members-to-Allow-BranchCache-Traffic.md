---
title: 配置适用于非域成员的防火墙规则，以允许 BranchCache 流量
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 288865f0237969e0bed7e105f8d539759984275e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834768"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>配置适用于非域成员的防火墙规则，以允许 BranchCache 流量

>适用于：Windows 服务器 （半年频道），Windows Server 2016

配置第三方防火墙产品并手动配置防火墙规则允许 BranchCache 在分布式的缓存模式下运行的客户端计算机，您可以使用本主题中的信息。  
  
> [!NOTE]  
> -   如果已配置 BranchCache 客户端计算机使用组策略，组策略设置将覆盖客户端计算机将策略应用到的任何手动配置。  
> -   如果已部署 BranchCache 使用 DirectAccess，可以在本主题中使用设置来配置 IPsec 规则以允许 BranchCache 流量。  
  
中的成员身份**管理员**，或等效身份是进行这些配置更改所需的最低。  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]:对等内容缓存并检索发现协议  
分布式的缓存客户端必须允许入站和出站 MS PCCRD 流量，Web 服务动态发现 (Ws-discovery) 协议中携带。  
  
防火墙设置必须允许多播的流量除了入站和出站流量。 可以使用以下设置配置为分布式的缓存模式的防火墙例外。  
  
IPv4 多播：239.255.255.250  
  
IPv6 多路广播：FF02::C  
  
入站流量：本地端口：3702，远程端口： 临时  
  
出站流量：本地端口： 临时的远程端口：3702  
  
安装程序： %systemroot%\system32\svchost.exe （BranchCache 服务 [PeerDistSvc]）  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]:对等内容缓存和检索：检索协议  
分布式的缓存客户端必须允许入站和出站 MS PCCRR 流量，如征求意见文档 (RFC) 2616年中所述执行 HTTP 1.1 协议中。  
  
防火墙设置必须允许入站和出站流量。 可以使用以下设置配置为分布式的缓存模式的防火墙例外。  
  
入站流量：本地端口：80，远程端口： 临时  
  
出站流量：本地端口： 临时的远程端口：80  
  


