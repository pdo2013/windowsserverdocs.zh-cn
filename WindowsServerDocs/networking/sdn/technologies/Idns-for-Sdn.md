---
title: SDN 的内部 DNS 服务（idn，则）
description: 本主题介绍如何可以提供 DNS 服务向您托管的租户工作负载通过内部 DNS （idn，则） 软件定义的 Windows Server 2016 网络的集成。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4bdb495b585cf60315dee60f9365e282877fdc1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="internal-dns-service-idns-for-sdn"></a>内部 DNS 为 SDN 服务 \(iDNS\)

>适用于：Windows Server（半年通道），Windows Server 2016

如果你的工作计划部署 Windows Server 2016 中的软件定义网络 \(SDN\) 的企业版或云服务提供商 \(CSP\)，您可以通过内部 DNS \(iDNS\)，与 SDN 是已集成到你的托管的租户工作负载提供 DNS 服务。

托管的虚拟机 \(VMs\) 和应用程序需要 DNS 内自己网络和 Internet 上的外部资源进行通信。 与 idn，则，你可以与 DNS 名称分辨率服务提供租户对其独立、 当地名称空间和 Internet 资源。

Idn，则服务不可从租户虚拟网络的访问，因为以外的其他通过 idn，则的代理服务器不可受到恶意租户网络上的活动。

**主要的功能**

以下是有关 idn，则主要的功能。

- 提供共享的 DNS 名称分辨率服务租户工作负载
- 为名称分辨率和在租户命名空间中的 DNS 注册权威 DNS 服务
- Internet 名称来源租户虚拟机的功能的分辨率的递归 DNS 服务。
- 如果需要，你可以配置同时举办结构和租户名称
- 成本效益的 DNS 解决方案-租户无需自己 DNS 基础结构的部署
- 使用 Active Directory 集成，才能高发布。

除了这些功能，如果你担心保留您的广告集成 DNS 服务器打开到 Internet，你可以将部署后面外围网络中的另一个递归解析程序 idn，则服务器。

因为 idn，则所有 DNS 查询的集中式的服务器，CSP 或企业可以还实施租户 DNS 防火墙、 应用筛选器、 检测到恶意的活动和审核交易在中心位置

## <a name="idns-infrastructure"></a>idn 基础结构，则
Idn，则基础结构包括 idn，则服务器和 idn，则代理。

### <a name="idns-servers"></a>idn，则服务器
idn，则包含一组的租户特定数据，如 VM DNS 资源记录 DNS 服务器。

他们内部 DNS 区域，权威服务器和还充当公共名称解析程序 idn，则服务器时租户 Vm 尝试连接到外部资源。

所有虚拟网络虚拟机的功能主机名称下同一区域作为 DNS 资源记录存储。 例如，如果部署 idn contoso.local 名为某个区域，则，该网络上 Vm DNS 资源记录存储 contoso.local 区域中。

租户 VM 完全合格的域名 \(FQDNs\) 包括计算机名称和 DNS 职务字符串虚拟网络，以 GUID 格式。 例如，如果你有一个租户 VM 命名 TENANT1 本地、 虚拟网络 contoso 上 VM FQDN 是 TENANT1。*vn guid*。 contoso.local，其中*vn guid*是 DNS 职务字符串虚拟网络。

>[!NOTE]
>如果你是结构管理员，你可以使用您 CSP 或企业 DNS 的基础结构，由于 idn，则服务器，而不是部署新 DNS 服务器专为使用 idn，则作为服务器。 还是部署新服务器 idn，则你可以使用现有的基础结构，idn，则依赖于 Active Directory 提供高发布。 因此必须使用 Active Directory 集成 idn，则服务器。

### <a name="idns-proxy"></a>idn，则代理服务器
idn，则代理是 Windows 服务在每个主机，运行和其租户虚拟网络 DNS 将通讯转发到 idn，则服务器。

下图显示了 DNS 交通路径从通过到 idn 服务器，则 idn，则代理租户虚拟网络和 Internet。

