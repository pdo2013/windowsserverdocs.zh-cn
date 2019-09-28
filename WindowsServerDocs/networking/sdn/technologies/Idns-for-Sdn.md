---
title: SDN 的内部 DNS 服务 (iDNS)
description: 本主题说明如何使用内部 DNS （Idn）将 DNS 服务提供给托管的租户工作负荷，该内部 DNS 与 Windows Server 2016 中的软件定义网络集成。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a7e5aa9e1ae7442c706c1bdbdb56d65234fe5ae8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405970"
---
# <a name="internal-dns-service-idns-for-sdn"></a>SDN 的内部 DNS 服务 (iDNS)

>适用于：Windows Server（半年频道）、Windows Server 2016

如果你使用的是云服务提供商 @no__t-在 Windows Server 2016 中计划部署软件定义的网络 \(SDN @ no__t，则可以通过使用内部 DNS 向托管的租户工作负荷提供 DNS 服务 @no__4iDNS @ no__t-5，它与 SDN 集成。

托管虚拟机 \(VMs @ no__t，应用程序需要 DNS 在其自己的网络中和 Internet 上的外部资源通信。 通过 Idn，你可以为租户提供 DNS 名称解析服务，用于隔离本地命名空间和 Internet 资源。

由于 Idn 服务无法从租户虚拟网络（而不是通过 Idn 代理）进行访问，因此服务器不容易受到租户网络上的恶意活动的攻击。

**关键功能**

下面是 Idn 的主要功能。

- 为租户工作负荷提供共享 DNS 名称解析服务
- 用于名称解析的权威 DNS 服务和租户命名空间内的 DNS 注册
- 用于解析来自租户 Vm 的 Internet 名称的递归 DNS 服务。
- 如果需要，可以配置构造和租户名称的同时托管
- 经济高效的 DNS 解决方案-租户不需要部署自己的 DNS 基础结构
- 需要 Active Directory 集成的高可用性。

除了这些功能，如果您担心将 AD 集成的 DNS 服务器打开到 Internet，则可以将 Idn 服务器部署在外围网络中的另一个递归解析程序之后。

由于 Idn 是所有 DNS 查询的集中式服务器，因此 CSP 或企业还可以实现租户 DNS 防火墙、应用筛选器、检测恶意活动，以及在中心位置审核事务

## <a name="idns-infrastructure"></a>Idn 基础结构
Idn 基础结构包括 Idn 服务器和 Idn 代理。

### <a name="idns-servers"></a>Idn 服务器
Idn 包括一组 DNS 服务器，这些服务器托管特定于租户的数据，例如 VM DNS 资源记录。

Idn 服务器是其内部 DNS 区域的权威服务器，当租户 Vm 尝试连接到外部资源时，它们也充当公用名的解析程序。

虚拟网络上的 Vm 的所有主机名均存储为同一区域下的 DNS 资源记录。 例如，如果为一个名为 Idn 的区域部署了，则该网络上的 Vm 的 DNS 资源记录将存储在 contoso 区域中。

租户 VM 完全限定的域名 \(FQDNs @ no__t-1 包含计算机名称和虚拟网络的 DNS 后缀字符串（采用 GUID 格式）。 例如，如果你有一个名为 TENANT1 的租户 VM，该 VM 位于虚拟网络 contoso，本地，则该 VM 的 FQDN 为 TENANT1。*vn*，其中*Vn*是用于虚拟网络的 DNS 后缀字符串。

>[!NOTE]
>如果你是构造管理员，则可以使用你的 CSP 或企业 DNS 基础结构作为 Idn 服务器，而不是专门将新的 DNS 服务器部署为用作 Idn 服务器。 无论是为 Idn 部署新服务器还是使用现有基础结构，Idn 都依赖于 Active Directory 来提供高可用性。 因此，你的 Idn 服务器必须与 Active Directory 集成。

### <a name="idns-proxy"></a>Idn 代理
Idn 代理是一种 Windows 服务，它在每个主机上运行，并将租户虚拟网络 DNS 流量转发到 Idn 服务器。

下图描绘了通过 Idn 代理到 Idn 服务器和 Internet 的租户虚拟网络的 DNS 流量路径。

![Idn 基础结构](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>如何部署 Idn
当你使用脚本在 Windows Server 2016 中部署 SDN 时，Idn 会自动包含在你的部署中。

有关详细信息，请参阅以下主题。

- [使用脚本部署软件定义的网络基础结构](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>了解 Idn 部署步骤
使用脚本时，可以使用本部分了解如何安装和配置 Idn。

下面概述了部署 Idn 所需的步骤。

>[!NOTE]
>如果已使用脚本部署了 SDN，则无需执行这些步骤。 仅提供有关信息和故障排除用途的步骤。

### <a name="step-1-deploy-dns"></a>第 1 步：部署 DNS
你可以使用以下示例 Windows PowerShell 命令来部署 DNS 服务器。
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>步骤 2：在网络控制器中配置 Idn 信息
此脚本段是管理员对网络控制器进行的 REST 调用，它会通知 Idn 区域配置，例如 iDNSServer 的 IP 地址和用于托管 Idn 名称的区域。 

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
>这是在 SDNExpress 中**配置 ConfigureIDns**部分的摘录。 有关详细信息，请参阅[使用脚本部署软件定义的网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-3-configure-the-idns-proxy-service"></a>步骤 3:配置 Idn 代理服务
Idn 代理服务在每个 Hyper-v 主机上运行，在租户虚拟网络与 Idn 服务器所在的物理网络之间提供桥梁。 必须在每个 Hyper-v 主机上创建以下注册表项。


**DNS 端口：** 固定端口53

- 注册表项 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "Port"
- ValueData = 53
- ValueType = "Dword"
       

**DNS 代理端口：** 固定端口53

- 注册表项 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**DNS IP：** 在租户选择使用 Idn 服务时在网络接口上配置的固定 IP 地址

- 注册表项 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "String"

        
**Mac 地址：** DNS 服务器的媒体访问控制地址

- 注册表项 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = "aa-bb-cc-aa-bb-cc"
- ValueType = "String"

**服务器地址 IDN：** 以逗号分隔的 Idn 服务器列表。

- 注册表项：HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "转发器"
- ValueData = "10.0.0.9"
- ValueType = "String"



>[!NOTE]
>这是在 SDNExpress 中**配置 ConfigureIDnsProxy**部分的摘录。 有关详细信息，请参阅[使用脚本部署软件定义的网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>步骤 4：重新启动网络控制器主机代理服务
你可以使用以下 Windows PowerShell 命令重新启动网络控制器主机代理服务。
    
    Restart-Service nchostagent -Force
    
有关详细信息，请参阅[重新启动服务](https://technet.microsoft.com/library/hh849823.aspx)。

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>启用 DNS 代理服务的防火墙规则
你可以使用以下 Windows PowerShell 命令创建防火墙规则，该规则允许代理与 VM 和 Idn 服务器进行通信。
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

有关详细信息，请参阅[set-netfirewallrule](https://technet.microsoft.com/library/jj554869.aspx)。
    
### <a name="validate-the-idns-service"></a>验证 Idn 服务
若要验证 Idn 服务，必须部署示例租户工作负荷。

有关详细信息，请参阅[创建 VM 和连接到租户虚拟网络或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。

如果希望租户 VM 使用 Idn 服务，则必须将 VM 网络接口 DNS 服务器配置留空，并允许接口使用 DHCP。 

启动具有此类网络接口的 VM 后，它会自动收到允许 VM 使用 Idn 的配置，并且 VM 会立即通过使用 Idn 服务开始执行名称解析。

如果通过将网络接口 DNS 服务器和备用 DNS 服务器信息留空来将租户 VM 配置为使用 Idn 服务，则网络控制器会提供具有 IP 地址的 VM，并代表 VM 通过 Idn 服务器执行 DNS 名称注册. 

网络控制器还向 Idn 代理通知 VM，并将所需的详细信息通知给 VM 的名称解析。 

当 VM 启动 DNS 查询时，代理将充当从虚拟网络到 Idn 服务的查询转发器。 

DNS 代理还可以确保租户 VM 查询是隔离的。 如果 Idn 服务器对查询具有权威，则 Idn 服务器将使用权威响应进行响应。 如果 Idn 服务器对查询没有权威，则会执行 DNS 递归来解析 Internet 名称。

>[!NOTE]
>此信息包含在 SDNExpressTenant 的**配置 AttachToVirtualNetwork**部分中。 有关详细信息，请参阅[使用脚本部署软件定义的网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

