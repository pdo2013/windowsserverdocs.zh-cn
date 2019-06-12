---
title: 使用针对拆分式 DNS 部署的 DNS 策略
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c74bb2ee2f1647716c8c38e392434a5b7f01805f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446390"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>使用 DNS 策略执行拆分\-澍 DNS 部署

>适用于：Windows Server 2016

你可以使用本主题以了解如何在 Windows Server 中配置 DNS 策略&reg;2016 对于拆分式 DNS 部署，其中有两个版本的单个区域的另一个用于在您的组织的 intranet，内部用户和另一个用于外部用户，用户通常是 Internet 上的用户。

>[!NOTE]
>了解如何使用 DNS 策略执行拆分\-大脑 DNS 部署与 Active Directory 集成的 DNS 区域，请参阅[Split-Brain dns 在 Active Directory 中使用 DNS 策略](dns-sb-with-ad.md)。

以前，此方案所需的 DNS 管理员维护两个不同的 DNS 服务器，每个提供服务添加到每个用户，内部和外部集。 如果只在该区域内的几个记录已拆分\-brained 或区域 （内部和外部） 的两个实例已委派到同一个父域，这已经成为管理难题。 

拆分式部署的另一配置方案的 DNS 名称解析是选择性递归控件。 在某些情况下，企业 DNS 服务器需要时还必须作为外部用户的权威名称服务器，并为其阻止递归对于内部用户，Internet 上执行递归式解析。 

本主题包含以下部分。

