---
title: 配置为非域成员允许分支缓存交通防火墙规则
description: 本主题介绍 Windows Server 2016，其中演示如何在分布式托管的缓存模式优化分支机构中 WAN 带宽使用量部署分支缓存分支缓存部署指南中
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5f744141efc35bb493bcd95fad53eafbbc3f78d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>配置为非域成员允许分支缓存交通防火墙规则

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题中的信息，配置第三方防火墙产品并手动允许分支缓存以分布式的缓存模式运行的防火墙规则配置客户端计算机。  
  
> [!NOTE]  
> -   如果你已经将配置分支缓存客户端计算机使用组策略的组策略设置覆盖策略应用于客户端计算机任何手动配置。  
> -   如果你已经部署分支缓存与 DirectAccess，可以本主题中使用设置配置 IPsec 规则，以允许分支缓存流量。  
  
在会员**管理员**，或等效的最低要求进行配置更改。  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS PCCRD]: 对等内容缓存和检索发现协议  
分布式的缓存客户必须允许传入和传出 MS PCCRD 通信，执行采用 Web 服务动态发现 （WS 发现） 协议。  
  
防火墙设置必须允许除了传入和传出通信多址广播的通信。 可以使用以下设置配置防火墙例外，以便分布式的缓存模式。  
  
IPv4 多址广播： 239.255.255.250  
  
IPv6 多址广播： FF02::C  
  
归巢的交通： 本地端口： 3702，远程的端口： 短暂  
  
站通信： 本地端口： 短暂的偏远的端口： 3702  
  
计划： %systemroot%\system32\svchost.exe （分支缓存服务 [PeerDistSvc]）  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS PCCRR]: 对等内容缓存和检索： 检索协议  
分布式的缓存客户必须允许传入和传出 MS PCCRR 通信，评论 (RFC) 2616年请求中所述中 HTTP 1.1 协议, 携带。  
  
防火墙设置必须允许传入和传出通信。 可以使用以下设置配置防火墙例外，以便分布式的缓存模式。  
  
归巢的交通： 本地端口： 80，远程的端口： 短暂  
  
站通信： 本地端口： 短暂的偏远的端口： 80  
  


