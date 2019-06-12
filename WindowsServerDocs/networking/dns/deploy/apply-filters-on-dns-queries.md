---
title: 使用 DNS 策略在 DNS 查询上应用筛选器
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e9322da3142c584c7b9d0a28396a1d1fd62ce6ee
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446401"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>使用 DNS 策略在 DNS 查询上应用筛选器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题以了解如何在 Windows Server 中配置 DNS 策略&reg;2016 可创建基于你提供的条件的查询筛选器。 

DNS 策略中的查询筛选器，可以配置 DNS 服务器根据 DNS 查询和发送 DNS 查询的 DNS 客户端的方式自定义响应。

例如，您可以使用查询筛选器阻止列表阻止来自已知恶意域的 DNS 查询会阻止 DNS 查询响应从这些域配置 DNS 策略。 从 DNS 服务器不发送任何响应，因为恶意域成员的 DNS 查询将会超时。

另一个示例是创建查询筛选器允许仅一组特定的客户端解析某些名称的允许列表。

## <a name="bkmk_criteria"></a> 查询筛选条件
以下任意条件中，创建具有任意逻辑组合的查询筛选器 (和/或/NOT)。

|名称|描述|
|-----------------|---------------------|
|客户端子网|预定义的客户端子网的名称。 用于验证从其发送查询的子网。|
|传输协议|传输协议在查询中使用。 可能的值为 UDP 和 TCP。|
|Internet 协议|在查询中使用的网络协议。 可能的值为 IPv4 和 IPv6。|
|服务器接口 IP 地址|收到 DNS 请求的 DNS 服务器的网络接口的 IP 地址。|
|FQDN|完全限定的域名的记录在查询中，与使用通配符的可能性。|
|查询类型|正在查询的记录的类型\(A、 SRV、 TXT 等\)。|
|当天的时间|接收到查询的时间。|

下面的示例演示了如何创建任一块筛选器的 DNS 策略或允许 DNS 名称解析查询。

>[!NOTE]
>本主题中的示例命令使用 Windows PowerShell 命令**添加 DnsServerQueryResolutionPolicy**。 有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。 

## <a name="bkmk_block1"></a>阻止域中的查询

在某些情况下您可能想要阻止的域已标识为恶意的行为，或不符合你的组织使用指南的域的 DNS 名称解析。 可以使用 DNS 策略来完成域的阻塞查询。

在此示例中配置的策略未创建任何特定区域上 – 改为创建服务器级别策略应用于 DNS 服务器上配置的所有区域。 服务器级别策略都要计算的第一个，因此首先要匹配的查询时收到的 DNS 服务器。

以下示例命令将配置服务器级别策略，以便阻止与域的任何查询**后缀 contosomalicious.com**。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>当配置**操作**的值的参数**忽略**，DNS 服务器配置为完全删除具有无响应的查询。 这将导致 DNS 客户端在超时对恶意域中。

## <a name="bkmk_block2"></a>从子网块查询
与此示例中，你可以阻止从子网的查询，如果它找到了一些恶意软件感染，但尝试联系恶意网站使用你的 DNS 服务器。 

` Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru

Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" -PassThru `

下面的示例演示如何结合使用 FQDN 条件中使用的子网条件阻止受感染的子网从某些恶意域的查询。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="bkmk_block3"></a>块类型的查询
您可能需要阻止在服务器上的查询的某些类型的名称解析。 例如，您可以阻止 ANY 查询，而这可以恶意使用，以创建放大攻击。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="bkmk_allow1"></a>允许仅从域查询
不能仅使用 DNS 策略来阻止查询，可用于自动批准从特定域或子网的查询。 在配置允许列表时，DNS 服务器只处理查询从允许的域，同时阻止来自其他域的所有其他查询。

以下示例命令只允许计算机和设备在 contoso.com 域和子域来查询 DNS 服务器。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="bkmk_allow2"></a>允许仅从子网的查询
您还可以创建允许列表的 IP 子网，以便不来自这些子网的所有查询将被都忽略。

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="bkmk_allow3"></a>允许仅特定 QTypes
您可以到 QTYPEs 中应用允许列表。 

例如，如果你的查询 DNS 服务器接口 164.8.1.1 的外部客户，仅某些 QTYPEs 允许其进行查询，尽管有其他 QTYPEs 等内部服务器进行名称解析或适用于监视目的使用 SRV 或 TXT 记录。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。 
