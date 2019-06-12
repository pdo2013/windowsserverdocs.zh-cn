---
title: 在 Active Directory 中使用针对拆分式 DNS 的 DNS 策略
description: 你可以使用本主题以利用流量的裂脑部署与 Active Directory 的 DNS 策略的管理功能集成的 Windows Server 2016 中的 DNS 区域。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 66931d2196b741e469cb726929f7b58985b8d0cd
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812147"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>在 Active Directory 中使用针对拆分式 DNS 的 DNS 策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题以利用拆分的 DNS 策略的流量管理功能\-大脑部署与 Active Directory 集成的 Windows Server 2016 中的 DNS 区域。

Windows Server 2016 中 DNS 的策略支持已扩展到 Active Directory 集成的 DNS 区域。 Active Directory 集成提供了多\-主 DNS 服务器的高可用性功能。 

以前，此方案所需的 DNS 管理员维护两个不同的 DNS 服务器，每个提供服务添加到每个用户，内部和外部集。 如果只在该区域内的几个记录已拆分\-brained 或区域 （内部和外部） 的两个实例已委派到同一个父域，这已经成为管理难题。

> [!NOTE]
> - DNS 部署拆分\-澍时有一个区域的两个版本，适用于内部用户的组织 intranet 上的一个版本，对于外部用户 – 而言，通常情况下，Internet 上的用户的一个版本。
> - 本主题[Split-Brain DNS 部署为使用 DNS 策略](split-brain-DNS-deployment.md)介绍了如何使用 DNS 策略和区域作用域部署拆分\-澍单个 Windows Server 2016 DNS 服务器上的 DNS 系统。



##  <a name="example-split-brain-dns-in-active-directory"></a>示例拆分\-澍 Active Directory 中的 DNS

此示例使用一个虚构公司 Contoso，维护 www.career.contoso.com 在职业网站。

站点具有两个版本，另一个用于提供内部招聘的内部用户。 此内部站点是本地 IP 地址 10.0.0.39 上可用。 

第二个版本是站点的相同，可在公共 IP 地址 65.55.39.10 的公共版本。

缺少的 DNS 策略的情况下，需要管理员来托管这些单独的 Windows Server DNS 服务器上的两个区域，并单独管理它们。 

使用 DNS 策略这些区域可以现在驻留在相同的 DNS 服务器。

如果 contoso.com 的 DNS 服务器是 Active Directory 集成，并且在两个网络接口上侦听，Contoso DNS 管理员可以按照本主题来实现拆分中的步骤\-澍部署。

使用以下 IP 地址，DNS 管理员配置的 DNS 服务器接口。

- 面向 Internet 的网络适配器配置具有 208.84.0.53 外部查询的公共 IP 地址。
- Intranet 面向网络适配器配置具有内部查询 10.0.0.56 专用 IP 地址。

下图描绘了此方案。

![裂脑 AD 集成的 DNS 部署](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>DNS 策略如何拆分\-澍 DNS 在 Active Directory 的工作原理

与所需的 DNS 策略配置 DNS 服务器，会针对 DNS 服务器上的策略评估每个名称解析请求。

服务器接口使用在此示例中用作条件的内部和外部客户端之间进行区分。

如果在其接收查询的服务器接口与匹配任何策略，关联的区域作用域用于对查询做出响应。 

因此，在本例中为专用 IP (10.0.0.56) 收到的 DNS 查询的 www.career.contoso.com 接收 DNS 响应，其中包含内部 IP 地址;和公共网络接口接收的 DNS 查询收到包含 （这是正常的查询解析相同） 的默认区域作用域中的公共 IP 地址的 DNS 响应。  

对动态 DNS 的支持\(DDNS\)更新和清理仅支持默认区域作用域。 由于内部客户端提供服务的默认区域作用域，因此 Contoso DNS 管理员可以继续使用现有机制 （动态 DNS 或静态） 以更新在 contoso.com 中的记录。 对于非\-默认区域作用域\(例如，在此示例中的外部作用域\)，DDNS 或清理支持不可用。

### <a name="high-availability-of-policies"></a>高可用性的策略

DNS 策略不是 Active Directory 集成。 正因为如此，DNS 策略不会复制到其他托管相同的 Active Directory 集成的区域的 DNS 服务器。 

DNS 策略存储在本地 DNS 服务器上。 您可以轻松地导出 DNS 策略从一台服务器到另一个通过使用下面的示例 Windows PowerShell 命令。

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

有关详细信息，请参阅以下 Windows PowerShell 参考主题。

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>如何配置拆分的 DNS 策略\-澍 Active Directory 中的 DNS

若要使用 DNS 策略配置 DNS Split-Brain 部署，必须使用以下部分中，提供详细的配置说明。

### <a name="add-the-active-directory-integrated-zone"></a>添加 Active Directory 集成的区域步骤

可以使用下面的示例命令将 Active Directory 集成的 contoso.com 区域添加到 DNS 服务器。

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

有关详细信息，请参阅[Add-dnsserverprimaryzone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps)。

### <a name="create-the-scopes-of-the-zone"></a>创建区域的作用域

可以使用本部分中进行分区来创建外部区域作用域的区域 contoso.com。

区域作用域是该区域的唯一实例。 DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址或相同的 IP 地址。 

要在 Active Directory 集成区域中添加此新的区域作用域，因为区域作用域和在其中记录将通过 Active Directory 复制到域中其他副本的服务器。

默认情况下，区域作用域中每个 DNS 区域存在。 此区域作用域为该区域具有相同的名称和旧的 DNS 操作适用于此作用域。 此默认区域作用域将承载 www.career.contoso.com 的内部版本。

以下示例命令可用于在 DNS 服务器上创建区域作用域。

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。

### <a name="add-records-to-the-zone-scopes"></a>将记录添加到区域作用域

下一步是添加到两个表示 web 服务器主机的记录区域作用域外部，默认\(内部客户端\)。 

在默认内部区域范围内，记录 www.career.contoso.com 添加 IP 地址 10.0.0.39，这是专用的 IP 地址;和在外部区域范围内，同一记录\(www.career.contoso.com\)具有公共 IP 地址 65.55.39.10 添加。 

记录\(在默认的内部区域范围和外部区域范围\)跨域使用其相应的区域作用域都将自动复制。

以下示例命令可用于将记录添加到 DNS 服务器上的区域作用域。

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

> [!NOTE]
> **– 区域范围区域**记录添加到默认区域作用域时不包括参数。 此操作是与将记录添加到正常的区域相同。

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="create-the-dns-policies"></a>创建 DNS 策略

您确定了外部网络和内部网络的服务器接口，并且已创建的区域作用域后，必须创建连接的内部和外部区域作用域的 DNS 策略。

> [!NOTE]
> 此示例使用的服务器界面\(在以下示例命令中的-ServerInterface 参数\)用作条件的内部和外部客户端之间进行区分。 外部和内部客户端之间进行区分的另一种方法是通过将用作条件的客户端子网。 如果可以识别为内部客户端属于的子网，你可以配置 DNS 策略来区分根据客户端子网。 有关如何配置流量管理使用客户端子网条件的信息，请参阅[地理位置基于流量管理和主服务器的使用 DNS 策略](primary-geo-location.md)。

公用接口上接收到的 DNS 查询时，配置策略后，从该区域的外部作用域返回答案。 

> [!NOTE]
> 没有策略所需的映射的默认内部区域作用域。 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

> [!NOTE]
> 208.84.0.53 是公共网络接口上的 IP 地址。

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

现在，DNS 服务器配置与所需的 DNS 策略为裂脑名称服务器与 Active Directory 集成 DNS 区域。

您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。 
