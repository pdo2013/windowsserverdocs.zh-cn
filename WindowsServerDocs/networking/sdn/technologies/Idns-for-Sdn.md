---
title: SDN 的内部 DNS 服务 (iDNS)
description: 本主题说明如何提供 DNS 服务为托管的租户工作负荷使用内部 DNS (Idn) 与 Windows Server 2016 中软件定义的网络集成在一起。
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
ms.openlocfilehash: 4d4ae5ee5f5600d86349ca26b7acbdb284b45bac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824078"
---
# <a name="internal-dns-service-idns-for-sdn"></a>SDN 的内部 DNS 服务 (iDNS)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

如果您为云服务提供商工作\(CSP\)或规划以部署软件定义的网络的企业\(SDN\)在 Windows Server 2016 中，您可以为托管的租户工作负荷提供 DNS 服务通过使用内部 DNS \(Idn\)，这与 SDN 集成。

托管虚拟机\(Vm\)和应用程序需要 DNS 中他们自己的网络和 Internet 上的外部资源进行通信。 使用 Idn，可以提供租户与 DNS 名称解析服务为其独立的本地命名空间和对 Internet 资源。

因为 Idn 服务不能从租户虚拟网络访问，而不通过 Idn 代理服务器不是易受攻击租户网络上的恶意活动。

**主要功能**

以下是使用 idn 的主要功能。

- 提供共享的 DNS 名称解析服务的租户工作负荷
- 名称解析和 DNS 注册的租户名称空间中的权威 DNS 服务
- 从租户 Vm Internet 名称解析的递归 DNS 服务。
- 如果需要，可以配置的结构和租户名称的同时进行托管
- 经济高效的 DNS 解决方案的租户不需要部署自己的 DNS 基础结构
- 通过 Active Directory 集成，需要的高可用性。

除了这些功能，如果您担心如何随时对 AD 集成的 DNS 服务器打开到 Internet，可以部署在外围网络中的另一个递归解析程序背后的 Idn 服务器。

IDNS 是集中式的服务器对所有 DNS 查询，因为 CSP 或企业版可以还实施租户 DNS 防火墙、 应用筛选器、 检测恶意活动和审核在中央位置的事务

## <a name="idns-infrastructure"></a>iDNS 基础结构
IDNS 基础结构包括 Idn 服务器和 Idn 代理。

### <a name="idns-servers"></a>Idn 服务器
iDNS 包括一组托管特定于租户的数据，例如 VM 的 DNS 资源记录的 DNS 服务器。

Idn 服务器是其内部的 DNS 区域的权威服务器，同时还充当冲突解决程序的公共名称时租户 Vm 尝试连接到外部资源。

所有虚拟网络上的 Vm 的主机名都在同一个区域下存储作为 DNS 资源记录。 例如，如果部署名为 contoso.local 区域的 iDNS，该网络上 Vm 的 DNS 资源记录存储在 contoso.local 区域中。

租户 VM 完全限定的域名\(Fqdn\)对于虚拟网络，采用 GUID 格式包含计算机名称和 DNS 后缀字符串。 例如，如果你已有租户 VM 名为位于本地，虚拟网络 contoso TENANT1 VM 的 FQDN 是 TENANT1。*vn guid*。 contoso.local，其中*vn guid*是为虚拟网络的 DNS 后缀字符串。

>[!NOTE]
>如果你是构造管理员，你可以使用 CSP 或企业 DNS 基础结构，如 Idn 服务器而不是部署新的 DNS 服务器专门为使用 Idn 作为服务器。 无论你部署中使用 idn 的新服务器还是使用现有的基础结构，Idn 依赖于 Active Directory 来提供高可用性。 因此必须与 Active Directory 集成 Idn 服务器。

### <a name="idns-proxy"></a>iDNS 代理
iDNS 代理是在每台主机上运行并且该租户虚拟网络 DNS 将流量转发到 Idn 服务器的 Windows 服务。

下图描绘了中通过 Idn 服务器的 Idn 代理租户虚拟网络和 Internet 的 DNS 通信路径。

