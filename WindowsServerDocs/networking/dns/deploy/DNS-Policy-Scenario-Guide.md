---
title: DNS 策略方案指南
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fab127fcd524526a515752871c1a3db92047c46
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406228"
---
# <a name="dns-policy-scenario-guide"></a>DNS 策略方案指南

>适用于：Windows Server（半年频道）、Windows Server 2016

本指南旨在供 DNS、网络和系统管理员使用。  
  
DNS 策略是 Windows Server @ no__t-0 2016 中 DNS 的新增功能。 您可以使用本指南了解如何使用 DNS 策略来控制 DNS 服务器如何根据您在策略中定义的不同参数处理名称解析查询。   
  
本指南包含 DNS 策略概述信息，以及特定 DNS 策略方案，这些方案为你提供了有关如何配置 DNS 服务器行为以实现你的目标的说明，包括适用于主要和的基于地理位置的流量管理辅助 DNS 服务器、应用程序高可用性、裂脑 DNS 等。  
  
本指南包含下列各节。  
  
- [DNS 策略概述](DNS-Policies-Overview.md)  
- [将 DNS 策略用于基于地理位置的流量管理和主服务器](primary-geo-location.md)  
- [将 DNS 策略用于基于地理位置的流量管理和主要辅助部署](primary-secondary-geo-location.md)  
- [根据一天中的时间将 DNS 策略用于智能 DNS 响应](dns-tod-intelligent.md)
- [基于当天使用 Azure 云应用服务器的时间的 DNS 响应](dns-tod-azure-cloud-app-server.md)
- [将 DNS 策略用于裂脑 DNS 部署](split-brain-DNS-deployment.md)
- [在 Active Directory 中对裂脑 DNS 使用 DNS 策略](dns-sb-with-ad.md)
- [使用 DNS 策略对 DNS 查询应用筛选器](apply-filters-on-dns-queries.md)
- [使用 DNS 策略实现应用程序负载平衡](app-lb.md)
- [将 DNS 策略用于应用程序负载平衡和地理位置识别](app-lb-geo.md)

