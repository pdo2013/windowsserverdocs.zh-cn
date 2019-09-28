---
title: 使用针对基于地理位置的流量管理和主服务器的 DNS 策略
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9c313b88e2502a99baf5962a1f2eb224d67a38dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406180"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>使用针对基于地理位置的流量管理和主服务器的 DNS 策略

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解如何配置 DNS 策略，以允许主 DNS 服务器基于客户端尝试连接到的客户端和资源的地理位置来响应 DNS 客户端查询，并为客户端提供 IP ad最接近的资源的装饰。  
  
>[!IMPORTANT]  
>此方案说明了在仅使用主 DNS 服务器时，如何为基于地理位置的流量管理部署 DNS 策略。 如果同时拥有主 DNS 服务器和辅助 DNS 服务器，则还可以完成基于地理位置的流量管理。 如果你有主要辅助部署，请先完成本主题中的步骤，然后完成本主题中的步骤：[将 DNS 策略用于基于地理位置的流量管理和主要辅助部署](primary-secondary-geo-location.md)。

使用新的 DNS 策略，你可以创建 DNS 策略，以允许 DNS 服务器响应客户端查询，从而请求 Web 服务器的 IP 地址。 Web 服务器的实例可能位于不同物理位置的不同数据中心。 DNS 可以评估客户端和 Web 服务器位置，并通过为客户端提供 Web 服务器的 Web 服务器 IP 地址来响应客户端请求，该地址与客户端位于同一位置。  

你可以使用以下 DNS 策略参数来控制 DNS 服务器对来自 DNS 客户端的查询的响应。 

- **客户端子网**。 预定义的客户端子网的名称。 用于验证从中发送查询的子网。
- **传输协议**。 查询中使用的传输协议。 可能的条目为**UDP**和**TCP**。
- **Internet 协议**。 查询中使用的网络协议。 可能的条目为**IPv4**和**IPv6**。
- **服务器接口 IP 地址**。 接收 DNS 请求的 DNS 服务器的网络接口的 IP 地址。
- **FQDN**。 查询中记录的完全限定的域名（FQDN），可能使用通配符。
- **查询类型**。 查询的记录类型（A、SRV、TXT 等）。
- **当天的时间**。 接收查询的时间。 
  
可以结合使用以下条件和逻辑运算符（和/或）来表述策略表达式。 如果这些表达式匹配，则策略应执行下列操作之一。
 
- **忽略**。 DNS 服务器会悄悄地删除查询。          
- **拒绝**。 DNS 服务器使用失败响应来响应查询。          
- **允许**。 DNS 服务器通过流量管理的响应回复。          
  
##  <a name="bkmk_example"></a>基于地理位置的流量管理示例

下面的示例演示了如何使用 DNS 策略来基于执行 DNS 查询的客户端的物理位置实现流量重定向。   
  
此示例使用两个虚构的公司： Contoso 云服务，提供 web 和域托管解决方案;和 Woodgrove 食物服务，在全球多个城市提供食物交付服务，其中有一个名为 woodgrove.com 的网站。  
  
Contoso 云服务有两个数据中心，一个在美国，另一个在欧洲。 欧洲数据中心托管了 woodgrove.com 的食品定购门户。   
  
为了确保 woodgrove.com 客户获得其网站的响应体验，Woodgrove 希望将欧洲的客户定向到将欧洲数据中心和美国的客户定向到美国数据中心。 位于世界各地的客户可以定向到其中一个数据中心。   
  
下图描述了此方案。  
  
