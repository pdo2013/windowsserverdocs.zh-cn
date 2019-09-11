---
title: 为负载平衡和网络地址转换 (NAT) 配置软件负载平衡器
description: 本主题是软件定义的网络指南的一部分，其中介绍了如何在 Windows Server 2016 中管理租户工作负荷和虚拟网络。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 70bc6aa6a1276506d81b56520b7e127cd271cc83
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867158"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>为负载平衡和网络地址转换 (NAT) 配置软件负载平衡器

>适用于：Windows Server（半年频道）、Windows Server 2016

可以使用本主题了解如何使用软件定义的网络\(SDN\)软件负载均衡器\(SLB\)来提供出站网络地址转换\(NAT\)，入站 NAT，或在应用程序的多个实例之间进行负载均衡。

## <a name="software-load-balancer-overview"></a>软件负载均衡器概述

SDN 软件负载均衡器\(SLB\)为应用程序提供高可用性和网络性能。 它是第 4 \(层 TCP，即 UDP\)负载均衡器，用于在云服务或负载均衡器集中定义的虚拟机中的健康服务实例之间分配传入流量。

配置 SLB 以执行以下操作：

- 将虚拟网络外部的传入流量负载均衡到虚拟机\(vm\)，也称为公共 VIP 负载平衡。
- 对虚拟网络中的 vm、云服务中的 Vm 之间的传入流量进行负载均衡，或在跨界虚拟网络中的本地计算机与 Vm 之间进行负载均衡。 
- 使用网络地址转换（NAT）将虚拟网络中的 VM 网络流量转发到外部目标（也称为出站 NAT）。
- 将外部流量转发到特定 VM，也称为入站 NAT。




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>例如：为虚拟网络中的两个 Vm 创建负载平衡的公共 VIP

在此示例中，将创建一个具有公共 VIP 的负载均衡器对象和两个 Vm 作为池成员，以向 VIP 提供请求。 此代码示例还添加了一个 HTTP 运行状况探测，用于检测某个池成员是否变成了不响应。

1. 准备负载均衡器对象。

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. 分配前端 IP 地址，通常称为虚拟 IP （VIP）。<p>VIP 必须是提供给负载平衡器管理器的一个逻辑网络 IP 池中未使用的 IP。 

   ```PowerShell
    $VIPIP = "10.127.134.5"
    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException
    
    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"
      
    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig
   ```

3. 分配后端地址池，其中包含组成负载平衡 Vm 集成员的动态 Ip （Dip）。 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. 定义负载平衡器用于确定后端池成员的运行状况状态的运行状况探测。<p>在此示例中，将定义一个查询到 "/health.htm." 的 Owin.requestpath 的 HTTP 探测  查询每5秒运行一次，由 IntervalInSeconds 属性指定。<p>运行状况探测必须收到 HTTP 响应代码200，用于探测器的11次连续查询，以将后端 IP 视为正常。 如果后端 IP 不正常，则它不会从负载均衡器接收流量。

   >[!IMPORTANT]
   >对于应用到后端 IP 的任何访问控制列表（Acl），请勿阻止进出子网中的第一个 IP 的流量，因为这是探测的始发点。

   使用以下示例定义运行状况探测。

   ```PowerShell
    $Probe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $Probe.ResourceId = "Probe1"
    $Probe.ResourceRef = "/loadBalancers/$LBResourceId/Probes/$($Probe.ResourceId)"

    $Probe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties
    $Probe.properties.Protocol = "HTTP"
    $Probe.properties.Port = "80"
    $Probe.properties.RequestPath = "/health.htm"
    $Probe.properties.IntervalInSeconds = 5
    $Probe.properties.NumberOfProbes = 11

    $LoadBalancerProperties.Probes += $Probe
   ```

5. 定义一个负载均衡规则，用于将到达前端 IP 的流量发送到后端 IP。  在此示例中，后端池接收到端口80的 TCP 流量。<p>使用以下示例定义负载均衡规则：

   ```PowerShell
   $Rule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $Rule.ResourceId = "webserver1"

   $Rule.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $Rule.Properties.FrontEndIPConfigurations += $FrontEndIPConfig
   $Rule.Properties.backendaddresspool = $BackEndAddressPool 
   $Rule.Properties.protocol = "TCP"
   $Rule.Properties.FrontEndPort = 80
   $Rule.Properties.BackEndPort = 80
   $Rule.Properties.IdleTimeoutInMinutes = 4
   $Rule.Properties.Probe = $Probe

   $LoadBalancerProperties.loadbalancingRules += $Rule
   ```

