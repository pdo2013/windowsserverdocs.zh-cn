---
title: 使用 DNS 策略 Split-Brain Active Directory 中的 DNS
description: 你可以使用本主题充分利用流量的使用 Active Directory 裂部署 DNS 策略管理功能集成 DNS 在 Windows Server 2016 的区域。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d294fcad3b48c8698dffd93e94f6ef7ffc681ea2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>使用 DNS 策略 Split-Brain Active Directory 中的 DNS

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题为在使用 Active Directory split\ 脑部署集成 DNS 充分利用 DNS 策略的交通管理功能在 Windows Server 2016 的区域。

Windows Server 2016，DNS 策略的支持扩展到 Active Directory 集成 DNS 区域。 集成的 Active Directory 提供对 DNS 服务器的 multi\ 母版高可用性功能。 

以前，此项 scenario 要求 DNS 管理员维护两种不同的 DNS 服务器、 于每组的用户，内部和外部每个提供服务。 如果只有区域内的几记录已 split\ brained 或者区域 （内部和外部） 的两个实例已委派给相同的家长域，这会变得管理难题。

>[!NOTE]
> - DNS 部署 split\ 脑时将有一个区域中的两个版本、适用于内部用户组织 intranet 上, 一个版本和外部，对于用户，通常情况下，用户在 Internet 上的一个版本。
> - 主题[使用 DNS 策略 Split-Brain DNS 部署](split-brain-DNS-deployment.md)解释了如何使用 DNS 策略和区域范围部署 split\ 脑 DNS 系统上的单个 Windows Server 2016 DNS 服务器。



##  <a name="example-split-brain-dns-in-active-directory"></a>示例 Split\ 脑 DNS Active Directory 中

此示例虚构公司，Contoso，维护 www.career.contoso.com 职业网站。

站点的两个版本，一个用于内部内部工作发布有的用户。 此内部站点是当地的 IP 地址 10.0.0.39 上可用。 

第二个版本是同一站点，可用的公用的 IP 地址 65.55.39.10 的公用版本。

如果没有 DNS 策略，在管理员需要承载不同的 Windows Server DNS 服务器上的这两个区域和单独管理他们。 

使用 DNS 策略这些区域可立即承载相同 DNS 服务器上。

如果 contoso.com DNS 服务器 Active Directory 集成，并且正在侦听两个网络接口，Contoso DNS 管理员可以按照实现 split\ 脑部署本主题中的步骤操作。

DNS 管理员配置 DNS 服务器接口，使用以下 IP 地址。

- 对于外部查询 208.84.0.53 公共 IP 地址配置 Internet 面向网络适配器。
- 专用 IP 地址的内部查询 10.0.0.56 配置 Intranet 面向网络适配器。

下图显示了这种情况。

![Split-Brain 广告集成 DNS 部署](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>对于 Split\ 脑 DNS Active Directory 中的 DNS 策略的工作原理

当使用所需的 DNS 策略配置 DNS 服务器时，每个名称分辨率请求针对 DNS 服务器上的策略评估。

服务器界面用于在此示例中为条件区分内部和外部的客户端。

在其收到查询服务器接口匹配任何策略，如果关联的区域范围用于响应查询。 

因此，在本例中，在专用 IP (10.0.0.56) 接收 DNS 查询的 www.career.contoso.com 接收 DNS 响应，其中包含内部 IP 地址;并且在公共网络接口收到 DNS 查询收到 DNS 响应，其中包含 （这是正常查询分辨率相同） 默认区域范围内的公共 IP 地址。  

仅在默认区域范围支持支持动态 DNS \(DDNS\) 更新和清理。 内部客户端维护的默认区域范围，因为 Contoso DNS 管理员可以继续使用现有的机制（动态 DNS 或静态）更新中 contoso.com 记录。为 non\ 默认区域范围 \（例如外部范围内此 example\ 中），DDNS 或清理支持不可用。

### <a name="high-availability-of-policies"></a>策略高可用性

DNS 策略不可 Active Directory 集成。 出于此原因，DNS 策略未复制到主持相同的 Active Directory 集成的区域的其他 DNS 服务器。 

本地 DNS 服务器上存储 DNS 策略。 你可以轻松地 DNS 策略从一个服务器导出到另一个通过使用以下命令示例 Windows PowerShell。

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

有关详细信息，请参阅下面的 Windows PowerShell 参考主题。

- [Get-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/get-dnsserverqueryresolutionpolicy)
- [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/add-dnsserverqueryresolutionpolicy)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>如何为 Active Directory 中 Split\ 脑 DNS 配置 DNS 策略

若要配置 DNS Split-Brain 使用 DNS 策略部署，必须使用以下各部分，提供了详细的配置说明进行操作。

### <a name="add-the-active-directory-integrated-zone"></a>添加 Active Directory 集成的区域

可以使用下面的示例命令添加到 DNS 服务器 Active Directory 集成的 contoso.com 区域。

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

有关详细信息，请参阅[添加 DnsServerPrimaryZone](https://technet.microsoft.com/library/jj649876.aspx)。

### <a name="create-the-scopes-of-the-zone"></a>创建该区域的范围

你可以使用此部分中进行分区区域 contoso.com 创建外部区域范围。

区域范围是唯一区域的实例。 DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可能存在多个范围，使用不同的 IP 地址或同一 IP 地址。 

添加 Active Directory 集成区域中的此新区域范围，因为区域范围和记录内部它将复制通过 Active Directory 到其他副本服务器域中。

默认情况下，区域范围存在每 DNS 区域中。 此区域范围作为区域，具有相同的名称，并传统 DNS 运营处理此范围。 此默认区域范围将举办 www.career.contoso.com 的内部版本。

可以使用下面的示例命令 DNS 服务器上创建区域范围。

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

有关详细信息，请参阅[添加 DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)。

### <a name="add-records-to-the-zone-scopes"></a>添加到区域范围记录

下一步是添加到这两个表示 web 服务器主机记录区域范围外部和默认 \(for internal clients\)。 

在默认内部区域的范围内，记录 www.career.contoso.com 添加 IP 地址 10.0.0.39，它是专用的 IP 地址。并且在外部区域范围内，相同的录制 \(www.career.contoso.com\) 添加 65.55.39.10 的公用 IP 地址。 

记录 \（两者中的默认内部区域范围和外部区域 scope\）将自动复制跨与他们各自的区域范围域。

你可以使用下面的示例命令添加到区域范围 DNS 服务器上的记录。

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>**区域范围 – 区域**参数时将不包括记录被添加到区域默认范围。 此操作才相同记录添加到正常的区域。

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

### <a name="create-the-dns-policies"></a>创建 DNS 策略

你已确定服务器界面外部网络和内部网络，并且你已创建区域范围后，你必须创建 DNS 连接内部和外部区域范围的策略。

>[!NOTE]
>此示例中使用服务器接口 \（例如命令 below\-ServerInterface 参数）作为条件，以便区分内部和外部的客户端。 另一种方法来区分内部和外部客户端是通过使用客户子网作为的条件。 如果你可以识别子网属于内部客户端，您可以配置 DNS 策略来区分根据客户网。 如何将配置客户端子网条件的使用的交通管理的信息，请参阅[使用与主要服务器的地理位置基于交通管理 DNS 策略](primary-geo-location.md)。

配置策略、DNS 查询的公用接口上收到时后，从区域外部范围返回答案。 

>[!NOTE]
>没有策略所需的映射默认内部区域范围。 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 为公共网络接口上的 IP 地址。

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。

现在 DNS 服务器配置需要 DNS 策略使用 Active Directory 裂名称服务器集成 DNS 的区域。

你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。 
