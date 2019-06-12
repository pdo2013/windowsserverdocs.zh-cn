---
title: 使用针对基于地理位置的流量管理和主要-辅助部署的 DNS 策略
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cf66a306c7f023852cec93d6458e74a99c46c831
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812110"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>使用针对基于地理位置的流量管理和主要-辅助部署的 DNS 策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解如何创建基于地理位置的流量管理的 DNS 策略时你的 DNS 部署包括主要和辅助 DNS 服务器。  

前面的方案中，[地理位置基于流量管理和主服务器的使用 DNS 策略](primary-geo-location.md)，提供基于地理位置的流量管理的 DNS 策略配置主 DNS 服务器上的说明。 在 Internet 基础结构，但是，DNS 服务器广泛部署在主要和辅助模型，其中选择和安全的主服务器上存储区域的可写副本，而该区域的只读副本保留在多个辅助服务器上。   
  
辅助服务器使用权威传输 (AXFR) 和增量区域传输 (IXFR) 的区域传输协议请求并接收区域更新，其中包括对主 DNS 服务器上区域的新更改。   
  
> [!NOTE]
> 有关 AXFR 的详细信息，请参阅 Internet 工程任务组 (IETF)[征求意见 5936](https://tools.ietf.org/rfc/rfc5936.txt)。 有关 IXFR 的详细信息，请参阅 Internet 工程任务组 (IETF)[征求意见 1995年](https://tools.ietf.org/html/rfc1995)。  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>主要和辅助地理位置基于流量管理示例  
下面是如何在主要和辅助部署中使用 DNS 策略以实现基于客户端执行 DNS 查询的物理位置的流量重定向的示例。  
  
此示例使用两个虚构的公司 Contoso 云服务，从而提供 web 应用和托管解决方案; 的域和 Woodgrove 食物服务，它在全球范围内提供多个城市中的食物传送服务，并且具有网站上名为 woodgrove.com。  
  
若要确保 woodgrove.com 客户从他们的网站获取一种响应体验，Woodgrove 想欧洲客户端定向到在欧洲数据中心和美国的客户端定向到美国数据中心。 可以将位于世界其他位置的客户定向到在数据中心之一。  
  
Contoso 云服务有两个数据中心，一个在美国，另一个在欧洲，Contoso 在其承载其餐 woodgrove.com 的门户。  
  
Contoso DNS 部署包括两个辅助服务器：**SecondaryServer1**，使用 IP 地址 10.0.0.2; 并**SecondaryServer2**，用 IP 地址 10.0.0.3。 这些辅助服务器都作为与 SecondaryServer1 位于欧洲和 SecondaryServer2 位于美国、 位于两个不同的区域的名称服务器
  
上没有可写的主要区域复制**PrimaryServer** （IP 地址为 10.0.0.1） 进行区域更改。 常规区域复制到辅助服务器，与辅助服务器始终是最新的对 PrimaryServer 上区域的任何新更改。
  
下图描绘了此方案。
  
![主要和辅助地理位置基于流量管理示例](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>DNS 主要和辅助系统的工作原理

部署基于地理位置中的主要和辅助 DNS 部署的流量管理时，必须了解如何正常传输之前了解区域作用域级别传输发生的主要和辅助区域。 以下各节提供有关区域和区域作用域级别传送的信息。  
  
- [DNS 主要-辅助部署中的区域传送](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [DNS 主要-辅助部署中的区域作用域级别传送](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>DNS 主要-辅助部署中的区域传送

您可以创建 DNS 主要-辅助部署，并通过执行以下步骤同步区域。  
1. 在安装 DNS 时，主 DNS 服务器上创建主要区域。  
2. 在辅助服务器上，创建区域和指定的主服务器。   
3. 在主服务器上可以作为受信任的辅助数据库上的主要区域添加辅助服务器。   
4. 辅助区域的完整区域复制请求 (AXFR) 并享受该区域的副本。   
5. 需要时，主服务器将通知发送到辅助服务器有关区域更新信息。  
6. 辅助服务器进行增量区域传输请求 (IXFR)。 因此，辅助服务器保持同步与主服务器。   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>DNS 主要-辅助部署中的区域作用域级别传送

流量管理方案需要其他步骤才能区域划分为不同的区域作用域。 正因为如此，则将区域作用域中的数据传输到辅助服务器，并将策略和 DNS 客户端子网传输到辅助服务器需要其他步骤。   
  
使用主要和辅助服务器配置 DNS 基础结构后，DNS，使用以下过程自动执行区域作用域级别复制。  
  
若要确保区域作用域级别传输，DNS 服务器，请使用的 DNS (EDNS0) 选择 RR 的扩展机制。 从与作用域区域的所有区域传送 （AXFR 或 IXFR） 请求都源自 EDNS0 选择 RR，其选项 ID 默认情况下设置为"65433"。 有关 EDNSO 的详细信息，请参阅 IETF[征求意见 6891](https://tools.ietf.org/html/rfc6891)。  
  
选择 RR 的值是将请求发送的区域范围名称。 当主 DNS 服务器从受信任的辅助服务器接收此数据包时，它解释为该区域作用域发出的请求。   
  
如果主服务器具有该区域的作用域使用进行了响应传输 (XFR) 数据从该作用域。 响应包含具有相同的选项 ID"65433"选择 RR 和值设置为相同的区域范围。 辅助服务器收到此响应，从响应中，检索的范围信息并更新该特殊作用域的区域。  
  
此过程完成后主服务器维护的受信任的辅助副本已发送此类区域作用域请求通知的列表。   
  
对于区域作用域中的任何进一步更新，IXFR 通知发送到辅助服务器，使用相同的选择 RR。 接收该通知区域作用域可以包含该选择 RR 的 IXFR 请求，并在同一进程按上文所述遵循。  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>如何为主要和辅助地理位置基于流量管理配置 DNS 策略

在开始之前，请确保你已完成所有主题中的步骤[地理位置基于流量管理和主服务器的使用 DNS 策略](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md)，并使用区域，区域作用域，DNS 客户端配置主 DNS 服务器子网和 DNS 策略。  
  
> [!NOTE]
> 本主题中的说明将 DNS 客户端的子网、 区域作用域和 DNS 策略 DNS 主服务器复制到辅助 DNS 服务器是初始 DNS 设置和验证。 在将来可能会想要更改的 DNS 客户端子网、 区域作用域和主服务器上的策略设置。 在这种情况下，可以创建自动化脚本，用于保留与主服务器同步的辅助服务器。  
  
若要配置主要和辅助地理位置基于查询响应的 DNS 策略，必须执行以下步骤。  
  
- [创建辅助区域](#create-the-secondary-zones)  
- [主区域上配置区域复制设置](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [复制 DNS 客户端子网](#copy-the-dns-client-subnets)  
- [在辅助服务器上创建区域作用域](#create-the-zone-scopes-on-the-secondary-server)  
- [配置 DNS 策略](#configure-dns-policy)  
  
以下部分提供详细的配置说明。  
  
> [!IMPORTANT]
> 以下部分包含示例 Windows PowerShell 命令包含多个参数的示例值。 请确保将这些命令中的示例值替换之前运行这些命令适用于你的部署的值为。  
> 
> 中的成员身份**DnsAdmins**，或等效身份所需执行以下过程。  
  
### <a name="create-the-secondary-zones"></a>创建辅助区域

可以创建要复制到 SecondaryServer1 和 SecondaryServer2 的区域的辅助副本 （假设这些 cmdlet 执行远程的单个管理客户端）。   
  
例如，您可以创建 www.woodgrove.com 的辅助副本上 SecondaryServer1 和 SecondarySesrver2。  
  
可以使用以下 Windows PowerShell 命令来创建辅助区域。  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[添加 DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps)。  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>主区域上配置区域复制设置

您必须配置主要区域设置，以便：

1. 允许从主服务器到指定的辅助服务器的区域传送。  
2. 区域更新通知发送到辅助服务器的主服务器。  
  
可以使用以下 Windows PowerShell 命令来配置在主要区域上的区域复制设置。
  
> [!NOTE]
> 在以下示例命令中，参数 **-通知**指定主服务器会将有关更新的通知发送到辅助数据库的选择列表。  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
有关详细信息，请参阅[集 DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps)。  
  
  
### <a name="copy-the-dns-client-subnets"></a>复制 DNS 客户端子网

必须将 DNS 客户端子网从主服务器复制到辅助服务器中。
  
可以使用以下 Windows PowerShell 命令将复制到辅助服务器的子网。
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[添加 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>在辅助服务器上创建区域作用域

必须在辅助服务器上创建区域作用域。 在 DNS 中，区域作用域也开始从主服务器请求 XFRs。 与主服务器上的区域作用域上的任何更改，包含区域作用域信息的通知发送到辅助服务器。 辅助服务器然后可以使用增量更改更新其区域作用域。  
  
可以使用以下 Windows PowerShell 命令在辅助服务器上创建区域作用域。  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

> [!NOTE]
> 在这些示例命令中， **-ErrorAction 忽略**参数会包括在内，因为在每个区域上存在的默认区域作用域。 不能创建或删除默认区域作用域。 管道操作将导致尝试创建该作用域，则会失败。 或者，可以在两个辅助区域上创建非默认区域作用域。  
  
有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="configure-dns-policy"></a>配置 DNS 策略

创建子网后，分区 （区域作用域），并且您已将记录添加，则必须创建连接的子网和分区的策略，因此，如果查询来自 DNS 客户端子网之一中的源，从返回的查询响应区域的正确作用域。 没有策略所需的映射的默认区域作用域。  
  
可以使用以下 Windows PowerShell 命令创建 DNS 客户端子网链接和区域作用域的 DNS 策略。   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在，使用所需的 DNS 策略，基于地理位置的流量重定向配置辅助 DNS 服务器。  
  
当 DNS 服务器收到名称解析查询时，DNS 服务器的计算结果中针对配置的 DNS 策略对 DNS 请求的字段。 如果名称解析请求中的源 IP 地址与匹配任何策略，关联的区域作用域用于响应查询，并将用户定向到地理上接近它们的资源。   
  
您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。
