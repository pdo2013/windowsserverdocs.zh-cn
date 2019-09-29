---
title: 使用针对拆分式 DNS 部署的 DNS 策略
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5449c9e96a5a9ecd08ca35e703a76927f4e27158
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356019"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>将 DNS 策略用于 Split @ no__t-0Brain DNS 部署

>适用于：Windows Server 2016

你可以使用本主题来了解如何在 Windows Server @ no__t-0 2016 中为拆分的 DNS 部署配置 DNS 策略，其中有两个版本的单个区域，一个用于组织 intranet 上的内部用户，另一个用于外部用户，后者是通常是 Internet 上的用户。

>[!NOTE]
>有关如何将 DNS 策略用于 split @ no__t-0brain DNS 部署与 Active Directory 集成 DNS 区域的信息，请参阅[在 Active Directory 中将 Dns 策略用于裂脑 dns](dns-sb-with-ad.md)。

以前，此方案要求 DNS 管理员维护两个不同的 DNS 服务器，每个服务器为内部和外部用户提供服务。 如果区域中只有几条记录被拆分 @ no__t-0brained，或同时向同一个父域委派了区域的两个实例（内部和外部），这就成为了管理难题。 

用于拆分的另一种配置方案是选择性递归控制 DNS 名称解析。 在某些情况下，企业 DNS 服务器应该为内部用户在 Internet 上执行递归解析，同时，它们还必须充当外部用户的权威名称服务器，并阻止它们的递归。 

本主题包含以下部分。

