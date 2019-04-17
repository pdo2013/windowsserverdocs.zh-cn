---
title: 对于负载平衡配置软件负载平衡和网络地址翻译 (NAT)
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
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
ms.openlocfilehash: 7f0393db564061caa0bc8f18b1d623f24749b46c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>对于负载平衡配置软件负载平衡和网络地址翻译 (NAT)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何使用软件定义网络 \(SDN\) 软件负载平衡 \(SLB\) 提供站网络地址转换 NAT，站 NAT，或负载平衡之间的应用程序的多个实例。

本主题包含以下部分。

- [软件负载平衡概述](#bkmk_slbover)
- [示例： 创建公用 VIP 负载平衡虚拟网络上的两个虚拟机的池](#bkmk_publicvip)
- [示例： 用于 SLB 站 NAT](#bkmk_obnat)
- [示例： 向后端池中添加网络接口](#bkmk_backend)
- [示例： 使用软件负载平衡转发的交通](#bkmk_forward)

## <a name="bkmk_slbover"></a>软件负载平衡概述

SDN 软件负载平衡 \(SLB\) 高可用性和网络的性能，提供给你的应用程序。 它是一层 4 \ （UDP\ TCP） 负载平衡分发进入通信健康服务中的云服务或负载平衡集中定义虚拟机的实例。

您可以配置 SLB 执行下列。

* 加载余额传入交通外部到虚拟机 \(VMs\) 虚拟网络。 这称为公共 VIP 负载平衡。
* 加载余额传入交通之间虚拟网络中的虚拟机的功能、 之间的云服务、 虚拟机的功能或之间本地计算机和在跨本地虚拟网络虚拟机的功能。 
* 到使用网络地址翻译 (NAT) 的外部目标，从虚拟网络转发 VM 网络流量。  这称为站 nat。
* 转发给特定 VM 外部通信。  这称为站 nat。

>[!IMPORTANT]
>一个已知的问题阻止在 NetworkController Windows PowerShell 模块的负载平衡对象 Windows Server 2016 5 中正常工作。 解决方法是改用动态哈希表格和调用 WebRequest。 此方法以下示例所示。


## <a name="bkmk_publicvip"></a>示例： 创建公用 VIP 负载平衡虚拟网络上的两个虚拟机的池

此示例中可用于为池成员向 VIP 提供请求公共 VIP 和两个 Vm 创建负载平衡对象。  此示例代码还添加了可检测到的一个池成员变得无响应 HTTP 健康探测。

###<a name="step-1-prepare-the-load-balancer-object"></a>第 1 步： 准备负载平衡对象
你可以使用下面的示例准备负载平衡对象。

    $lbresourceId = "LB2"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

###<a name="step-2-assign-a-front-end-ip"></a>第 2 步： 将分配前端 IP
前端 IP 是通常所说的作为虚拟 IP (VIP)。  VIP 必须取自未使用 IP 这已之前提供给负载平衡管理器 IP 池的逻辑网络之一。

你可以使用下面的示例分配前端 IP 地址。

    $vipip = "10.127.132.5"
    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

###<a name="step-3-allocate-a-backend-address-pool"></a>第 3 步： 分配的后端地址池
后端地址池包含构成虚拟机的功能负载平衡组成员动态 IPs (Dip)。 在此步骤只能分配池;添加 IP 配置后面的步骤。

你可以使用下面的示例分配的后端地址池。
 
    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-4-define-a-health-probe"></a>第 4 步： 定义健康探测
健康探测是负载平衡用于确定后端池成员运行状况。 使用此示例中，您定义查询 HTTP 探测到的 RequestPath"月 health.htm"。  执行查询 IntervalInSeconds 属性指定的每 5 秒。

健康探测必须接收 11 连续探测考虑为正常的后端 IP 查询的 200 HTTP 响应代码。 如果不正常的后端 IP，负载平衡不将向 IP 发送通信。

>[!Note]
>很重要，您将应用于后端 IP 任何访问控制列出不会阻止指向或从第一个 IP 网中, 流量，因为它是探测发放点。

你可以使用下面的示例定义健康探测。
 
    $lbprobe = @{}
    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$lbresourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties = @{}
    $lbprobe.properties.protocol = "HTTP"
    $lbprobe.properties.port = "80"
    $lbprobe.properties.RequestPath = "/health.htm"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11
    $lbproperties.probes += $lbprobe

###<a name="step-5-define-a-load-balancing-rule"></a>第 5 步： 定义负载平衡规则
此加载平衡规则定义如何将发送到的后端 IP 到达前端 IP 的交通。  在此示例中，向端口 80 TCP 交通发给后端池。

你可以使用下面的示例定义负载平衡规则。

    $lbrule = @{}
    $lbrule.ResourceId = "webserver1"
    $lbrule.properties = @{}
    $lbrule.properties.FrontEndIPConfigurations = @()
    $lbrule.properties.FrontEndIPConfigurations += $fe
    $lbrule.properties.backendaddresspool = $backend 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = 80
    $lbrule.properties.Probe = $lbprobe
    $lbproperties.loadbalancingRules += $lbrule

###<a name="step-6-add-the-load-balancer-configuration-to-network-controller"></a>第 6 步： 添加到网络控制器负载平衡配置
到目前为止在此示例中，所有创建的对象有 Windows PowerShell 会话的内存中。 此步骤中将添加到网络控制器的对象。

你可以使用下面的示例添加到网络控制器负载平衡器配置。

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

此步骤后您将需要按照下面添加到此后端池网络接口的示例。

## <a name="bkmk_obnat"></a>示例： 用于 SLB 站 NAT

此示例中可用于与提供站 NAT 功能 vm 专用地址空间的虚拟网络上联系站到 internet 后端池配置 SLB。

###<a name="step-1-create-the-loadbalancer-properties-front-end-ip-and-backend-pool"></a>第 1 步： 创建负载平衡器属性，前端 IP 和后端池。
可以使用下面的示例创建负载平衡器属性，前端 IP 和后端池。

    $lbresourceId = "OutboundNATMembers"
    $vipip = "10.127.132.7"

    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-2-define-the-outbound-nat-rule"></a>第 2 步： 定义站 NAT 规则
你可以使用下面的示例定义站 NAT 规则。 

    $onat = @{}
    $onat.ResourceId = "onat1"
    $onat.properties = @{}
    $onat.properties.frontendipconfigurations = @()
    $onat.properties.frontendipconfigurations += $fe
    $onat.properties.backendaddresspool = $backend
    $onat.properties.protocol = "ALL"
    $lbproperties.OutboundNatRules += $onat

###<a name="step-3-add-the-load-balancer-object-in-network-controller"></a>第 3 步： 添加负载平衡对象在网络控制器
你可以使用下面的示例中网络控制器添加负载平衡对象。

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

在下一步中，你可以添加你想要提供 internet 访问权限的网络接口。

## <a name="bkmk_backend"></a>示例： 向后端池中添加网络接口
可以使用此示例中向后端池中添加网络接口。

你必须重复此步骤可以处理每个网络接口请求对 VIP 进行。 你还可以重复上将其添加多个负载平衡对象到一个网络接口此过程。 例如，如果你拥有的 Web 服务器 VIP 负载平衡对象和单独的加载平衡对象提供站 nat。
    
### <a name="step-1-get-the-load-balancer-object-containing-the-back-end-pool-to-which-you-will-add-a-network-interface"></a>第 1 步： 获取包含的后端池，你将向其添加网络接口负载平衡对象
你可以使用下面的示例检索负载平衡对象。

    $lbresourceid = "LB2"
    $lb = (Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Get" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -DisableKeepAlive -UseBasicParsing).content | convertfrom-json 

### <a name="step-2-get-the-network-interface-and-add-the-backendaddress-pool-to-the-loadbalancerbackendaddresspools-array"></a>第 2 步： 获取网络接口，并添加 loadbalancerbackendaddresspools 深刻 backendaddress 池。
你可以使用下面的示例添加到 loadbalancerbackendaddresspools 深刻 backendaddress 池，获取网络接口。

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    
### <a name="step-3-put-the-network-interface-to-apply-the-change"></a>第 3 步： 将使网络接口要将更改应用
你可以使用下面的示例使网络接口以应用更改。

    new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force
 
## <a name="bkmk_forward"></a>示例： 使用软件负载平衡转发的交通
如果你需要将映射到一个网络接口虚拟网络上的虚拟 IP 不定义个别端口，则可以创建 L3 转移规则。  此规则转发分配 VIP，必须在 PublicIPAddress 对象包含通过 VM 的所有通信。

如果 VIP 和冲动指相同子网，然后这相当于执行 L3 转移不 nat。

>[!NOTE]
>此过程，不需要你创建一个负载平衡对象。  分配给网络接口 PublicIPAddress 是软件负载平衡执行其配置足够信息。

###<a name="step-1-create-a-public-ip-object-to-contain-the-vip"></a>第 1 步： 创建包含 VIP 公共 IP 对象
可以使用下面的示例来创建公共 IP 对象。

    $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
    $publicIPProperties.ipaddress = "10.127.132.6"
    $publicIPProperties.PublicIPAllocationMethod = "static"
    $publicIPProperties.IdleTimeoutInMinutes = 4
    $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri

####<a name="step-2-assign-the-publicipaddress-to-a-network-interface"></a>第 2 步： 将 PublicIPAddress 分配给网络接口
你可以使用下面的示例分配给网络接口 PublicIPAddress。

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
    New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties



 