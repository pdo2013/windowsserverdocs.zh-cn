---
title: DNS 策略概述
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06086bbd7edc2fa489805eb5075062332e002ab4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policies-overview"></a>DNS 策略概述

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解 DNS 策略，这便是 Windows Server 2016 中的新增功能。 你可以使用 DNS 策略地理位置基于交通管理、 智能 DNS 响应基于一天时间，管理单个 DNS 服务器 split\ 脑部署中配置了在 DNS 查询等应用筛选器。 以下各项提供有关这些功能的详细信息。

-   **应用程序负载平衡。** 在部署的不同位置的应用的多个实例后，你可以使用 DNS 策略平衡不同的应用程序的情况下，动态分配的应用程序的交通加载之间的交通负载。

-   **Geo\ 位置基于交通管理。** 你可以使用 DNS 策略允许响应 DNS 客户端查询基于地理位置的客户端和资源的客户端尝试连接时，主要和辅助 DNS 服务器客户端提供最近资源的 IP 地址。 

-   **拆分脑 DNS。** Split\ 脑 DNS DNS 记录分为相同 DNS 服务器，在不同的区域范围内，DNS 客户端接收基于这些客户端是内部或外部的客户端的是否响应。 独立 DNS 服务器上，您可以配置 split\ 脑 DNS 集成的 Active Directory 区域或区域。

-   **筛选。** 您可以配置 DNS 策略创建查询均基于你提供的条件的筛选器。 在 DNS 策略查询筛选器允许你配置 DNS 服务器以自定义根据 DNS 查询和 DNS 客户端发送 DNS 查询的方式进行响应。 
-   **讨论。** 你可以使用 DNS 策略将恶意 DNS 客户端重定向到 non\ 存在而不是将它们定向到它们尝试访问的计算机的 IP 地址。

-   **基于重定向的一天的时间。** 你可以使用 DNS 策略使用的基于当天的时间 DNS 策略应用程序交通分散不同的应用程序的地理位置分布式实例。

## <a name="new-concepts"></a>新的概念  
创建上面列出的方案的支持策略，以便就需要能够识别记录区域的客户端上网络，其他元素间组中的组。 通过以下新 DNS 对象表示以下元素：  

- **客户网：**客户端子网对象表示从中查询提交到 DNS 服务器 IPv4 或 IPv6 子网。 你可以创建个子网以后定义根据使用哪些子网请求来自应用的策略。 例如，在拆分脑 DNS 方案，分辨率为的名称，如请求*www.microsoft.com*即可回答向客户端从内部子网，内部 IP 地址和客户端中外部子网到不同的 IP 地址。

- **递归范围：**递归范围是一组控制递归 DNS 服务器上的设置的唯一实例。 递归范围包含转发器的列表，并指定递归是否已启用。 有许多递归范围 DNS 服务器。 DNS 服务器递归策略允许你选择的一组查询递归范围。 如果 DNS 服务器不可权威某些查询、DNS 服务器递归策略允许您可以控制如何解决这些查询。 你可以指定使用以及是否使用递归哪些转发器。

- **区域范围：** DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可出现在多个范围，使用不同的 IP 地址。 此外，在区域范围级完成区域传输。 这意味着记录从主区域中的时区范围，将传输到同一区域辅助区域中的范围内。

## <a name="types-of-policy"></a>类型的策略

DNS 策略都除以级别，然后键入。 你可以使用查询分辨率策略定义处理查询的方式和时区传输策略定义区域传输发生的方式。 你可以将应用在服务器或时区级别每个策略类型。
  
### <a name="query-resolution-policies"></a>查询分辨率策略

你可以使用 DNS 查询分辨率策略若要指定 DNS 服务器由处理查询如何传入的分辨率。 每个 DNS 查询分辨率策略包含的以下元素：  
  
