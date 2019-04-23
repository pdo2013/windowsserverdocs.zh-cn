---
title: DNS 策略方案指南
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b3a1400a68e22fc4988c87c9222b66f718cf5fd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865638"
---
# <a name="dns-policy-scenario-guide"></a>DNS 策略方案指南

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本指南供 DNS、 网络和系统管理员使用。  
  
DNS 策略是一项新功能 Windows Server 中的 DNS&reg; 2016年。 可以使用本指南，了解如何使用 DNS 策略来控制 DNS server 如何处理基于在策略中定义的不同参数的名称解析查询。   
  
本指南包含 DNS 策略概述信息，以及特定的 DNS 策略方案，为您提供有关如何配置 DNS 服务器行为，以完成你的目标，包括主地理位置基于流量管理和辅助 DNS 服务器、 应用程序高可用性、 拆分式 DNS 和的详细信息。  
  
本指南包含下列各节。  
  
- [DNS 策略概述](DNS-Policies-Overview.md)  
- [使用基于 DNS 策略的地理位置和主服务器的流量管理](primary-geo-location.md)  
- [对于地理位置基于流量管理和主要-辅助部署使用 DNS 策略](primary-secondary-geo-location.md)  
- [使用 DNS 策略执行基于一天的时间智能 DNS 响应](dns-tod-intelligent.md)
- [基于 Azure 的一天的时间的 DNS 响应云应用程序服务器](dns-tod-azure-cloud-app-server.md)
- [使用针对拆分式 DNS 部署 DNS 策略](split-brain-DNS-deployment.md)
- [使用 Active Directory 中的拆分式 DNS 的 DNS 策略](dns-sb-with-ad.md)
- [在 DNS 查询上应用筛选器使用 DNS 策略](apply-filters-on-dns-queries.md)
- [使用 DNS 策略执行应用程序负载均衡](app-lb.md)
- [使用 DNS 策略执行应用程序负载平衡和地理位置感知](app-lb-geo.md)

