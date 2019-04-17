---
title: 使用 DNS 策略 Split-Brain DNS 部署
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b25ac752ea347f4d184628eb26bc7e297443306
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>使用 DNS Split\ 脑 DNS 部署策略

>适用于：Windows Server 2016

你可以使用本主题以了解如何在 Windows Server 配置 DNS 策略&reg;2016 裂 DNS 部署，有两种版本的一个区域的另一个用于你的组织 intranet 上内部用户和另一个用于外部用户，通常都在 Internet 上的用户。

>[!NOTE]
>有关如何使用 DNS 策略为在使用 Active Directory split\ 脑 DNS 部署集成 DNS 区域的信息，请参阅[使用 Active Directory 中 Split-Brain DNS DNS 策略](dns-sb-with-ad.md)。

以前，此项 scenario 要求 DNS 管理员维护两种不同的 DNS 服务器、 于每组的用户，内部和外部每个提供服务。 如果只有区域内的几记录已 split\ brained 或者区域 （内部和外部） 的两个实例已委派给相同的家长域，这会变得管理难题。 

裂部署另一种情况配置 DNS 名称分辨率是选择性递归控件。 在某些情况下，我们希望时，它们还必须充当授权名称外部用户，服务器，并阻止递归它们适用于内部用户，在 Internet 上执行递归分辨率企业 DNS 服务器。 

本主题包含以下部分。