|字段|描述|可能值|  
|---------|---------------|-------------------|  
|**名称**|策略名称|-256 个字符<br />-可以包含有效的文件名任何字符|  
|**状态**|策略状态|启用（默认）<br />-已禁用|  
|**级别**|策略级别|服务器<br />-区域|  
|**正在处理订单**|服务器后查询按级别进行分类，并在应用中，查找为其查询符合条件，并将其查询应用的第一个策略|-数值<br />每个策略包含相同级别，并将值上的应用的唯一值|  
|**操作**|由 DNS 服务器执行的操作|-允许（默认为区域级别）<br />-拒绝（默认上服务器级别）<br />-忽略|  
|**条件**|策略条件（和/或）和条件，必须满足对于策略应用的列表|-条件运营商（/）<br />-列表中的条件（请参阅条件表所示）|  
|**范围**|区域范围和每个范围内的加权的值的列表。 为负载平衡 distribution 使用加权的值。 例如，如果该列表将包括 datacenter1 与 3 粗细，与 5 粗细 datacenter2 服务器将回复与记录 datacentre1 三次退出八个请求|-区域范围（按名称）和重量列表|  

> [!NOTE]
> 仅能值服务器级别策略**拒绝**或**忽略**作为一项操作。

由两个元素 DNS 策略条件字段：

|名称|描述|示例值|
|--------|---------------|-----------------|
|**客户网**|传输用于查询的协议。 可能条目**UDP**和**TCP**|-   **EQ、西班牙、法国**-解决了如果子网被标识为西班牙或法国<br />-   **新、加拿大、墨西哥**-为 true 加拿大和墨西哥之外的任何子网客户网是否解决了|  
|**传输协议**|传输用于查询的协议。 可能条目**UDP**和**TCP**|-   **EQ TCP**<br />-   **EQ UDP**|  
|**Internet 协议**|网络用于查询的协议。 可能条目**IPv4**和**IPv6**|-   **EQ IPv4**<br />-   **EQ IPv6**|  
|**服务器界面 IP 地址**|传入 DNS 服务器网络接口的 IP 地址|-   **EQ 10.0.0.1**<br />-   **EQ 192.168.1.1**|  
|**FQDN**|使用通配符的可能性的查询、中记录的 FQDN|-   **EQ，www.contoso.com** -解析 tot 真正仅 if 查询正在尝试解决*www.contoso.com* FQDN<br />-   **EQ,\*.contoso.com,\*.woodgrove.com** -解决了如果查询适用于任何以结束录制*contoso.com***或者***woodgrove.com*|  
|**查询类型**|录制正在查询 A、SVR（文本文件）的类型|-   **EQ，文本，SRV** -如果请求的查询文本，可解决 tot 真正**或者**SRV 记录<br />-   **EQ、MX** -解析 tot 真正如果 MX 记录请求的查询|  
|**一天时间**|收到查询的一天时间|-   **EQ 10:00 12:00，：00 22-23:00** -如果查询收到 10 上午和中午之间，可解决 tot 真正**或者**10 下午与 11 之间|  
  
作为起始点使用上的表下, 表可用于定义用于匹配的任何类型的记录，但 SRV 记录来自客户端 10.0.0.0 月 24 网中通过 TCP 之间 8 并通过界面 10.0.0.3 10 晚上 contoso.com 域中的查询条件：  
  
|名称|值|  
|--------|---------|  
|客户网|EQ，10.0.0.0 月 24|  
|传输协议|EQ TCP|  
|服务器界面 IP 地址|EQ 10.0.0.3|  
|FQDN|EQ，*。contoso.com|  
|查询类型|新 SRV|  
|一天时间|EQ，20:00 22:00|  
  
前提是他们有不同的值为处理订单，则可以创建多个查询分辨率的相同级别的策略。 多个策略可用时，则 DNS 服务器处理传入查询方式如下：  
  