![idn 基础结构，则](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>如何部署 idn，则
当您使用脚本部署 Windows Server 2016 中的 SDN 时，idn，则自动包含在你部署。

有关详细信息，请参阅以下主题。

- [部署软件定义的网络基础结构使用脚本](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>了解 idn，则部署步骤
可以使用本部分以了解如何安装和配置时部署 SDN 使用脚本 idn，则。

以下是需部署 idn，则的步骤的摘要。

>[!NOTE]
>如果您使用脚本的部署 SDN，你不需要执行这些步骤。 步骤提供的信息和疑难解答的用途。

### <a name="step-1-deploy-dns"></a>第 1 步： 部署 DNS
你可以使用下面的示例 Windows PowerShell 命令部署 DNS 服务器。
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>第 2 步： 网络控制器在配置 idn，则信息
此脚本段是其余调用程序由管理员到网络控制器、 通知它有关 idn，则区域配置-例如 iDNSServer 和用于主机 idn，则名区域的 IP 地址。 

```
    Url: https://<url>/networking/v1/iDnsServer/configuration
Method: PUT
{
      "properties": {
        "connections": [
          {
            "managementAddresses": [
              "10.0.0.9"
            ],
            "credential": {
              "resourceRef": "/credentials/iDnsServer-Credentials"
            },
            "credentialType": "usernamePassword"
          }
        ],
        "zone": "contoso.local"
      }
    }
```

>[!NOTE]
>这是了部分中的摘要**配置 ConfigureIDns** SDNExpress.ps1 中。 有关详细信息，请参阅[部署使用脚本软件定义的网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-3-configure-the-idns-proxy-service"></a>第 3 步： 配置 idn 代理服务则
Idn 代理服务，则运行每提供这座大桥租户虚拟网络之间的物理 idn，则服务器所在的网络 HYPER-V 主机上。 必须在每个 HYPER-V 主机上创建以下注册表项。


**DNS 端口：**修复 53 端口

- 注册表项 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- 数值名称 ="端口"
- ValueData = 53
- 值 ="Dword"
       

**DNS 代理服务器端口：**修复 53 端口

- 注册表项 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- 数值名称 ="ProxyPort"
- ValueData = 53
- 值 ="Dword"
        
**DNS IP:**选择使用 idn，则服务租户的情况下，这是固定的网络接口上, 配置的 IP 地址。

- 注册表项 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- 数值名称 ="IP"
- ValueData ="169.254.169.254"
- 值 ="字符串"

        
**Mac 地址：**媒体访问控制 DNS 服务器地址

- 注册表项 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- 数值名称 ="MAC"
- ValueData ="aa-bb-cc-aa-bb-cc"
- 值 ="字符串"

**Idn，则服务器的地址：**逗号分隔 idn 服务器，则的列表。

- 注册表项： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- 数值名称 ="转发器"
- ValueData ="10.0.0.9"
- 值 ="字符串"



>[!NOTE]
>这是了部分中的摘要**配置 ConfigureIDnsProxy** SDNExpress.ps1 中。 有关详细信息，请参阅[部署使用脚本软件定义的网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>步骤 4： 重启网络控制器主机代理服务
你可以使用下面的 Windows PowerShell 命令重启网络控制器主机代理服务。
    
    Restart-Service nchostagent -Force
    
有关详细信息，请参阅[重启服务](https://technet.microsoft.com/library/hh849823.aspx)。

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>启用了防火墙规则 DNS 代理服务
以下 Windows PowerShell 命令用于创建允许与 VM 和 idn，则 server 通信的代理服务器的异常防火墙规则。
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

有关详细信息，请参阅[启用 NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx)。
    
### <a name="validate-the-idns-service"></a>验证 idn，则服务
若要验证 idn 服务，则，必须部署示例租户工作负载。

有关详细信息，请参阅[创建 VM 和连接到租户虚拟网络或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。

如果你希望租户 VM 使用 idn，则服务，必须将 VM 网络接口 DNS 服务器配置留空，并允许接口若要使用 DHCP。 

与网络接口 VM 启动后，它将自动接收允许使用 idn，则，VM 配置并 VM 立即开始使用 idn，则服务执行名称分辨率。

配置租户 VM 使用 idn，则服务通过将网络接口 DNS 服务器和备用 DNS 服务器信息留空，如果网络控制器 VM 提供 IP 地址，并执行代表 idn 服务器，则使用 VM DNS 名称注册。 

网络控制器还会告知 idn，则代理 VM 以及所需的详细信息的 VM 执行名称分辨率。 

VM 启动 DNS 查询，代理作为转发器的从虚拟网络查询到 idn，则服务。 

DNS 代理还可以确保租户 VM 查询独立。 如果 idn，则服务器权威查询，idn，则服务器响应权威的响应。 如果不权威查询 idn，则服务器，它执行 DNS 递归解析 Internet 名称。

>[!NOTE]
>此信息包含在部分**配置 AttachToVirtualNetwork** SDNExpressTenant.ps1 中。 有关详细信息，请参阅[部署使用脚本软件定义的网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

