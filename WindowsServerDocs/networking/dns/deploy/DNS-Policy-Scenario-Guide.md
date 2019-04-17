---
title: DNS 策略方案指南
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7eab7aa403271992e9bc38f20ca0ddfa52bdd9cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policy-scenario-guide"></a>DNS 策略方案指南

>适用于：Windows Server（半年通道），Windows Server 2016

本指南供 DNS、网络和系统管理员使用。  
  
DNS 策略是一个新功能，在 Windows Server DNS&reg; 2016 年。 你可以使用本指南若要了解如何使用 DNS 策略控制 DNS 服务器处理基于策略的定义的其他参数名称分辨率查询的方式。   
  
本指南包含 DNS 策略概述的信息，以及为你提供有关如何配置 DNS 服务器行为的说明进行操作，以实现你目标，包括主服务器和辅助 DNS 服务器、应用程序高可用性、裂 DNS，并为的地理位置基于交通管理的特定 DNS 策略方案。  
  
本指南包含以下部分。  
  
- [DNS 策略概述](DNS-Policies-Overview.md)  
- [地理位置的使用 DNS 策略基于交通主服务器管理](primary-geo-location.md)  
- [地理位置的使用 DNS 策略基于交通管理与主要辅助部署](primary-secondary-geo-location.md)  
- [使用基于当天的时间的智能 DNS 响应 DNS 策略](dns-tod-intelligent.md)
- [基于时间的 Azure 的一天中的 DNS 响应云应用服务器](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 策略 Split-Brain DNS 部署](split-brain-DNS-deployment.md)
- [使用 DNS 策略 Split-Brain Active Directory 中的 DNS](dns-sb-with-ad.md)
- [用于 DNS 策略 DNS 查询的问题的应用筛选器](apply-filters-on-dns-queries.md)
- [使用应用程序负载平衡 DNS 策略](app-lb.md)
- [使用应用程序使用负载平衡地理位置感知 DNS 策略](app-lb-geo.md)