- [DNS 拆分的部署示例](#bkmk_sbexample)
- [DNS 选择性递归控制的示例](#bkmk_recursion)

## <a name="bkmk_sbexample"></a>DNS 拆分的部署示例
下面的示例演示了如何使用 DNS 策略来完成上述拆分后的 DNS 应用场景。

本部分包含以下主题。

- [DNS 拆分过程的工作原理](#bkmk_sbhow)
- [如何配置 DNS 拆分的部署](#bkmk_sbconfigure)


此示例使用一家虚构公司 Contoso，该公司在 www.career.contoso.com 维护一个职业网站。

站点有两个版本，一个用于内部工作投递可用的内部用户。 此内部站点可在本地 IP 地址10.0.0.39 上找到。 

第二个版本是同一站点的公共版本，可在公共 IP 地址65.55.39.10 获取。

在缺少 DNS 策略的情况下，管理员需要在单独的 Windows Server DNS 服务器上托管这两个区域，并单独对其进行管理。 

使用 DNS 策略，现在可以在同一台 DNS 服务器上托管这些区域。  

下图描述了此方案。

![裂脑 DNS 部署](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


## <a name="bkmk_sbhow"></a>DNS 拆分过程的工作原理

当 DNS 服务器配置了所需的 DNS 策略时，将针对 DNS 服务器上的策略评估每个名称解析请求。

在此示例中，使用服务器接口来区分内部和外部客户端。

如果接收查询的服务器接口与任何策略匹配，则将使用关联的区域范围来响应查询。 

因此，在我们的示例中，对专用 IP （10.0.0.56）上收到的 www.career.contoso.com 的 DNS 查询接收包含内部 IP 地址的 DNS 响应;在公共网络接口上接收的 DNS 查询接收包含默认区域作用域中的公共 IP 地址的 DNS 响应（这与正常的查询解决方法相同）。  

## <a name="bkmk_sbconfigure"></a>如何配置 DNS 拆分的部署
若要使用 DNS 策略配置 DNS 拆分的部署，必须使用以下步骤。

- [创建区域作用域](#bkmk_zscopes)  
- [将记录添加到区域作用域](#bkmk_records)  
- [创建 DNS 策略](#bkmk_policies)

以下各节提供了详细的配置说明。

>[!IMPORTANT]
>以下各节包含示例 Windows PowerShell 命令，其中包含许多参数的示例值。 在运行这些命令之前，请确保将这些命令中的示例值替换为适用于你的部署的值。 

### <a name="bkmk_zscopes"></a>创建区域作用域

区域作用域是区域的唯一实例。 DNS 区域可以有多个区域作用域，每个区域作用域包含其自己的一组 DNS 记录。 同一条记录可以出现在多个作用域中，具有不同的 IP 地址或相同的 IP 地址。 

> [!NOTE]
> 默认情况下，在 DNS 区域中存在区域作用域。 此区域作用域具有与区域相同的名称，并且旧式 DNS 操作在此作用域上起作用。 此默认区域作用域将托管 www.career.contoso.com 的外部版本。

你可以使用以下示例命令对区域作用域 contoso.com 进行分区，以创建内部区域作用域。 内部区域作用域将用于保留 www.career.contoso.com 的内部版本。

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

有关详细信息，请参阅[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>将记录添加到区域作用域

下一步是将代表 Web 服务器主机的记录添加到这两个区域作用域中：内部和默认（对于外部客户端）。 

在内部区域作用域中，将使用 IP 地址10.0.0.39 （这是一个专用 IP）添加记录<strong>www.career.contoso.com</strong> 。在默认区域范围内，同一记录<strong>www.career.contoso.com</strong>与 IP 地址65.55.39.10 一起添加。

在将记录添加到默认区域作用域时，以下示例命令中未提供 **-ZoneScope**参数。 这类似于将记录添加到 vanilla 区域。

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

有关详细信息，请参阅[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="bkmk_policies"></a>创建 DNS 策略

确定外部网络和内部网络的服务器接口并创建了区域作用域后，必须创建连接内部和外部区域作用域的 DNS 策略。

>[!NOTE]
>此示例使用服务器界面作为条件来区分内部和外部客户端。 区分外部和内部客户端的另一种方法是使用客户端子网作为条件。 如果可以识别内部客户端所属的子网，则可以将 DNS 策略配置为基于客户端子网来区分。 有关如何使用客户端子网标准配置流量管理的信息，请参阅[将 DNS 策略用于基于地理位置的流量管理和主服务器](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers)。

当 DNS 服务器在专用接口上收到查询时，将从内部区域作用域返回 DNS 查询响应。

>[!NOTE]
>映射默认区域作用域不需要策略。 

在下面的示例命令中，10.0.0.56 是专用网络接口上的 IP 地址，如上图所示。

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  


## <a name="bkmk_recursion"></a>DNS 选择性递归控制的示例

下面的示例演示了如何使用 DNS 策略来完成上述 DNS 选择性递归控制方案。

本部分包含以下主题。

- [DNS 选择性递归控制的工作原理](#bkmk_recursionhow)
- [如何配置 DNS 选择性递归控制](#bkmk_recursionconfigure)

此示例使用与上一个示例 Contoso 相同的虚构公司，该公司在 www.career.contoso.com 上维护一个职业网站。

在 DNS 拆分的部署示例中，相同的 DNS 服务器对外部和内部客户端进行响应，并为它们提供不同的答案。 

除了充当外部客户端的权威名称服务器外，某些 DNS 部署可能还需要相同的 DNS 服务器来执行内部客户端的递归名称解析。 这种情况称为 DNS 选择性递归控制。

在以前版本的 Windows Server 中，启用递归意味着它在所有区域的整个 DNS 服务器上都处于启用状态。 由于 DNS 服务器也在侦听外部查询，同时为内部和外部客户端启用了递归，从而使 DNS 服务器成为打开的解析程序。 

配置为打开的解析程序的 DNS 服务器可能易受到资源耗尽的攻击，并可能被恶意客户端滥用以创建反射攻击。 

因此，Contoso DNS 管理员不希望 contoso.com 的 DNS 服务器对外部客户端执行递归名称解析。 对于内部客户端，只需要递归控制，但对于外部客户端，可以阻止递归控制。 

下图描述了此方案。

![选择性递归控制](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>DNS 选择性递归控制的工作原理

如果为其接收了 Contoso DNS 服务器的查询（例如，对于 www.microsoft.com），则会针对 DNS 服务器上的策略评估名称解析请求。 

由于这些查询不在任何区域，因此，不会对在0as 中定义的区域级别策略 @no__t-no__t 进行计算。 

DNS 服务器评估递归策略，专用接口上收到的查询与**SplitBrainRecursionPolicy**匹配。 此策略指向启用了递归的递归作用域。

然后，DNS 服务器将执行递归以从 Internet 获取 www.microsoft.com 的答案，并将响应缓存在本地。 

如果在外部接口上收到查询，则不匹配任何 DNS 策略，并且默认递归设置（在此情况下为**禁用状态**）会应用。

这会阻止服务器作为外部客户端的开放解析程序，同时充当内部客户端的缓存解析程序。 

### <a name="bkmk_recursionconfigure"></a>如何配置 DNS 选择性递归控制

若要使用 DNS 策略配置 DNS 选择性递归控制，必须使用以下步骤。

- [创建 DNS 递归作用域](#bkmk_recscopes)
- [创建 DNS 递归策略](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>创建 DNS 递归作用域

递归作用域是一组设置的唯一实例，这些设置控制 DNS 服务器上的递归。 递归作用域包含转发器列表，并指定是否启用了递归。 DNS 服务器可以有多个递归作用域。 

旧递归设置和转发器列表被称为默认递归作用域。 不能添加或删除默认递归作用域，由名称点 \( "标识\)。

在此示例中，默认递归设置处于禁用状态，而内部客户端的新递归作用域是在启用了递归的位置创建的。

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

有关详细信息，请参阅[DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>创建 DNS 递归策略

你可以创建 DNS 服务器递归策略，为一组符合特定条件的查询选择一个递归作用域。 

如果 DNS 服务器对某些查询没有权威，则可以通过 DNS 服务器递归策略来控制如何解析查询。 

在此示例中，启用了递归的内部递归范围与专用网络接口相关联。

你可以使用以下示例命令来配置 DNS 递归策略。

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

现在，使用为内部客户端启用了选择性递归控制的拆分的名称服务器或 DNS 服务器来配置 DNS 服务器。

你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。 

有关详细信息，请参阅[DNS 策略方案指南](DNS-Policy-Scenario-Guide.md)。
