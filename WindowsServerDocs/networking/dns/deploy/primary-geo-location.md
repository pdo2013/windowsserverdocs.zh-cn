---
title: 使用针对基于地理位置的流量管理和主服务器的 DNS 策略
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 110014adf1e23be574f23efc01e8a4d69397e547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831978"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>使用针对基于地理位置的流量管理和主服务器的 DNS 策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题，了解如何配置 DNS 策略以允许主 DNS 服务器响应 DNS 客户端查询客户端并向其客户端尝试连接，该资源的地理位置上基于客户端提供 IP address 的最近的资源。  
  
>[!IMPORTANT]  
>该方案演示了如何使用只有主 DNS 服务器时部署基于地理位置的流量管理的 DNS 策略。 已为主要和辅助 DNS 服务器时，还可以完成基于地理位置的流量管理。 如果您的主要和辅助部署，先完成本主题中的步骤，然后完成主题中提供的步骤[使用 DNS 策略的地理位置基于流量管理和主要-辅助部署](primary-secondary-geo-location.md).

使用新的 DNS 策略，可以创建允许以响应客户端查询来查找 Web 服务器的 IP 地址的 DNS 服务器的 DNS 策略。 Web 服务器的实例可能位于不同物理位置的不同数据中心。 DNS 可以评估客户端和 Web 服务器位置，然后通过提供客户端与 Web 服务器的 IP 地址以物理方式靠近客户端的 Web 服务器为响应客户端请求。  

可以使用以下 DNS 策略参数来控制 DNS 服务器查询响应来自 DNS 客户端。 

- **客户端子网**。 预定义的客户端子网的名称。 用于验证从其发送查询的子网。
- **传输协议**。 传输协议在查询中使用。 可能的项都**UDP**并**TCP**。
- **Internet 协议**。 在查询中使用的网络协议。 可能的项都**IPv4**并**IPv6**。
- **服务器接口 IP 地址**。 收到 DNS 请求的 DNS 服务器的网络接口的 IP 地址。
- **FQDN**。 完全限定域 (FQDN) 在查询中，记录的因为它使用通配符。
- **查询类型**。 正在查询的记录 （A、 SRV、 TXT 等） 的类型。
- **一天中的时间**。 接收到查询的时间。 
  
使用逻辑运算符 （和/或） 制定策略表达式，可以结合以下条件。 当两个表达式匹配时，策略需要执行以下操作之一。
 
- **忽略**。 DNS 服务器以无提示方式会删除该查询。          
- **拒绝**。 DNS 服务器做出响应失败响应与该查询。          
- **允许**。 DNS 服务器返回使用流量管理响应进行响应。          
  
##  <a name="bkmk_example"></a>基于地理位置的流量管理示例

下面是举例说明如何使用 DNS 策略以实现基于客户端执行 DNS 查询的物理位置的流量重定向。   
  
此示例使用两个虚构的公司 Contoso 云服务，从而提供 web 应用和托管解决方案; 的域和 Woodgrove 食物服务，它在全球范围内提供多个城市中的食物传送服务，并且具有网站上名为 woodgrove.com。  
  
Contoso 云服务有两个数据中心，一个在美国，欧洲的另一个。 在欧洲数据中心承载餐 woodgrove.com 的门户。   
  
若要确保 woodgrove.com 客户从他们的网站获取一种响应体验，Woodgrove 想欧洲客户端定向到在欧洲数据中心和美国的客户端定向到美国数据中心。 可以将位于世界其他位置的客户定向到在数据中心之一。   
  
下图描绘了此方案。  
  
