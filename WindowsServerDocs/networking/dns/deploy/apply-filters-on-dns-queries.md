---
title: 用于 DNS 策略 DNS 查询的问题的应用筛选器
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff10af5f88a03f806a5e5b5fa698c7c3637816bd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>用于 DNS 策略 DNS 查询的问题的应用筛选器

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何在 Windows Server 配置 DNS 策略&reg;2016 创建查询均基于你提供的条件的筛选器。 

在 DNS 策略查询筛选器允许你配置 DNS 服务器以自定义根据 DNS 查询和 DNS 客户端发送 DNS 查询的方式进行响应。

例如，你可以将 DNS 策略配置为与查询筛选阻止列表以阻止 DNS 查询来自已知恶意域，这会阻止来自这些域中查询到响应 DNS。 由于从 DNS 服务器发送无响应，该恶意域成员 DNS 查询超时。

另一个示例是要创建查询筛选器可使指定的一组的客户端解析某些名称的允许列表。

## <a name="bkmk_criteria"></a>查询筛选
以下条件可以创建查询 filters 使用任何逻辑组合 （和/或/不）。

|名称|描述|
|-----------------|---------------------|
|客户网|预定义的客户端子网名称。 用于验证查询已从其发送的子网。|
|传输协议|传输用于查询的协议。 可能的值为 UDP 和 TCP。|
|Internet 协议|网络用于查询的协议。 可能的值为 IPv4 和 IPv6。|
|服务器界面 IP 地址|收到 DNS 请求 DNS 服务器的网络接口 IP 地址|
|FQDN|完全限定域名查询中, 记录的可能性使用通配符。|
|查询类型|录制正在查询的类型 \ (A、 SRV、 文本等。 \)|
|一天时间|一天中收到查询的时间。|

下面的示例显示了如何创建阻止以下 DNS 策略的筛选器或允许分辨率 dns 名称。

>[!NOTE]
>本主题中的示例命令使用 Windows PowerShell 命令**添加 DnsServerQueryResolutionPolicy**。 有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。 

##<a name="bkmk_block1"></a>从域阻止查询

在某些情况下，你可能想要阻止 DNS 命名分辨率域，你已确定为恶意或不符合你的组织使用量原则的域。 你可以通过使用 DNS 策略完成域阻止查询。

不在任何特定的区域创建你在此示例配置的策略，改为你创建服务器级别策略应用于所有配置 DNS 服务器上的区域。 服务器级别策略是第一个受到评估，因此首先要匹配的查询时收到 DNS 服务器。

下面的示例命令配置服务器级别策略阻止任何与域查询**后缀 contosomalicious.com**。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>当您将配置**操作**参数使用值**忽略**，DNS 服务器配置根本下降查询无响应。 这将导致超时恶意域中的 DNS 客户端。

##<a name="bkmk_block2"></a>从子网阻止查询
如果它找到某些恶意软件感染，并尝试联系恶意网站的使用 DNS 服务器，与此示例中，您可以阻止从子网查询。 

添加 DnsServerClientSubnet-命名"MaliciousSubnet06"-IPv4Subnet 层通过 172.0.33.0/24

添加 DnsServerQueryResolutionPolicy-命名"BlockListPolicyMalicious06"-操作忽略 ClientSubnet"EQ，MaliciousSubnet06"-层通过

下面的示例演示如何结合 FQDN 条件中使用子网条件阻止某些恶意域从受感染子网查询。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>阻止一种查询
你可能需要阻止某些类型的查询服务器上的名称分辨率。 例如，你还可以阻止 '任何' 查询，这可用于恶意创建放大攻击。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>允许仅从某个域中查询
不能仅使用 DNS 阻止查询到的策略，可用于自动批准查询来自特定域或个子网。 当您将配置允许列表时，DNS 服务器仅同时阻止所有其他查询来自其他域处理允许的域中查询。

下面的示例命令允许仅计算机和在 contoso.com 和子域查询 DNS 服务器的设备。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>允许只能从子网查询
你还可以创建允许列出的 IP 子网以便从这些子网中并非发出所有查询都忽略。

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>允许仅某些 QTypes
你可以对 QTYPEs 上应用允许列表。 

例如，如果你有外部查询 DNS 服务器接口 164.8.1.1 的客户，仅某些 QTYPEs 允许无法查询，如 SRV 或文本记录的内部服务器名称分辨率或监视的目的使用其他 QTYPEs 时。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。 
