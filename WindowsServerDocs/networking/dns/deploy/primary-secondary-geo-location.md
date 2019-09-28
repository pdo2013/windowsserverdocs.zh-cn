---
title: 使用针对基于地理位置的流量管理和主要-辅助部署的 DNS 策略
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a7836160fc7363ec3d7b2fb11e194db82970f9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406153"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>使用针对基于地理位置的流量管理和主要-辅助部署的 DNS 策略

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题了解当 DNS 部署同时包含主 DNS 服务器和辅助 DNS 服务器时，如何为基于地理位置的流量管理创建 DNS 策略。  

前面的方案中，[使用 Dns 策略对主服务器进行基于地理位置的流量管理](primary-geo-location.md)，提供了有关在主 DNS 服务器上配置基于地理位置的流量管理的 dns 策略的说明。 然而，在 Internet 基础结构中，DNS 服务器是在主-辅助模型中广泛部署的，在该模型中，区域的可写副本存储在 select 和 secure 主服务器上，而区域的只读副本保留在多个辅助服务器上。   
  
辅助服务器使用区域传输协议权威传输（AXFR）和增量区域传输（IXFR）来请求和接收区域更新，其中包括对主 DNS 服务器上的区域的新更改。   
  
> [!NOTE]
> 有关 AXFR 的详细信息，请参阅 Internet 工程任务组（IETF）[征求意见 5936](https://tools.ietf.org/rfc/rfc5936.txt)。 有关 IXFR 的详细信息，请参阅 Internet 工程任务组（IETF）[征求意见 1995](https://tools.ietf.org/html/rfc1995)。  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>主要-辅助异地位置流量管理示例  
下面的示例演示了如何在主辅助部署中使用 DNS 策略，以根据执行 DNS 查询的客户端的物理位置实现流量重定向。  
  
此示例使用两个虚构的公司： Contoso 云服务，提供 web 和域托管解决方案;和 Woodgrove 食物服务，在全球多个城市提供食物交付服务，其中有一个名为 woodgrove.com 的网站。  
  
为了确保 woodgrove.com 客户获得其网站的响应体验，Woodgrove 希望将欧洲的客户定向到将欧洲数据中心和美国的客户定向到美国数据中心。 位于世界各地的客户可以定向到其中一个数据中心。  
  
Contoso 云服务有两个数据中心，一个位于美国，另一个在欧洲，而另一个在欧洲，Contoso 为 woodgrove.com 托管其食品定购门户。  
  
Contoso DNS 部署包括两个辅助服务器：**SecondaryServer1**，IP 地址为 10.0.0.2;IP 地址为10.0.0.3 的和**SecondaryServer2**。 这些辅助服务器充当两个不同区域中的名称服务器，SecondaryServer1 位于欧洲，SecondaryServer2 位于美国
  
在**PrimaryServer** （IP 地址10.0.0.1）上有一个主要的可写区域副本，在其中进行区域更改。 对于到辅助服务器的常规区域传输，辅助服务器始终是最新的，并且对 PrimaryServer 上的区域进行了任何新的更改。
  
下图描述了此方案。
  
![主要-辅助异地位置流量管理示例](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>DNS 主-辅助系统的工作原理

在主-辅助 DNS 部署中部署基于地理位置的流量管理时，了解在了解区域范围级别传输之前如何进行一般的主要辅助区域传输是非常重要的。 以下各节提供了有关区域和区域范围级别传输的信息。  
  
- [在 DNS 主-辅助部署中进行区域传输](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [DNS 主-辅助部署中的区域作用域级别传输](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>在 DNS 主-辅助部署中进行区域传输

你可以使用以下步骤创建 DNS 主要辅助部署和同步区域。  
1. 安装 DNS 时，会在主 DNS 服务器上创建主区域。  
2. 在辅助服务器上，创建区域并指定主服务器。   
3. 在主服务器上，可以将辅助服务器作为受信任的辅助服务器添加到主区域。   
4. 辅助区域会进行完整的区域传输请求（AXFR）并接收区域的副本。   
5. 如果需要，主服务器会将通知发送到辅助服务器上有关区域更新的信息。  
6. 辅助服务器会进行增量区域传输请求（IXFR）。 因此，辅助服务器仍与主服务器保持同步。   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>DNS 主-辅助部署中的区域作用域级别传输

流量管理方案需要额外的步骤将区域分区到不同的区域作用域中。 因此，需要执行额外的步骤将区域范围内的数据传输到辅助服务器，并将策略和 DNS 客户端子网传输到辅助服务器。   
  
使用主服务器和辅助服务器配置 DNS 基础结构后，将使用以下进程自动执行区域范围级别的传输。  
  
为了确保区域作用域级别的传输，DNS 服务器使用 DNS （EDNS0） OPT RR 的扩展机制。 具有作用域的区域中的所有区域传送（AXFR 或 IXFR）请求都源自 EDNS0 OPT RR，默认情况下，其选项 ID 设置为 "65433"。 有关 EDNSO 的详细信息，请参阅 IETF[请求注释 6891](https://tools.ietf.org/html/rfc6891)。  
  
OPT RR 的值是要向其发送请求的区域范围名称。 主 DNS 服务器从受信任的辅助服务器接收此数据包时，会将该请求解释为适用于该区域作用域的请求。   
  
如果主服务器具有该区域作用域，则它会通过该作用域中的传输（XFR）数据进行响应。 响应包含具有相同选项 ID "65433" 且值设置为相同区域作用域的 OPT RR。 辅助服务器接收此响应，从响应中检索范围信息，并更新区域的特定作用域。  
  
完成此过程后，主服务器将维护已发送此类区域范围请求通知的受信任辅助副本的列表。   
  
对于区域作用域中的任何进一步更新，会将 IXFR 通知发送到辅助服务器，并且具有相同的 OPT RR。 接收该通知的区域作用域会使 IXFR 请求包含该 OPT RR，并按上述步骤操作，如下所述。  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>如何配置基于主要-辅助地理位置的流量管理的 DNS 策略

在开始之前，请确保已完成 "[将 DNS 策略用于基于地理位置的流量管理和主服务器](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md)" 主题中的所有步骤，并且为主 DNS 服务器配置了区域、区域作用域、Dns 客户端子网和 DNS政策.  
  
> [!NOTE]
> 本主题中的说明将 dns 主服务器中的 DNS 客户端子网、区域作用域和 DNS 策略复制到 DNS 辅助服务器，用于初始 DNS 设置和验证。 将来，你可能想要更改主服务器上的 DNS 客户端子网、区域作用域和策略设置。 在这种情况下，你可以创建自动化脚本来使辅助服务器与主服务器保持同步。  
  
若要为基于主要辅助地理位置的查询响应配置 DNS 策略，必须执行以下步骤。  
  
- [创建辅助区域](#create-the-secondary-zones)  
- [在主要区域上配置区域传送设置](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [复制 DNS 客户端子网](#copy-the-dns-client-subnets)  
- [在辅助服务器上创建区域作用域](#create-the-zone-scopes-on-the-secondary-server)  
- [配置 DNS 策略](#configure-dns-policy)  
  
以下各节提供了详细的配置说明。  
  
> [!IMPORTANT]
> 以下各节包含示例 Windows PowerShell 命令，其中包含许多参数的示例值。 在运行这些命令之前，请确保将这些命令中的示例值替换为适用于你的部署的值。  
> 
> 若要执行以下过程，需要**DnsAdmins**中的成员身份或同等身份。  
  
### <a name="create-the-secondary-zones"></a>创建辅助区域

你可以创建要复制到 SecondaryServer1 和 SecondaryServer2 的区域的次要副本（假设 cmdlet 是从单个管理客户端远程执行的）。   
  
例如，可以在 SecondaryServer1 和 SecondarySesrver2 上创建 www.woodgrove.com 的辅助副本。  
  
你可以使用以下 Windows PowerShell 命令创建辅助区域。  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps)。  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>在主要区域上配置区域传送设置

你必须配置主区域设置，以便：

1. 允许从主服务器到指定辅助服务器的区域传输。  
2. 主服务器将区域更新通知发送到辅助服务器。  
  
你可以使用以下 Windows PowerShell 命令在主要区域上配置区域传输设置。
  
> [!NOTE]
> 在下面的示例命令中，参数**通知**指定主服务器将向辅助副本的选择列表发送有关更新的通知。  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
有关详细信息，请参阅[add-dnsserverprimaryzone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps)。  
  
  
### <a name="copy-the-dns-client-subnets"></a>复制 DNS 客户端子网

必须将 DNS 客户端子网从主服务器复制到辅助服务器。
  
你可以使用以下 Windows PowerShell 命令将子网复制到辅助服务器。
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>在辅助服务器上创建区域作用域

你必须在辅助服务器上创建区域作用域。 在 DNS 中，区域范围还开始从主服务器请求 XFRs。 在主服务器上对区域作用域进行任何更改后，会将包含区域作用域信息的通知发送到辅助服务器。 然后，辅助服务器可以通过增量更改来更新其区域作用域。  
  
你可以使用以下 Windows PowerShell 命令在辅助服务器上创建区域作用域。  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

> [!NOTE]
> 在这些示例命令中，将包含 **-ErrorAction Ignore**参数，因为每个区域中都存在一个默认区域作用域。 不能创建或删除默认区域作用域。 管道将导致尝试创建该作用域，并且它将失败。 另外，还可以在两个辅助区域上创建非默认区域作用域。  
  
有关详细信息，请参阅[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="configure-dns-policy"></a>配置 DNS 策略

创建子网、分区（区域作用域）并添加记录后，你必须创建用于连接子网和分区的策略，以便在查询来自某个 DNS 客户端子网中的源时，从区域的正确范围。 映射默认区域作用域不需要策略。  
  
你可以使用以下 Windows PowerShell 命令创建一个 DNS 策略，用于链接 DNS 客户端子网和区域作用域。   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在，辅助 DNS 服务器配置了必需的 DNS 策略，以根据地理位置重定向流量。  
  
当 DNS 服务器收到名称解析查询时，DNS 服务器会根据配置的 DNS 策略评估 DNS 请求中的字段。 如果名称解析请求中的源 IP 地址与任何策略相匹配，则会使用关联的区域作用域来响应查询，并将用户定向到离它们最近的资源。   
  
你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。
