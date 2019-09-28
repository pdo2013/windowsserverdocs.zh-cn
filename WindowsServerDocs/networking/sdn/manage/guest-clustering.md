---
title: 虚拟网络中的来宾群集
description: 仅允许连接到虚拟网络的虚拟机使用网络控制器分配的 IP 地址来在网络上进行通信。  需要浮动 IP 地址的群集技术（如 Microsoft 故障转移群集）需要执行一些额外的步骤才能正常工作。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: 05704beeae27bd9de9ad0c5cf578581c650a976f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406035"
---
# <a name="guest-clustering-in-a-virtual-network"></a>虚拟网络中的来宾群集

>适用于：Windows Server（半年频道）、Windows Server 2016

仅允许连接到虚拟网络的虚拟机使用网络控制器分配的 IP 地址来在网络上进行通信。  需要浮动 IP 地址的群集技术（如 Microsoft 故障转移群集）需要执行一些额外的步骤才能正常工作。

使浮动 IP 可访问的方法是使用软件负载平衡器 \(SLB @ no__t-1 虚拟 IP \(VIP @ no__t。  软件负载平衡器必须在该 IP 上的端口上配置运行状况探测，以便 SLB 将流量定向到当前具有该 IP 的计算机。


## <a name="example-load-balancer-configuration"></a>例如：负载均衡器配置

此示例假设你已创建了将成为群集节点的 Vm，并将它们附加到虚拟网络。  有关指南，请参阅[创建 VM 并连接到租户虚拟网络或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。  

在此示例中，将创建一个虚拟 IP 地址（192.168.2.100）以表示群集的浮动 IP 地址，并配置一个运行状况探测来监视 TCP 端口59999，以确定哪个节点是活动节点。

1. 选择 VIP。<p>通过分配 VIP IP 地址进行准备，该地址可以是与群集节点在同一子网中的任何未使用或保留的地址。  VIP 必须与群集的浮动地址匹配。

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. 创建负载均衡器属性对象。

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. 创建前 @ no__t-0end IP 地址。

   ```PowerShell
   $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
   $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
   $FrontEnd.resourceId = "Frontend1"
   $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
   $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
   $FrontEnd.properties.privateIPAddress = $VIP
   $FrontEnd.properties.privateIPAllocationMethod = "Static"
   ```

4. 创建一个后端 @ no__t-0end 池以包含群集节点。

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. 添加探测以检测浮动地址当前处于活动状态的群集节点。 

   >[!NOTE]
   >在下面定义的端口上针对 VM 的永久地址的探测查询。  端口只能在活动节点上进行响应。 

   ```PowerShell
   $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
   $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

   $lbprobe.ResourceId = "Probe1"
   $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
   $lbprobe.properties.protocol = "TCP"
   $lbprobe.properties.port = "59999"
   $lbprobe.properties.IntervalInSeconds = 5
   $lbprobe.properties.NumberOfProbes = 11
   ```

6. 添加 TCP 端口1433的负载均衡规则。<p>你可以根据需要修改协议和端口。  还可以对此 VIP 上的其他端口和 protcols 重复此步骤多次。  将 EnableFloatingIP 设置为 $true 很重要，因为这会告知负载平衡器将数据包发送到原始 VIP 所在的节点。

   ```PowerShell
   $LoadBalancerProperties.loadbalancingRules += $lbrule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $lbrule.properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $lbrule.ResourceId = "Rules1"

   $lbrule.properties.frontendipconfigurations += $FrontEnd
   $lbrule.properties.backendaddresspool = $BackEnd 
   $lbrule.properties.protocol = "TCP"
   $lbrule.properties.frontendPort = $lbrule.properties.backendPort = 1433 
   $lbrule.properties.IdleTimeoutInMinutes = 4
   $lbrule.properties.EnableFloatingIP = $true
   $lbrule.properties.Probe = $lbprobe
   ```

7. 在网络控制器中创建负载均衡器。

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. 将群集节点添加到后端池。<p>可以向池中添加群集所需的节点数。

   ```PowerShell
   # Cluster Node 1

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force
   ```

   创建负载均衡器并将网络接口添加到后端池后，便可以配置群集了。  

9. 可有可无如果使用 Microsoft 故障转移群集，请继续下一个示例。 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>示例 2：配置 Microsoft 故障转移群集

你可以使用以下步骤来配置故障转移群集。

1. 为故障转移群集安装和配置属性。

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = “192.168.2.100” 

   $nodes = @("DB1", "DB2")
   ```

2. 在一个节点上创建群集。

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. 停止群集资源。

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. 设置群集 IP 和探测端口。<p>IP 地址必须与上一示例中使用的前端 ip 地址相匹配，并且探测端口必须与上一示例中的探测端口匹配。

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. 启动群集资源。

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. 添加剩余节点。

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**群集处于活动状态。**_ 转到指定端口上的 VIP 的流量定向到活动节点。

---