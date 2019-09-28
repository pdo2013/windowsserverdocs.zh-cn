---
title: 使用 DNS 策略在 DNS 查询上应用筛选器
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 95b68995326dc3d3bf48ca36caa9b2ab4923a7c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406202"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>使用 DNS 策略在 DNS 查询上应用筛选器

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来了解如何在 Windows Server&reg; 2016 中配置 DNS 策略，以创建基于您提供的条件的查询筛选器。 

DNS 策略中的查询筛选器允许你将 DNS 服务器配置为基于发送 DNS 查询的 DNS 查询和 DNS 客户端以自定义方式进行响应。

例如，你可以通过查询筛选器阻止列表配置 DNS 策略，阻止来自已知恶意域的 DNS 查询，这会阻止 DNS 响应来自这些域的查询。 由于不会从 DNS 服务器发送响应，因此恶意域成员的 DNS 查询超时。

另一个示例是创建一个查询筛选器允许列表，该列表仅允许一组特定的客户端解析特定名称。

## <a name="bkmk_criteria"></a>查询筛选条件
您可以创建具有以下条件的任意逻辑组合（AND/OR/NOT）的查询筛选器。

|名称|描述|
|-----------------|---------------------|
|客户端子网|预定义的客户端子网的名称。 用于验证从中发送查询的子网。|
|传输协议|查询中使用的传输协议。 可能的值为 UDP 和 TCP。|
|Internet 协议|查询中使用的网络协议。 可能的值为 IPv4 和 IPv6。|
|服务器接口 IP 地址|接收 DNS 请求的 DNS 服务器的网络接口的 IP 地址。|
|FQDN|查询中记录的完全限定的域名，可能使用通配符。|
|查询类型|查询\(的记录类型，SRV，TXT，等等。\)|
|当天的时间|接收查询的时间。|

以下示例演示了如何创建 DNS 策略筛选器，以阻止或允许 DNS 名称解析查询。

>[!NOTE]
>本主题中的示例命令使用 Windows PowerShell 命令**DnsServerQueryResolutionPolicy**。 有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。 

## <a name="bkmk_block1"></a>阻止来自域的查询

在某些情况下，你可能想要阻止为你确定为恶意的域的 DNS 名称解析，或阻止不符合组织使用准则的域的 DNS 名称解析。 你可以使用 DNS 策略来完成域的阻塞查询。

在此示例中配置的策略不是在任何特定区域创建的，而是创建应用于 DNS 服务器上配置的所有区域的服务器级策略。 首先对服务器级别策略进行评估，并在 DNS 服务器收到查询时首先进行匹配。

下面的示例命令将服务器级别策略配置为阻止具有域**后缀 contosomalicious.com**的任何查询。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>当你用值**IGNORE**配置**操作**参数时，DNS 服务器配置为删除无响应的查询。 这会导致恶意域中的 DNS 客户端超时。

## <a name="bkmk_block2"></a>阻止子网中的查询
在此示例中，如果发现某个子网感染了某些恶意软件，并尝试使用你的 DNS 服务器与恶意站点联系，则可以阻止该子网中的查询。 

"DnsServerClientSubnet-Name" MaliciousSubnet06 "-IPv4Subnet 172.0.33.0/24-PassThru

DnsServerQueryResolutionPolicy-Name "BlockListPolicyMalicious06"-Action IGNORE-ClientSubnet "EQ，MaliciousSubnet06"-PassThru

下面的示例演示如何结合使用子网条件和 FQDN 标准来阻止受感染子网中某些恶意域的查询。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="bkmk_block3"></a>阻止查询类型
你可能需要在服务器上阻止对某些类型的查询进行名称解析。 例如，可以阻止 "ANY" 查询，这可用于恶意地创建放大攻击。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="bkmk_allow1"></a>仅允许来自域的查询
你不仅可以使用 DNS 策略来阻止查询，还可以使用它们自动批准来自特定域或子网的查询。 配置允许列表时，DNS 服务器仅处理来自允许域的查询，同时阻止其他域中的所有其他查询。

下面的示例命令仅允许 contoso.com 和子域中的计算机和设备查询 DNS 服务器。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="bkmk_allow2"></a>仅允许来自子网的查询
你还可以为 IP 子网创建允许列表，以便忽略并非源自这些子网的所有查询。

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="bkmk_allow3"></a>仅允许某些 QTypes
可以将允许列表应用到 QTYPEs。 

例如，如果您的外部客户查询 DNS 服务器接口164.8.1.1，则只允许查询某些 QTYPEs，而另一些 QTYPEs 如 SRV 或 TXT 记录，供内部服务器用来进行名称解析或用于监视目的。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。 
