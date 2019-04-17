---
title: 地理位置的使用 DNS 策略基于交通主服务器管理
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd72e13cd3d2d7f3ca886afcdcf97e824ef227f5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>地理位置的使用 DNS 策略基于交通主服务器管理

>适用于：Windows Server（半年通道），Windows Server 2016

可以使用本主题以了解如何配置 DNS 策略允许主 DNS 服务器响应 DNS 客户端查询基于地理位置的客户端和资源的客户端尝试连接时，客户端提供最近资源的 IP 地址。  
  
>[!IMPORTANT]  
>此项 scenario 说明了如何部署基于地理位置交通管理 DNS 策略，当你使用的仅主 DNS 服务器。 当你有主要和辅助 DNS 服务器，还可以完成地理位置基于交通管理。 如果你有主要辅助部署，首先完成本主题中的步骤，然后完成提供主题中的步骤[使用与主要辅助部署的地理位置基于交通管理 DNS 策略](primary-secondary-geo-location.md)。

使用新 DNS 策略，则可以创建 DNS 策略允许客户端查询要求 IP 地址的 Web 服务器对反应 DNS 服务器。 Web server 实例可能位于不同的物理位置不同数据中心。 DNS 可以评估客户端和 Web 服务器的位置，然后通过向客户端提供 Web 服务器的 IP 地址的物理位置更接近于客户端的 Web 服务器响应客户端请求。  

你可以使用以下 DNS 策略参数控制 DNS 客户端查询到 DNS 服务器响应。 

- **客户端子网**。 预定义的客户端子网名称。 用于验证查询已从其发送的子网。
- **传输协议**。 传输用于查询的协议。 可能条目**UDP**和**TCP**。
- **Internet 协议**。 网络用于查询的协议。 可能条目**IPv4**和**IPv6**。
- **服务器界面 IP 地址**。 这收到 DNS 请求 DNS 服务器的网络接口的 IP 地址。
- **FQDN**。 完全合格域 (FQDN) 中查询，记录使用通配符的可能性。
- **查询类型**。 录制正在查询（A、SRV、文本等）的类型。
- **一天时间**。 一天中收到查询的时间。 
  
你可以与逻辑运营商（和/或）以阐明策略表情组合以下条件。 当这些表情匹配时，应策略执行以下操作之一。
 
- **忽略**。 DNS 服务器悄悄下降查询。          
- **拒绝**。 DNS 服务器的响应该查询与失败响应。          
- **允许**。 重新交通管理响应响应 DNS 服务器。          
  
##  <a name="bkmk_example"></a>地理位置基于交通管理示例

以下是一种方式使用 DNS 策略实现交通重定向基于执行 DNS 查询的客户端的物理位置。   
  
此示例中使用两个虚构的公司 Contoso 云服务，该法案 web 和域举办解决方案;和 Woodgrove 食物服务，其中提供美食传递城市的多个全球和服务方面的优化调整网站命名 woodgrove.com。  
  
Contoso 云服务都有一个在美国，另一个欧洲两个数据中心。 欧洲 datacenter 承载餐 portal 用于进行 woodgrove.com。   
  
若要确保 woodgrove.com 客户能够响应体验从其网站，Woodgrove 想定向到欧洲 datacenter 欧洲的客户端和定向到美国 datacenter 的美国客户端。 在世界位于其他位置的客户可以将定向到任一数据中心。   
  
下图显示了这种情况。  
  