![iDNS 基础结构](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>如何部署 Idn
当使用脚本部署 Windows Server 2016 中的 SDN 时，Idn 是自动包含在你的部署。

有关详细信息，请参阅以下主题。

- [部署软件定义的网络基础结构使用脚本](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>了解部署步骤的 iDNS
本部分中可用于深入了解有关如何安装和配置部署 SDN 使用脚本时 Idn。

下面是部署 Idn 所需的步骤的摘要。

>[!NOTE]
>如果已通过使用脚本部署 SDN，你不需要执行任何这些步骤。 有关信息和故障排除目的提供的步骤。

### <a name="step-1-deploy-dns"></a>第 1 步：部署 DNS
可以使用下面的示例 Windows PowerShell 命令来部署 DNS 服务器。
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>步骤 2：配置网络控制器中的 Idn 信息
此脚本段是由管理员向网络控制器，告知有关 Idn 区域配置-例如 iDNSServer 和用于承载 Idn 名称的区域的 IP 地址的 REST 调用。 

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
>这是一段摘录部分**配置 ConfigureIDns** SDNExpress.ps1 中。 有关详细信息，请参阅[部署软件定义的网络基础结构使用脚本](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-3-configure-the-idns-proxy-service"></a>步骤 3:配置 Idn 代理服务
IDNS 代理服务运行在每个 HYPER-V 主机，提供租户的虚拟网络和 Idn 服务器所在的物理网络之间的桥梁。 必须在每个 HYPER-V 主机上创建以下注册表项。


**DNS 端口：** 固定的端口 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "Port"
- ValueData = 53
- ValueType = "Dword"
       

**DNS 代理端口：** 固定的端口 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**DNS IP:** 在租户选择使用 Idn 服务的情况下，网络接口上配置的固定的 IP 地址

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType ="String"

        
**Mac 地址：** DNS 服务器的媒体访问控制地址

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = “aa-bb-cc-aa-bb-cc”
- ValueType ="String"

**IDN 服务器地址：** 以逗号分隔 Idn 服务器的列表。

- 注册表项：HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "Forwarders"
- ValueData = “10.0.0.9”
- ValueType ="String"



>[!NOTE]
>这是一段摘录部分**配置 ConfigureIDnsProxy** SDNExpress.ps1 中。 有关详细信息，请参阅[部署软件定义的网络基础结构使用脚本](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>步骤 4：重新启动网络控制器主机代理服务
可以使用以下 Windows PowerShell 命令以重新启动网络控制器主机代理服务。
    
    Restart-Service nchostagent -Force
    
有关详细信息，请参阅[重新启动服务](https://technet.microsoft.com/library/hh849823.aspx)。

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>启用 DNS 代理服务的防火墙规则
可以使用以下 Windows PowerShell 命令来创建防火墙规则允许要与 VM 和 Idn 服务器通信的代理的异常。
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

有关详细信息，请参阅[Enable-netfirewallrule](https://technet.microsoft.com/library/jj554869.aspx)。
    
### <a name="validate-the-idns-service"></a>验证 Idn 服务
若要验证 Idn 服务，必须部署示例租户工作负荷。

有关详细信息，请参阅[创建 VM 和连接到租户虚拟网络或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。

如果所需的租户 VM 使用 Idn 服务，必须将 VM 网络接口的 DNS 服务器配置保留为空，并允许为使用 DHCP 的接口。 

此类网络接口的 VM 启动后，会自动获得的配置，使 VM 可以使用 Idn，和在 VM 立即开始使用 Idn 服务执行名称解析。

如果配置的租户 VM 使用 Idn 服务通过将网络接口的 DNS 服务器和备用 DNS 服务器信息保留为空白，网络控制器 VM 提供 IP 地址，并代表具有 Idn 服务器 VM 执行 DNS 名称注册. 

网络控制器还告知 Idn 代理 VM 和所需的详细信息，以对 VM 执行名称解析。 

当 VM 启动时的 DNS 查询时，该代理充当转发器从虚拟网络对 Idn 服务的查询。 

DNS 代理还可确保租户 VM 查询隔离。 如果查询权威 Idn 服务器，则 Idn 服务器响应权威响应。 如果 Idn 服务器不是查询权威的它将执行 DNS 递归解析 Internet 名称。

>[!NOTE]
>此信息包含的部分中**配置 AttachToVirtualNetwork** SDNExpressTenant.ps1 中。 有关详细信息，请参阅[部署软件定义的网络基础结构使用脚本](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