6. 将负载均衡器配置添加到网络控制器。<p>使用以下示例将负载均衡器配置添加到网络控制器：

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. 按照下一个示例将网络接口添加到此后端池。


## <a name="example-use-slb-for-outbound-nat"></a>例如：为出站 NAT 使用 SLB

在此示例中，你将使用后端池配置 SLB，以便为虚拟网络的专用地址空间中的 VM 提供出站 NAT 功能，以到达 internet 的出站。 

1. 创建负载均衡器属性、前端 IP 和后端池。

   ```PowerShell
    import-module NetworkController
    $URI = "https://sdn.contoso.com"

    $LBResourceId = "OutboundNATMMembers"
    $VIPIP = "10.127.134.6"

    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig

    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"
    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

2. 定义出站 NAT 规则。

   ```PowerShell
    $OutboundNAT = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRule
    $OutboundNAT.ResourceId = "onat1"
    
    $OutboundNAT.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRuleProperties
    $OutboundNAT.properties.frontendipconfigurations += $FrontEndIPConfig
    $OutboundNAT.properties.backendaddresspool = $BackEndAddressPool
    $OutboundNAT.properties.protocol = "ALL"

    $LoadBalancerProperties.OutboundNatRules += $OutboundNAT
   ```

3. 将负载均衡器对象添加到网络控制器中。

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. 按照下一个示例，添加要为其提供 internet 访问权限的网络接口。

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>例如：将网络接口添加到后端池
在此示例中，将网络接口添加到后端池。  您必须为每个可处理对 VIP 发出的请求的网络接口重复此步骤。 

也可以在单个网络接口上重复此过程，将其添加到多个负载均衡器对象。 例如，你有一个用于 web 服务器 VIP 的负载均衡器对象和一个单独的负载均衡器对象来提供出站 NAT。
    
1. 获取包含后端池的负载均衡器对象以添加网络接口。

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. 获取网络接口，并将 backendaddress 池添加到 loadbalancerbackendaddresspools 数组。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. 放置网络接口以应用更改。 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>例如：使用用于转发流量的软件负载均衡器
如果需要将虚拟 IP 映射到虚拟网络上的单个网络接口而不定义单独的端口，可以创建三级转发规则。  此规则通过 PublicIPAddress 对象中包含的所分配的 VIP，将所有流量转发到 VM。

如果将 VIP 和 DIP 定义为同一子网，则这等效于执行不带 NAT 的 L3 转发。

>[!NOTE]
>此过程不需要你创建负载均衡器对象。  将 PublicIPAddress 分配到网络接口的信息足以使软件负载平衡器执行其配置。


1. 创建公共 IP 对象以包含 VIP。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 将 PublicIPAddress 分配到网络接口。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>例如：使用软件负载平衡器通过动态分配的 VIP 转发流量
此示例与上一个示例重复相同的操作，但它会自动从负载均衡器的可用 Vip 池中分配 VIP，而不是指定特定的 IP 地址。 

1. 创建公共 IP 对象以包含 VIP。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 查询 PublicIPAddress 资源以确定分配了哪个 IP 地址。

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   IpAddress 属性包含分配的地址。  输出将如下所示：
   ```
    Counters                 : {}
    ConfigurationState       :
    IpAddress                : 10.127.134.2
    PublicIPAddressVersion   : IPv4
    PublicIPAllocationMethod : Dynamic
    IdleTimeoutInMinutes     : 4
    DnsSettings              :
    ProvisioningState        : Succeeded
    IpConfiguration          :
    PreviousIpConfiguration  :
   ```
 
3. 将 PublicIPAddress 分配到网络接口。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>例如：删除正在用于转发流量的 PublicIP 地址，并将其返回到 VIP 池
   此示例将删除前面的示例创建的 PublicIPAddress 资源。  删除 PublicIPAddress 后，将自动从网络接口中删除对 PublicIPAddress 的引用，将停止转发流量，并将 IP 地址返回到公共 VIP 池以供重复使用。  

4. 删除 PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 