![基于地理位置的流量管理示例](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>DNS 名称解析过程的工作方式  
  
在名称解析过程中，用户尝试连接到 www.woodgrove.com。 这会生成一个 DNS 名称解析请求，该请求将发送到在用户计算机上的网络连接属性中配置的 DNS 服务器。 通常，这是作为缓存解析程序的本地 ISP 提供的 DNS 服务器，被称为 LDNS。   
  
如果 DNS 名称未出现在 LDNS 的本地缓存中，则 LDNS 服务器会将该查询转发到对 woodgrove.com 具有权威的 DNS 服务器。 权威 DNS 服务器会将请求的记录（www.woodgrove.com）与 LDNS 服务器进行响应，后者反过来将记录缓存到用户的计算机上，然后再将其发送到用户的计算机。  
  
由于 Contoso 云服务使用 DNS 服务器策略，因此将托管 contoso.com 的权威 DNS 服务器配置为返回基于地理位置的流量管理的响应。 如图中所示，这会导致欧洲的客户端与欧洲数据中心的客户和美国的数据中心的方向。  
  
在这种情况下，权威 DNS 服务器通常会看到来自 LDNS 服务器的名称解析请求，很少从用户的计算机中查看。 因此，权威 DNS 服务器显示的名称解析请求中的源 IP 地址是 LDNS 服务器而不是用户计算机的 IP 地址。 但是，如果在配置基于地理位置的查询响应时使用 LDNS 服务器的 IP 地址，则可以合理估计用户的地理位置，因为用户正在查询其本地 ISP 的 DNS 服务器。  
  
>[!NOTE]  
>DNS 策略利用 UDP/TCP 数据包中的发件人 IP，其中包含 DNS 查询。 如果查询通过多个解析程序/LDNS 跃点到达主服务器，则该策略将仅考虑来自 DNS 服务器从中接收查询的最后一个解析程序的 IP。  
  
##  <a name="bkmk_config"></a>如何为基于地理位置的查询响应配置 DNS 策略  
若要为基于地理位置的查询响应配置 DNS 策略，必须执行以下步骤。  
  
1. [创建 DNS 客户端子网](#bkmk_subnets)  
2. [创建区域的作用域](#bkmk_scopes)  
3. [将记录添加到区域作用域](#bkmk_records)  
4. [创建策略](#bkmk_policies)  
  
>[!NOTE]  
>您必须在对要配置的区域具有权威的 DNS 服务器上执行这些步骤。 若要执行以下过程，需要**DnsAdmins**中的成员身份或同等身份。  
  
以下各节提供了详细的配置说明。  
  
>[!IMPORTANT]  
>以下各节包含示例 Windows PowerShell 命令，其中包含许多参数的示例值。 在运行这些命令之前，请确保将这些命令中的示例值替换为适用于你的部署的值。  
  
### <a name="bkmk_subnets"></a>创建 DNS 客户端子网  
  
第一步是确定要将流量重定向到的区域的子网或 IP 地址空间。 例如，如果要重定向美国和欧洲的流量，则需要确定这些区域的子网或 IP 地址空间。  
  
你可以从地理 IP 映射获取此信息。 根据这些地域 IP 分发，必须创建 "DNS 客户端子网"。 DNS 客户端子网是将查询发送到 DNS 服务器的 IPv4 或 IPv6 子网的逻辑分组。  
  
你可以使用以下 Windows PowerShell 命令来创建 DNS 客户端子网。  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
有关详细信息，请参阅[DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="bkmk_scopes"></a>创建区域作用域  
配置了客户端子网之后，必须将要重定向到的流量分区为两个不同的区域作用域，其中每个已配置的 DNS 客户端子网的作用域。   
  
例如，如果要重定向 DNS 名称 www.woodgrove.com 的流量，则必须在 woodgrove.com 区域中创建两个不同的区域作用域，一个用于美国，一个用于欧洲。  
  
区域作用域是区域的唯一实例。 DNS 区域可以有多个区域作用域，每个区域作用域包含其自己的一组 DNS 记录。 同一条记录可以出现在多个作用域中，具有不同的 IP 地址或相同的 IP 地址。  
  
>[!NOTE]  
>默认情况下，在 DNS 区域中存在区域作用域。 此区域作用域具有与区域相同的名称，并且此作用域上的传统 DNS 操作工作正常。   
  
你可以使用以下 Windows PowerShell 命令创建区域作用域。  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

有关详细信息，请参阅[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="bkmk_records"></a>将记录添加到区域作用域  
现在，必须将表示 web 服务器主机的记录添加到这两个区域作用域中。   
  
例如， **USZoneScope**和**EuropeZoneScope**。 在 USZoneScope 中，可以使用 IP 地址192.0.0.1 （位于美国数据中心）添加记录 www.woodgrove.com;在 EuropeZoneScope 中，可以在欧洲数据中心添加同一记录（www.woodgrove.com）和 IP 地址141.1.0.1。   
  
你可以使用以下 Windows PowerShell 命令将记录添加到区域作用域。  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
在此示例中，还必须使用以下 Windows PowerShell 命令将记录添加到默认区域作用域中，以确保世界各地的其他人仍可从两个数据中心访问 woodgrove.com web 服务器。  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
在默认范围内添加记录时，不包含**ZoneScope**参数。 这与将记录添加到标准 DNS 区域相同。  
  
有关详细信息，请参阅[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
### <a name="bkmk_policies"></a>创建策略  
创建子网、分区（区域作用域）并添加记录后，你必须创建用于连接子网和分区的策略，以便在查询来自某个 DNS 客户端子网中的源时，从区域的正确范围。 映射默认区域作用域不需要策略。   
  
你可以使用以下 Windows PowerShell 命令创建一个 DNS 策略，用于链接 DNS 客户端子网和区域作用域。   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在 DNS 服务器配置了所需的 DNS 策略，以根据地理位置重定向流量。  
  
当 DNS 服务器收到名称解析查询时，DNS 服务器会根据配置的 DNS 策略评估 DNS 请求中的字段。 如果名称解析请求中的源 IP 地址与任何策略相匹配，则会使用关联的区域作用域来响应查询，并将用户定向到离它们最近的资源。   
  
你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。  
  
  