![DNS 策略处理](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>递归策略  
递归策略是一种特殊**类型**的服务器级别策略。 递归策略控制 DNS 服务器执行递归查询的方式。 递归策略应用仅当查询处理达到递归路径。 你可以选择的一组查询递归拒绝或忽略值。 或者，你可以选择的一组查询转发器的一组。  
  
你可以使用递归策略实现 Split-brain DNS 配置。 在此配置，DNS 服务器执行递归对于查询的客户端的一组时 DNS 服务器执行递归该查询其他客户端。  
  
递归策略包含与包含常规 DNS 查询分辨率策略，以及的元素表所示：  
  
|名称|描述|  
|--------|---------------|  
|**应用对递归**|指定递归应仅使用此策略。|  
|**递归范围**|递归范围的名称。|  
  
> [!NOTE]  
> 仅可以在服务器级别创建递归策略。  
  
### <a name="zone-transfer-policies"></a>区域传输策略  
区域传输策略控制是否允许区域复制或不是由 DNS 服务器。 在服务器级别或区域级别，则可以创建区域传输的策略。 对 DNS 服务器出现了每个区域传输查询应用服务器级别策略。 仅在上一个 DNS 服务器上托管的区域查询应用区域级别策略。 最常用的区域级别策略是实现阻止或安全的列表。  
  
> [!NOTE]  
> 区域传输策略只能使用拒绝或忽略作为操作。  
  
下面的服务器级别区域传输策略可用于从给定子网拒绝区域传输 contoso.com 域：  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfCOnsotostoFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
可以创建多个区域传输策略相同级别，前提是他们有不同的值为处理订单。 多个策略可用时，则 DNS 服务器处理传入查询方式如下：  
  
![多个区域传输策略 DNS 过程](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>管理 DNS 策略  
你可以创建并管理通过使用 PowerShell DNS 策略。 通过不同的示例情况下，你可以通过 DNS 策略配置转到下面的示例：  
  
### <a name="traffic-management"></a>路况管理  
你可以直接交通根据 FQDN 到不同的服务器，具体取决于 DNS 客户端的位置。 下面的示例显示了如何创建管理策略以直接从北美 datacenter 某些网和欧洲 datacenter 网另一个客户的交通。  
  
```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  
  
脚本前两个行创建客户端子网北美和欧洲的对象。 之后的两条直线创建内 contoso.com 域，一个用于每个区域的时区范围。 之后的两条线将关联到另一个 IP 地址、欧洲的一个、北美的另一个 ww.contoso.com 每个区域中创建的记录。 最后，最后行脚本创建两个 DNS 查询分辨率策略，另一台用于到北美子网，欧洲子网到另一个应用。  
  
### <a name="block-queries-for-a-domain"></a>对于域阻止查询  
到某个域，你可以使用阻止查询到 DNS 查询分辨率策略。 下面的示例阻止所有查询到 treyresearch.net:  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>从子网阻止查询  
你还可以阻止来自特定子网查询。 下面的脚本创建子网 # 月 172.0.33.0 月 24 日，，然后创建忽略所有查询来自该子网策略：  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>允许递归内部客户端  
你可以通过使用 DNS 查询分辨率策略控制递归。 用于禁用它的外部拆分脑方案客户端时内部客户端，启用递归下面的示例。  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
第一行脚本中的更改默认递归范围内，只需命名为"。"（点）禁用递归。 第二行创建的名为递归范围*InternalClients*启用了递归。 第三行创建一个策略以将应用和新创建经过具有 IP 地址作为 10.0.0.34 服务器接口任何查询到递归范围。  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>创建服务器级别区域传输策略  
通过使用 DNS 区域传输策略，你可以控制更精确表单中的时区传输。 以允许任何服务器上给定子网区域传输，可以使用下面的示例脚本：  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
第一行脚本中的创建的名为子网对象*AllowedSubnet* IP 与阻止 172.21.33.0 月 24。 第二行创建区域传输策略允许子网先前已创建的区域传输到任何 DNS 服务器。  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>创建区域级别区域传输策略  
你也可以创建区域级别区域传输策略。 下面的示例忽略的 contoso.com 来自有一个 IP 地址的 10.0.0.33 服务器界面区域传输任何请求：  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>DNS 策略方案

有关如何使用信息 DNS 策略特定的情况下，请参阅本指南中的以下主题。

- [地理位置的使用 DNS 策略基于交通主服务器管理](primary-geo-location.md)  
- [地理位置的使用 DNS 策略基于交通管理与主要辅助部署](primary-secondary-geo-location.md)  
- [使用基于当天的时间的智能 DNS 响应 DNS 策略](dns-tod-intelligent.md)
- [基于时间的 Azure 的一天中的 DNS 响应云应用服务器](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 策略 Split-Brain DNS 部署](split-brain-DNS-deployment.md)
- [使用 DNS 策略 Split-Brain Active Directory 中的 DNS](dns-sb-with-ad.md)
- [用于 DNS 策略 DNS 查询的问题的应用筛选器](apply-filters-on-dns-queries.md)
- [使用应用程序负载平衡 DNS 策略](app-lb.md)
- [使用应用程序使用负载平衡地理位置感知 DNS 策略](app-lb-geo.md)