- [示例中的 DNS 裂部署](#bkmk_sbexample)
- [DNS 选择性递归控件的示例](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>示例中的 DNS 裂部署
以下是一种方式使用 DNS 策略以完成裂 DNS 前面所述的方案。

此部分中包含以下主题。

- [DNS 裂部署的工作原理](#bkmk_sbhow)
- [如何配置 DNS 裂部署](#bkmk_sbconfigure)


此示例虚构公司，Contoso，维护 www.career.contoso.com 职业网站。

站点的两个版本，一个用于内部内部工作发布有的用户。 此内部站点是当地的 IP 地址 10.0.0.39 上可用。 

第二个版本是同一站点，可用的公用的 IP 地址 65.55.39.10 的公用版本。

如果没有 DNS 策略，在管理员需要承载不同的 Windows Server DNS 服务器上的这两个区域和单独管理他们。 

使用 DNS 策略这些区域可立即承载相同 DNS 服务器上。  

下图显示了这种情况。

![裂 DNS 部署](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>DNS 裂部署的工作原理

当使用所需的 DNS 策略配置 DNS 服务器时，每个名称分辨率请求针对 DNS 服务器上的策略评估。

服务器界面用于在此示例中为条件区分内部和外部的客户端。

在其收到查询服务器接口匹配任何策略，如果关联的区域范围用于响应查询。 

因此，在本例中，在专用 IP (10.0.0.56) 接收 DNS 查询的 www.career.contoso.com 接收 DNS 响应，其中包含内部 IP 地址;并且在公共网络接口收到 DNS 查询收到 DNS 响应，其中包含 （这是正常查询分辨率相同） 默认区域范围内的公共 IP 地址。  

##<a name="bkmk_sbconfigure"></a>如何配置 DNS 裂部署
若要使用 DNS 策略配置 DNS Split-Brain 部署，必须使用以下步骤。

- [创建区域范围](#bkmk_zscopes)  
- [添加到区域范围记录](#bkmk_records)  
- [创建 DNS 策略](#bkmk_policies)

以下部分提供了详细的配置说明进行操作。

>[!IMPORTANT]
>以下部分包括包含许多参数示例值的示例 Windows PowerShell 命令。 确保你在运行以下命令之前就提供适用于你的部署的值与替换示例值在这些命令。 

###<a name="bkmk_zscopes"></a>创建区域范围

区域范围是唯一区域的实例。 DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可能存在多个范围，使用不同的 IP 地址或同一 IP 地址。 

>[!NOTE]
>默认情况下，区域范围存在上 DNS 区域。 此区域范围作为区域，具有相同的名称，并传统 DNS 运营处理此范围。 此默认区域范围将举办 www.career.contoso.com 外部版本。

你可以使用下面的示例命令进行分区区域范围 contoso.com，若要创建内部区域范围。 内部区域范围将用于保留 www.career.contoso.com 的内部版本。

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

有关详细信息，请参阅[添加 DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records"></a>添加到区域范围记录

下一步是添加表示 （适用于外部客户端） 的 Web 服务器主机内的两个区域范围-内部到和默认的记录。 

在内部区域范围内，记录**www.career.contoso.com**添加的 IP 地址 10.0.0.39，它是专用 IP;在默认区域范围相同的记录**www.career.contoso.com**，添加 65.55.39.10 的 IP 地址。

不**区域范围 – 区域**参数提供以下示例命令时将记录添加到区域默认范围。 这是类似于香草区域中添加记录。

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

###<a name="bkmk_policies"></a>创建 DNS 策略

你已确定服务器界面外部网络和内部网络，并且你已创建区域范围后，你必须创建 DNS 连接内部和外部区域范围的策略。

>[!NOTE]
>此示例中使用服务器接口作为条件，以便区分内部和外部的客户端。 另一种方法来区分内部和外部客户端是通过使用客户子网作为的条件。 如果你可以识别子网属于内部客户端，您可以配置 DNS 策略来区分根据客户网。 如何将配置客户端子网条件的使用的交通管理的信息，请参阅[使用与主要服务器的地理位置基于交通管理 DNS 策略](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers)。

DNS 服务器收到专用接口在查询时，将从内部区域范围返回 DNS 查询响应。

>[!NOTE]
>没有策略所需的映射默认区域范围。 

在下面的示例命令 10.0.0.56 是专用网络接口上的 IP 地址，在上图所示。

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。  


## <a name="bkmk_recursion"></a>DNS 选择性递归控件的示例

以下是一种方式使用 DNS 策略以完成 DNS 选择性递归控制前面所述的方案。

此部分中包含以下主题。

- [如何 DNS 选择性递归控制工作原理](#bkmk_recursionhow)
- [如何配置 DNS 选择性递归控制](#bkmk_recursionconfigure)

此示例如下所示上例 Contoso，维护职业网站 www.career.contoso.com 在同一虚构的公司。

DNS 裂部署示例中，在同一 DNS 服务器响应内部和外部客户，并为他们提供不同的答案。 

某些 DNS 部署可能需要相同 DNS 服务器执行除了充当名称权威服务器外部客户端的内部客户端递归名称分辨率。 这种情况下称为 DNS 选择性递归控件。

在以前版本的 Windows Server，启用递归意味着所有区域整个 DNS 服务器上启用了它。 DNS 服务器在外部查询还倾听，因为递归已启用内部和外部客户端，使 DNS 服务器打开解析程序。 

已设置为打开解析程序可能会受到资源耗尽，而且可能会被恶意客户端创建倒影攻击滥用 DNS 服务器。 

出于此原因，Contoso DNS 管理员不希望 contoso.com 来执行外部客户端递归名称分辨率的 DNS 服务器。 可以阻止递归控制外部客户端时只需内部客户端，递归控件。 

下图显示了这种情况。

![选择性递归控件](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>如何 DNS 选择性递归控制工作原理

如果收到为其 Contoso DNS 服务器是否非授权查询，如为 www.microsoft.com，然后名称分辨率请求针对评估 DNS 服务器上的策略。 

因为这些查询执行落下的任何区域，区域级别策略 \ （如定义裂 example\ 中） 不计算。 

DNS 服务器计算递归策略和查询专用接口匹配所接收**SplitBrainRecursionPolicy**。 此策略指向启用递归了递归范围。

然后，DNS 服务器执行递归若要从 Internet，获取 www.microsoft.com 答案，并将响应的本地缓存。 

如果查询收到外部接口、 没有 DNS 策略匹配项，以及默认递归设置-在此情况下是**禁用**的应用。

这可以防止服务器充当对于外部客户端，打开解析程序时它充当缓存内部客户端解析程序。 

### <a name="bkmk_recursionconfigure"></a>如何配置 DNS 选择性递归控制

若要使用 DNS 策略配置 DNS 选择性递归控件，必须使用以下步骤。

- [创建 DNS 递归范围](#bkmk_recscopes)
- [创建 DNS 递归策略](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>创建 DNS 递归范围

递归范围是一组控制递归 DNS 服务器上的设置的唯一实例。 递归范围包含转发器的列表，并指定递归是否已启用。 有许多递归范围 DNS 服务器。 

旧版递归设置和转发器列表称为默认递归范围。 你无法添加或删除默认递归范围，由名点标识 \("."\).

在此示例中，禁用默认递归设置，尽管启用递归了创建新的内部客户端递归范围。

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

有关详细信息，请参阅[添加 DnsServerRecursionScope](https://technet.microsoft.com/library/mt126268.aspx)

#### <a name="bkmk_recpolicy"></a>创建 DNS 递归策略

你可以创建 DNS 服务器递归选择递归范围的一组查询符合条件的特定策略。 

如果不权威对于某些查询 DNS 服务器，DNS 服务器递归策略允许你控制如何解决查询。 

在此示例中，启用了递归内部递归范围是与专用网络接口

你可以使用下面的示例命令配置 DNS 递归策略。

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.39"
    

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。

现在 DNS 服务器具有所需的 DNS 策略裂名称服务器或 DNS 服务器配置选择性递归控制启用内部客户端的使用。

你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。 

有关详细信息，请参阅[DNS 策略方案指南](DNS-Policy-Scenario-Guide.md)。
