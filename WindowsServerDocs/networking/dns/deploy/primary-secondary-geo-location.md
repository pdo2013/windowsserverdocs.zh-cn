---
title: 地理位置的使用 DNS 策略基于交通管理与主要辅助部署
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c78a0198e29fb59f30fd8ad776c7f200312d014
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>地理位置的使用 DNS 策略基于交通管理与主要辅助部署

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何创建 DNS 策略地理位置基于交通管理时 DNS 部署包含主要和辅助 DNS 服务器。  

以前的情况下，[使用与主要服务器的地理位置基于交通管理 DNS 策略](primary-geo-location.md)，提供配置为的地理位置基于交通管理 DNS 策略主 DNS 服务器上的说明进行操作。 在 Internet 基础结构，但是，DNS 服务器程序广泛地部署在其中选择和安全主服务器上存储可写的区域的副本，只读的区域副本保存在多个次要服务器的主要辅助模型。   
  
辅助服务器使用权威传输 (AXFR) 和增量区域转移 (IXFR) 的区域传输协议请求和接收区域的更新，包括对区域的主 DNS 服务器上的新更改。   
  
>[!NOTE]
>有关 AXFR 的详细信息，请参阅 Internet 工程工作组 (IETF)[征求评论 5936](https://tools.ietf.org/rfc/rfc5936.txt)。 有关 IXFR 的详细信息，请参阅 Internet 工程工作组 (IETF)[征求评论 1995 年](https://tools.ietf.org/html/rfc1995)。  
  
## <a name="bkmk_example"></a>主要辅助地理位置基于交通管理示例  
以下是如何主要辅助部署中使用 DNS 策略实现执行 DNS 查询的客户端的物理位置根据交通重定向的一个示例。  
  
此示例中使用两个虚构的公司 Contoso 云服务，该法案 web 和域举办解决方案;和 Woodgrove 食物服务，其中提供美食传递城市的多个全球和服务方面的优化调整网站命名 woodgrove.com。  
  
若要确保 woodgrove.com 客户能够响应体验从其网站，Woodgrove 想定向到欧洲 datacenter 欧洲的客户端和定向到美国 datacenter 的美国客户端。 在世界位于其他位置的客户可以将定向到任一数据中心。  
  
Contoso 云服务都有一个在美国，另一个欧洲，在其 Contoso 承载其餐 portal 用于进行 woodgrove.com 两个数据中心。  
  
Contoso DNS 部署包括两次服务器：**SecondaryServer1**的 IP 地址 10.0.0.2;和**SecondaryServer2**，10.0.0.3 的 IP 地址。 这些辅助服务器充当名称服务器在两种不同的地区，与 SecondaryServer1 位于欧洲和 SecondaryServer2 居住在美国
  
上没有可写的主区域副本**PrimaryServer**（的 IP 地址 10.0.0.1）进行区域的更改。 常规区域传输到第二服务器，辅助服务器是始终保持最新的到 PrimaryServer 上区域任何新更改。
  
下图显示了这种情况。
  
![主要辅助地理位置基于交通管理示例](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="bkmk_works"></a>DNS 主要辅助系统的工作原理

地理位置基于主要辅助 DNS 部署的交通管理部署时，务必了解如何正常换乘早了解区域范围级别传输的主要辅助区域。 以下部分区域和区域范围级别传输提供的信息。  
  
- [在主要辅助 DNS 部署区域传输](#bkmk_zone)  
- [区域范围级别传输 DNS 主要辅助部署](#bkmk_scope)  
  
### <a name="bkmk_zone"></a>在主要辅助 DNS 部署区域传输

你可以创建 DNS 主要辅助部署和同步区域，通过以下步骤。  
1. 当你安装 DNS 时的主区域创建主 DNS 服务器上。  
2. 在第二服务器上，创建分区，并指定的主服务器。   
3. 在主服务器上，你可以添加辅助服务器为受信任的主区域辅助。   
4. 辅助区域完整区域传输请求 (AXFR)，并且收到区域的副本。   
5. 在需要时，主要服务器将发送到第二有关区域更新服务器通知。  
6. 辅助服务器使增量区域转移请求 (IXFR)。 出于此原因，辅助服务器保持同步与主服务器。   
  
### <a name="bkmk_scope"></a>区域范围级别传输 DNS 主要辅助部署

路况管理方案要求到了不同的区域范围分区区域额外步骤。 出于此原因，还需要区域范围内的数据传输到第二服务器，并将策略和 DNS 客户端子网传输到第二服务器其他步骤。   
  
为你 DNS 基础结构配置主要和次要服务器后，DNS，使用以下过程会自动执行区域范围级别传输。  
  
若要确保区域范围级别传输、DNS 服务器使用扩展机制 (EDNS0) DNS 选择 RR。 EDNS0 选择 RR，其选项 ID 设置为"65433"默认情况下使用来自所有从范围的时区的区域（AXFR 或 IXFR）传输请求。 有关 EDNSO 的详细信息，请参阅 IETF[征求评论 6891](https://tools.ietf.org/html/rfc6891)。  
  
选择 RR 值为区域范围名为其发送该请求。 当主 DNS 服务器接收来自受信任的辅助服务器的此数据包时，它将解释作为有关该区域范围请求。   
  
主服务器具有该区域范围它是否响应 (XFR) 传输数据的此范围内。 响应包含具有相同的选项 ID"65433"中选择 RR 和值设置为相同的时区范围。 辅助服务器接收此响应、响应，从检索范围信息和更新该特定的范围内的区域。  
  
在此过程中之后, 主服务器维护受信任的映像，将它已发送的通知的此类区域范围请求的列表。   
  
对于区域范围内的任何进一步更新，IXFR 通知将发送到具有相同的选择 RR 辅助服务器。 接收该通知区域范围使包含该选择 RR IXFR 请求和相同的过程，如上所述遵循。  
  
## <a name="bkmk_config"></a>如何为主要辅助地理位置基于交通管理配置 DNS 策略

在开始之前，确保你已完成所有主题中的步骤[使用与主要服务器的地理位置基于交通管理 DNS 策略](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md)，并通过区域、区域范围、客户端 DNS 子网和 DNS 策略配置你的主 DNS 服务器。  
  
>[!NOTE]
> 本主题中的说明将 DNS 客户端子网区域范围，DNS 策略 DNS 主服务器复制到第二的 DNS 服务器是初始 DNS 设置和验证。 将来你可能想要更改 DNS 客户端子网、区域范围和主服务器上的策略设置。 在此情况下，你可以创建自动化脚本保持同步与主服务器辅助服务器。  
  
若要配置基于主要辅助地理位置查询响应 DNS 策略，你必须执行以下步骤。  
  
- [创建辅助区域](#bkmk_secondary)  
- [配置的主区域的时区传输设置](#bkmk_zonexfer)  
- [复制 DNS 客户端子网](#bkmk_client)  
- [在服务器上辅助创建区域范围](#bkmk_zonescopes)  
- [配置 DNS 策略](#bkmk_dnspolicy)  
  
以下部分提供了详细的配置说明进行操作。  
  
>[!IMPORTANT]
>以下部分包括包含许多参数示例值的示例 Windows PowerShell 命令。 确保你在运行以下命令之前就提供适用于你的部署的值与替换示例值在这些命令。  
><br>在会员**DnsAdmins**，或等效，需要执行以下步骤。  
  
### <a name="bkmk_secondary"></a>创建辅助区域

您可以创建您要复制到 SecondaryServer1 和 SecondaryServer2 区域辅助副本（假定 cmdlet 正在执行远程从单个管理客户端）。   
  
例如，你可以 SecondaryServer1 和 SecondarySesrver2 创建 www.woodgrove.com 次要副本。  
  
以下 Windows PowerShell 命令可用于创建次要的区域。  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[添加 DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps)。  
  
### <a name="bkmk_zonexfer"></a>配置的主区域的时区传输设置

你必须配置的主区域设置，以便：

1. 允许从主服务器的区域转移到次要指定的服务器。  
2. 区域更新通知由主服务器发送到第二服务器。  
  
你可以使用下面的 Windows PowerShell 命令配置上的主区域的时区传输设置。
  
>[!NOTE]
>在下面的示例命令参数**-通知**指定的主服务器，将向选择列表中辅助发送有关更新的通知。  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
有关详细信息，请参阅[组 DnsServerPrimaryZone](https://https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps)。  
  
  
### <a name="bkmk_client"></a>复制 DNS 客户端子网

辅助服务器，必须从主服务器复制 DNS 客户端个子网。
  
可以使用下面的 Windows PowerShell 命令辅助服务器复制子网。
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[添加 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="bkmk_zonescopes"></a>在服务器上辅助创建区域范围

你必须在第二服务器上创建区域范围。 在 DNS，区域范围还开始请求 XFRs 从主服务器。 在主服务器上的区域范围的任何更改，包含区域范围信息的通知发给辅助服务器。 辅助服务器可以更新与增量更改其时区范围。  
  
以下 Windows PowerShell 命令用于辅助服务器上创建区域范围。  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>这些示例命令，在**-ErrorAction 忽略**参数是包含，因为在每个区域的默认区域范围存在。 默认区域范围无法创建或删除。 管道将导致创建该范围尝试，它将失败。 或者，你可以在两个辅助区域创建区域非默认范围。  
  
有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="bkmk_dnspolicy"></a>配置 DNS 策略

创建子网后，分区（区域范围），并且你已添加的记录，你必须创建子网和分区中，连接的策略，以便当查询来源之一 DNS 客户端子网，查询响应从正确的范围内的区域。 没有策略所需的映射默认区域范围。  
  
以下 Windows PowerShell 命令可用于创建 DNS 策略，该链接 DNS 客户端子网和区域范围。   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在辅助 DNS 服务器具有所需的 DNS 策略重定向通信根据地理位置配置。  
  
当 DNS 服务器收到名称分辨率查询时，DNS 服务器评估配置 DNS 策略防御 DNS 请求中的字段。 名称分辨率请求中的源 IP 地址与匹配任何策略，如果关联的区域范围用于响应查询，并且用户定向到地理位置与他们最近的资源。   
  
你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。