- [拆分式 DNS 部署示例](#bkmk_sbexample)
- [DNS 选择性递归控件的示例](#bkmk_recursion)

## <a name="bkmk_sbexample"></a>拆分式 DNS 部署示例
下面是举例说明如何使用 DNS 策略来完成的拆分式 DNS 前面所述的方案。

本部分包含以下主题。

- [拆分式 DNS 部署的工作原理](#bkmk_sbhow)
- [如何配置拆分式 DNS 部署](#bkmk_sbconfigure)


此示例使用一个虚构公司 Contoso，维护 www.career.contoso.com 在职业网站。

站点具有两个版本，另一个用于提供内部招聘的内部用户。 此内部站点是本地 IP 地址 10.0.0.39 上可用。 

第二个版本是站点的相同，可在公共 IP 地址 65.55.39.10 的公共版本。

缺少的 DNS 策略的情况下，需要管理员来托管这些单独的 Windows Server DNS 服务器上的两个区域，并单独管理它们。 

使用 DNS 策略这些区域可以现在驻留在相同的 DNS 服务器。  

下图描绘了此方案。

![拆分式 DNS 部署](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


## <a name="bkmk_sbhow"></a>拆分式 DNS 部署的工作原理

与所需的 DNS 策略配置 DNS 服务器，会针对 DNS 服务器上的策略评估每个名称解析请求。

服务器接口使用在此示例中用作条件的内部和外部客户端之间进行区分。

如果在其接收查询的服务器接口与匹配任何策略，关联的区域作用域用于对查询做出响应。 

因此，在本例中为专用 IP (10.0.0.56) 收到的 DNS 查询的 www.career.contoso.com 接收 DNS 响应，其中包含内部 IP 地址;和公共网络接口接收的 DNS 查询收到包含 （这是正常的查询解析相同） 的默认区域作用域中的公共 IP 地址的 DNS 响应。  

## <a name="bkmk_sbconfigure"></a>如何配置拆分式 DNS 部署
若要使用 DNS 策略配置 DNS Split-Brain 部署，必须使用以下步骤。

- [创建区域作用域](#bkmk_zscopes)  
- [将记录添加到区域作用域](#bkmk_records)  
- [创建 DNS 策略](#bkmk_policies)

以下部分提供详细的配置说明。

>[!IMPORTANT]
>以下部分包含示例 Windows PowerShell 命令包含多个参数的示例值。 请确保将这些命令中的示例值替换之前运行这些命令适用于你的部署的值为。 

### <a name="bkmk_zscopes"></a>创建区域作用域

区域作用域是该区域的唯一实例。 DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址或相同的 IP 地址。 

> [!NOTE]
> 默认情况下，区域作用域的 DNS 区域上存在。 此区域作用域为该区域具有相同的名称和旧的 DNS 操作适用于此作用域。 此默认区域作用域将承载 www.career.contoso.com 外部版本。

可以使用下面的示例命令进行分区区域作用域 contoso.com，若要创建的内部区域作用域。 内部区域作用域将用于保留 www.career.contoso.com 的内部版本。

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>将记录添加到区域作用域

下一步是添加表示 Web 服务器主机到两个区域作用域的内部和默认值 （对于外部客户端） 的记录。 

在内部区域范围内，记录<strong>www.career.contoso.com</strong> 10.0.0.39，这是一个专用 IP; 的 IP 地址和默认区域作用域在同一个记录，添加<strong>www.career.contoso.com</strong>，是添加使用 65.55.39.10 的 IP 地址。

否 **– 区域范围区域**记录被添加到默认区域作用域时，在下面的示例命令中提供的参数。 这是类似于将记录添加到普通的区域。

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="bkmk_policies"></a>创建 DNS 策略

您确定了外部网络和内部网络的服务器接口，并且已创建的区域作用域后，必须创建连接的内部和外部区域作用域的 DNS 策略。

>[!NOTE]
>此示例使用的服务器界面用作条件的内部和外部客户端之间进行区分。 外部和内部客户端之间进行区分的另一种方法是通过将用作条件的客户端子网。 如果可以识别为内部客户端属于的子网，你可以配置 DNS 策略来区分根据客户端子网。 有关如何配置流量管理使用客户端子网条件的信息，请参阅[地理位置基于流量管理和主服务器的使用 DNS 策略](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers)。

当 DNS 服务器收到的专用接口上的查询时，从内部区域作用域返回 DNS 查询响应。

>[!NOTE]
>没有策略所需的映射的默认区域作用域。 

在以下示例命令中，10.0.0.56 是专用网络接口上的 IP 地址，如在上图中所示。

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  


## <a name="bkmk_recursion"></a>DNS 选择性递归控件的示例

下面是举例说明如何使用 DNS 策略来完成 DNS 选择性递归控件的前面所述的方案。

本部分包含以下主题。

- [如何 DNS 选择性递归控制的工作原理](#bkmk_recursionhow)
- [如何配置 DNS 选择性递归控件](#bkmk_recursionconfigure)

此示例使用与前面的示例中，Contoso，维护 www.career.contoso.com 在职业网站的同一虚构公司。

在 DNS 裂脑部署示例中，相同的 DNS 服务器响应两个外部和内部客户端，并为用户提供不同的答案。 

某些 DNS 部署可能需要相同的 DNS 服务器，除了对于外部客户端充当权威名称服务器的内部客户端执行递归名称解析。 这种情况下被称为 DNS 选择性递归控制。

在以前版本的 Windows Server 中，启用递归意味着所有区域的整个 DNS 服务器上启用了它。 因为 DNS 服务器还侦听外部查询，递归处于启用状态，对于内部和外部客户端，从而使该 DNS 服务器打开的冲突解决程序。 

DNS 服务器配置为打开的冲突解决程序可能受到资源耗尽，并可能由恶意客户端用于创建反射攻击会被滥用。 

因此，Contoso DNS 管理员不希望 contoso.com 对于外部客户端执行递归名称解析的 DNS 服务器。 没有仅需要递归控件内部的客户端，而递归控件可以为外部客户端被阻止。 

下图描绘了此方案。

![选择性递归控件](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>如何 DNS 选择性递归控制的工作原理

如果收到 Contoso DNS 服务器是为其非权威的查询，如为 www.microsoft.com，则名称解析请求计算针对 DNS 服务器上的策略。 

因为这些查询不属于任何区域，在区域级别策略\(裂脑示例中定义\)不会评估。 

DNS 服务器的计算结果的递归策略，并在专用接口匹配收到的查询**SplitBrainRecursionPolicy**。 此策略指向在其中启用递归的递归作用域。

然后，DNS 服务器执行递归 www.microsoft.com 从 Internet 获取答案，并缓存响应本地。 

如果外部接口，没有 DNS 策略匹配项，以及默认递归设置-在这种情况下即会收到查询**禁用**的应用。

这可阻止服务器充当对于外部客户端，一个打开冲突解决程序，而它充当内部客户端缓存解析程序。 

### <a name="bkmk_recursionconfigure"></a>如何配置 DNS 选择性递归控件

若要使用 DNS 策略配置 DNS 选择性递归控件，必须使用以下步骤。

- [创建 DNS 递归作用域](#bkmk_recscopes)
- [创建 DNS 递归策略](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>创建 DNS 递归作用域

递归作用域是组的控制递归 DNS 服务器上的设置的唯一实例。 递归作用域包含的转发器列表，并指定递归处于启用状态。 DNS 服务器可以有多个递归作用域。 

旧递归设置和转发器列表称为默认递归作用域。 无法添加或删除默认递归作用域，标识名称点\("。"\).

在此示例中，默认递归设置已禁用，而新的递归范围内部客户端以及用于创建递归处于启用状态。

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

有关详细信息，请参阅[添加 DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>创建 DNS 递归策略

您可以创建 DNS 服务器递归策略选择一组查询符合特定条件的递归作用域。 

如果 DNS 服务器，对于某些查询权威 DNS 服务器递归策略，可以控制如何解析查询。 

在此示例中，递归已启用的内部递归作用域是与专用网络接口相关联。

以下示例命令可用于配置 DNS 递归策略。

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

现在的 DNS 服务器与所需的 DNS 策略的裂脑名称服务器或 DNS 服务器配置了内部客户端启用的选择性递归控制。

您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。 

有关详细信息，请参阅[DNS 策略方案指南](DNS-Policy-Scenario-Guide.md)。