![地理位置基于交通管理示例](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>如何 DNS 名称分辨率进程的工作原理  
  
在名称分辨率过程中，用户将尝试连接到 www.woodgrove.com。这将导致 DNS 名称分辨率请求发送给用户的计算机上的网络连接属性中配置 DNS 服务器。 通常，这是提供的本地缓存的解析程序，作为 ISP DNS 服务器，并称为 LDNS。   
  
如果不存在于 LDNS 的本地缓存 DNS 名称，LDNS 服务器转发对 DNS 服务器的 woodgrove.com 授权查询。授权 DNS 服务器与该请求记录 (www.woodgrove.com) 响应 LDNS 服务器，反过来发送给用户的计算机之前本地缓存记录。  
  
Contoso 云服务使用 DNS 服务器策略，因为该主机 contoso.com 配置为返回地理位置授权 DNS 服务器基于交通管理响应。 这将导致欧洲的客户端方向欧洲 datacenter 和的方向美国客户端到美国数据中心中，如图所示。  
  
这种情况下，在授权 DNS 服务器通常可以看到即将从 LDNS 服务器和，很少，用户的计算机名称分辨率请求。 出于此原因，在名称分辨率请求中所看到的授权 DNS 服务器源 IP 地址是 LDNS 服务器的并且不该用户的计算机。 因为用户在查询他本地 ISP 的 DNS 服务器，使用 LDNS 服务器的 IP 地址，当你在配置地理位置基于但是，查询响应提供安公平用户来说，该地理位置。  
  
>[!NOTE]  
>DNS 策略利用发件人 IP UDP TCP/数据包包含 DNS 查询中。 如果查询达到了多个解析程序/LDNS 跃点到主服务器，策略将考虑仅从中 DNS 服务器接收查询的最后一个解析程序 IP。  
  
##  <a name="bkmk_config"></a>如何配置为的地理位置基于查询响应 DNS 策略  
若要配置为的地理位置基于查询响应 DNS 策略，你必须执行以下步骤。  
  
1. [创建 DNS 客户端子网](#bkmk_subnets)  
2. [创建该区域的范围](#bkmk_scopes)  
3. [添加到区域范围记录](#bkmk_records)  
4. [创建策略](#bkmk_policies)  
  
>[!NOTE]  
>你必须在你想要配置区域的授权 DNS 服务器执行这些步骤。 在会员**DnsAdmins**，或等效，需要执行以下步骤。  
  
以下部分提供了详细的配置说明进行操作。  
  
>[!IMPORTANT]  
>以下部分包括包含许多参数示例值的示例 Windows PowerShell 命令。 确保你在运行以下命令之前就提供适用于你的部署的值与替换示例值在这些命令。  
  
### <a name="bkmk_subnets"></a>创建 DNS 客户端子网  
  
第一步是找出子网或 IP 地址的地区，你希望将通信重定向的空间。 例如，如果你想要将通信美国和欧洲重定向，你需要确定子网或 IP 地址空间的这些区域。  
  
你可以从地理 IP 地图获取此信息。 基于这些地理 IP 分配，你必须创建"DNS 客户端个子网。" DNS 客户端子网是 IPv4 或 IPv6 个子网从中查询发送到 DNS 服务器的逻辑分组。  
  
以下 Windows PowerShell 命令用于创建 DNS 客户端个子网。  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
有关详细信息，请参阅[添加 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="bkmk_scopes"></a>创建区域范围  
将配置客户端子网后，你必须分区区域你想要将重定向到了两种不同的区域范围，其流量一个范围有配置 DNS 客户端个子网。   
  
例如，如果你想要将重定向 DNS 名称 www.woodgrove.com 的通信，您必须在 woodgrove.com 区域、美国和欧洲创建两个不同的区域范围。  
  
区域范围是唯一区域的实例。 DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可能存在多个范围，使用不同的 IP 地址或同一 IP 地址。  
  
>[!NOTE]  
>默认情况下，区域范围存在上 DNS 区域。 此区域范围作为区域具有相同的名称，并传统 DNS 运营处理此范围。   
  
可以使用下面的 Windows PowerShell 命令来创建区域范围。  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="bkmk_records"></a>添加到区域范围记录  
现在，你必须添加到了两个区域范围表示 web 服务器主机的记录。   
  
例如，**USZoneScope**和**EuropeZoneScope**。 在 USZoneScope，你可以添加记录 www.woodgrove.com 192.0.0.1，居住在美国 datacenter; 的 IP 地址并在 EuropeZoneScope 可以与欧盟数据中心中的 IP 地址 141.1.0.1 添加同一记录 (www.woodgrove.com)。   
  
你可以使用 Windows PowerShell 以下命令添加到区域范围记录。  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
在此示例中，您必须使用 Windows PowerShell 以下命令添加到默认的区域范围内，以确保在全世界其他人仍可以访问 woodgrove.com 任一两个数据中心中的 web 服务器记录。  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
**区域范围区域**参数时将不包括在的默认范围添加记录。 这就是向标准 DNS 区域中添加记录相同。  
  
有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
### <a name="bkmk_policies"></a>创建策略  
创建子网后，分区（区域范围），并且你已添加的记录，你必须创建子网和分区中，连接的策略，以便当查询来源之一 DNS 客户端子网，查询响应从正确的范围内的区域。 没有策略所需的映射默认区域范围。   
  
以下 Windows PowerShell 命令可用于创建 DNS 策略，该链接 DNS 客户端子网和区域范围。   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在将重定向基于地理位置通信所需的 DNS 策略配置 DNS 服务器。  
  
当 DNS 服务器收到名称分辨率查询时，DNS 服务器评估配置 DNS 策略防御 DNS 请求中的字段。 名称分辨率请求中的源 IP 地址与匹配任何策略，如果关联的区域范围用于响应查询，并且用户定向到地理位置与他们最近的资源。   
  
你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。  
  
  
