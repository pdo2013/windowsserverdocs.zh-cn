---
title: 使用应用程序负载平衡 DNS 策略
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d156c258b971c45bf1c4c20739440bd5cc9e239f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing"></a>使用应用程序负载平衡 DNS 策略

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解如何配置 DNS 策略应用程序负载平衡执行。

以前版本的 Windows Server DNS 只提供负载平衡使用循环响应;但是与 Windows Server 2016 中的 DNS，您可以配置应用程序负载平衡 DNS 策略。

当部署了多个应用程序的情况下时，你可以使用 DNS 策略平衡交通负载不同的应用程序的情况下，从而动态分配的应用程序的交通加载之间。

## <a name="example-of-application-load-balancing"></a>应用程序负载平衡它的示例

以下是一种方式可用于 DNS 策略应用程序负载平衡。

此示例中使用一个虚构公司-Contoso 礼品服务-其提供 gifing 联机服务，并方面的优化调整名为网站**contosogiftservices.com**。

在每个有不同的 IP 地址的多个数据中心中托管 contosogiftservices.com 网站。

北美，即主要市场 Contoso 礼品服务的网站托管三个中心： 芝加哥，IL，达拉斯州奥斯汀西雅图，WA。

西雅图 Web 服务器最佳的硬件配置，并且可以与其他两个站点处理倍加载。 Contoso 礼品服务想应用程序交通定向按以下方式。

- 由于西雅图 Web 服务器包含更多资源的客户端应用程序的一半定向到此服务器
- 四分之一的应用程序的客户端将定向到数据中心，达拉斯州奥斯汀
- 四分之一的应用程序的客户端将定向到芝加哥，IL、 数据中心

下图显示了这种情况。

![DNS 应用程序使用负载平衡 DNS 策略](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>应用程序如何负载平衡工作原理

你需要完成设置后 DNS 服务器具有 DNS 策略应用程序加载平衡使用此示例情况下，DNS 服务器响应 50%西雅图 Web 服务器地址，达拉斯 Web 服务器地址，使用时间的 25%和芝加哥 Web 服务器地址的时间 25%的时间。

因此对于每个 DNS 服务器接收的四个查询，它响应使用两种响应西雅图和每一个都达拉斯和芝加哥。

一个可能负载平衡与 DNS 策略问题是通过 DNS 客户端解析程序/LDNS，这可以干扰负载平衡，因为在客户端或解析程序不发送查询到 DNS 服务器 DNS 记录的缓存。

您可以使用较低的 Time\ to\ Live \(TTL\) 值应负载平衡 DNS 记录降低此问题的影响。

### <a name="how-to-configure-application-load-balancing"></a>如何配置应用程序负载平衡

以下各部分将向您展示如何配置应用程序负载平衡 DNS 策略。

#### <a name="create-the-zone-scopes"></a>创建区域范围

你必须先创建的范围区域 contosogiftservices.com 数据中心承载位置。

区域范围是唯一区域的实例。 DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可能存在多个范围，使用不同的 IP 地址或同一 IP 地址。

>[!NOTE]
>默认情况下，区域范围存在上 DNS 区域。 此区域范围作为区域，具有相同的名称，并传统 DNS 运营处理此范围。

可以使用下面的 Windows PowerShell 命令来创建区域范围。
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

有关详细信息，请参阅[添加 DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

####<a name="bkmk_records"></a>添加到区域范围记录

现在，你必须添加到了区域范围表示 web 服务器主机的记录。

在**SeattleZoneScope**，你可以添加记录 www.contosogiftservices.com IP 地址 192.0.0.1，它位于西雅图数据中心。

在**ChicagoZoneScope**，你可以添加 IP 地址 182.0.0.1 相同的录制 \(www.contosogiftservices.com\) 芝加哥数据中心中。

同样在**DallasZoneScope**，你可以添加 IP 地址 162.0.0.1 记录 \(www.contosogiftservices.com\) 芝加哥数据中心中。

你可以使用 Windows PowerShell 以下命令添加到区域范围记录。
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

####<a name="bkmk_policies"></a>创建 DNS 策略

你已创建分区 （区域范围） 并添加了记录后，你必须创建分布传入查询这些范围，以便 contosogiftservices.com 查询 50%响应到西雅图数据中心中的 Web 服务器的 IP 地址并其余同样分发之间芝加哥和达拉斯数据中心中的 DNS 策略。

以下 Windows PowerShell 命令用于创建余额这些三个数据中心跨应用程序交通 DNS 策略。

>[!NOTE]
>在下方，expression 示例命令-区域范围区域"SeattleZoneScope，2;ChicagoZoneScope，1;DallasZoneScope，1"配置 DNS 服务器提供一系列包含参数组合 \ < ZoneScope\，> \ < weight\ >。
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW – -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。  

现在，你已成功创建之间三个不同的数据中心中的 Web 服务器提供应用程序负载平衡 DNS 策略。

你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。