![基于地理位置的流量管理示例](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>如何的 DNS 名称解析过程工作原理  
  
在名称解析过程中，用户尝试连接到 www.woodgrove.com。 这会导致 DNS 名称解析请求发送到用户的计算机上的网络连接属性中配置的 DNS 服务器。 通常情况下，这是充当缓存解析程序，本地 ISP 提供的 DNS 服务器，并被称为 LDNS。   
  
如果不存在 LDNS 在本地缓存中的 DNS 名称，LDNS 服务器将转发到 woodgrove.com 具有权威的 DNS 服务器查询。 权威 DNS 服务器响应与请求的记录 (www.woodgrove.com) LDNS 服务器，又将其发送给用户的计算机之前在本地缓存的记录。  
  
由于 Contoso 云服务使用 DNS 服务器策略，承载 contoso.com 的权威 DNS 服务器配置来返回基于地理位置托管的流量响应。 这会导致欧洲客户端的方向欧洲数据中心与美国的客户端的方向到美国数据中心，如下图中所示。  
  
在此方案中，权威 DNS 服务器通常会看到来自 LDNS 服务器而，很少，来自用户的计算机的名称解析请求。 因此，看到的权威 DNS 服务器的名称解析请求中的源 IP 地址是计算机的 LDNS 服务器而不是计算机的用户。 但是，基于，使用 LDNS 服务器的 IP 地址配置的地理位置时响应提供了在用户的地理位置的合理估计的查询因为用户进行查询其本地 ISP 的 DNS 服务器。  
  
>[!NOTE]  
>DNS 策略使用 UDP/TCP 数据包包含 DNS 查询中的发件人 IP。 如果查询达到通过多个冲突解决程序/LDNS 跃点在主服务器，策略将考虑仅从其 DNS 服务器收到查询的最后一个冲突解决程序的 IP。  
  
##  <a name="bkmk_config"></a>如何为基于地理位置的查询响应中配置 DNS 策略  
若要配置基于地理位置的查询响应的 DNS 策略，必须执行以下步骤。  
  
1. [创建 DNS 客户端子网](#bkmk_subnets)  
2. [创建区域的作用域](#bkmk_scopes)  
3. [将记录添加到区域作用域](#bkmk_records)  
4. [创建策略](#bkmk_policies)  
  
>[!NOTE]  
>你想要配置的区域具有权威的 DNS 服务器上，必须执行这些步骤。 中的成员身份**DnsAdmins**，或等效身份所需执行以下过程。  
  
以下部分提供详细的配置说明。  
  
>[!IMPORTANT]  
>以下部分包含示例 Windows PowerShell 命令包含多个参数的示例值。 请确保将这些命令中的示例值替换之前运行这些命令适用于你的部署的值为。  
  
### <a name="bkmk_subnets"></a>创建 DNS 客户端子网  
  
第一步是确定的子网或 IP 地址空间为你想要将流量重定向的区域。 例如，如果你想要将流量重定向针对美国和欧洲，需要确定的子网或这些区域的 IP 地址空间。  
  
可以从地理 IP 映射获取此信息。 根据这些地理 IP 发行版，必须创建的"DNS 客户端子网。" DNS 客户端子网是从中查询发送到 DNS 服务器的 IPv4 或 IPv6 子网的逻辑分组。  
  
可以使用以下 Windows PowerShell 命令创建 DNS 客户端子网。  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
有关详细信息，请参阅[添加 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="bkmk_scopes"></a>创建区域作用域  
配置客户端子网后，必须分区区域你想要重定向到两个不同的区域作用域，其流量的每个已配置了 DNS 客户端子网一个作用域。   
  
例如，如果你想要将 DNS 名称 www.woodgrove.com 的流量重定向，你必须创建两个不同的区域作用域中 woodgrove.com 区域、 美国和欧洲。  
  
区域作用域是该区域的唯一实例。 DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址或相同的 IP 地址。  
  
>[!NOTE]  
>默认情况下，区域作用域的 DNS 区域上存在。 此区域作用域为该区域具有相同的名称和旧的 DNS 操作适用于此作用域。   
  
可以使用以下 Windows PowerShell 命令以创建区域作用域。  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="bkmk_records"></a>将记录添加到区域作用域  
现在，必须添加到了两个区域范围表示 web 服务器主机的记录。   
  
例如， **USZoneScope**并**EuropeZoneScope**。 在 USZoneScope，可以添加的 IP 地址 192.0.0.1，它位于美国数据中心; 记录 www.woodgrove.com并在 EuropeZoneScope 可以在欧洲数据中心内的 IP 地址 141.1.0.1 添加同一条记录 (www.woodgrove.com)。   
  
可以使用以下 Windows PowerShell 命令将记录添加到区域作用域。  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
在此示例中，您必须使用以下 Windows PowerShell 命令将记录添加到默认区域作用域，以确保世界上的其余部分仍可以从两个数据中心之一访问 woodgrove.com web 服务器。  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
**区域范围区域**默认作用域中添加一条记录时不包括参数。 这是与将记录添加到标准 DNS 区域相同。  
  
有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
### <a name="bkmk_policies"></a>创建策略  
创建子网后，分区 （区域作用域），并且您已将记录添加，则必须创建连接的子网和分区的策略，因此，如果查询来自 DNS 客户端子网之一中的源，从返回的查询响应区域的正确作用域。 没有策略所需的映射的默认区域作用域。   
  
可以使用以下 Windows PowerShell 命令创建 DNS 客户端子网链接和区域作用域的 DNS 策略。   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在已具有所需的 DNS 策略基于地理位置的流量重定向配置 DNS 服务器。  
  
当 DNS 服务器收到名称解析查询时，DNS 服务器的计算结果中针对配置的 DNS 策略对 DNS 请求的字段。 如果名称解析请求中的源 IP 地址与匹配任何策略，关联的区域作用域用于响应查询，并将用户定向到地理上接近它们的资源。   
  
您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。  
  
  
