---
title: Windows Server 软件定义的网络堆栈疑难解答
description: 此 Windows Server 指南检查软件定义网络 (SDN) 的常见错误和故障方案并概述了故障排除的工作流，可利用可用的诊断工具。
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: b6d4ff37186e66bec54794f8d6c9fd8a83e23e7d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845388"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Windows Server 软件定义的网络堆栈疑难解答

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本指南将检查软件定义网络 (SDN) 的常见错误和故障方案并概述了故障排除的工作流，可利用可用的诊断工具。  

有关 Microsoft 的软件定义的网络的详细信息，请参阅[软件定义的网络](../../sdn/Software-Defined-Networking--SDN-.md)。  

## <a name="error-types"></a>错误类型  
下面的列表从市场中生产部署 Windows Server 2012 R2 中表示的类与 HYPER-V 网络虚拟化 (HNVv1) 最常出现的问题并与相同类型的 Windows Server 2016 中出现的问题在很多方面一致使用新的软件定义网络 (SDN) 堆栈 HNVv2。  

大多数错误可划分为一小组类：   
* **无效或不受支持的配置**  
   用户调用 NorthBound API 不正确或无效的策略。   

* **策略应用程序中的错误**  
     网络控制器中的策略未被传递到 HYPER-V 主机时，显著延迟和/或不是最新 （例如，在实时迁移） 的所有 HYPER-V 主机上。  
* **配置偏移或软件 bug**  
 数据路径问题导致丢弃的数据包。  

* **外部错误与 NIC 硬件 / 驱动程序或衬网络构造**  
 产生错误行为 （如 VMQ) 任务卸载或配置错误 （如 MTU) 下衬网络构造   

 此故障排除指南检查每个这些种类的错误和建议的最佳做法和诊断工具可用于确定并修复错误。  

## <a name="diagnostic-tools"></a>诊断工具  

在讨论之前的故障排除工作流对于每个这些类型的错误，让我们检查提供的诊断工具。   
  
若要使用网络控制器 （控制路径） 的诊断工具，必须先安装 RSAT NetworkController 功能并导入``NetworkControllerDiagnostics``模块：  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

若要使用 HNV 诊断 （数据项） 的诊断工具，必须导入``HNVDiagnostics``模块：
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>网络控制器诊断  
在 TechNet 上记录了这些 cmdlet[网络控制器诊断 Cmdlet 主题](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/)。 它们帮助识别问题的控制路径之间的网络控制器节点和网络控制器与在 HYPER-V 主机上运行的 NC 主机代理之间的网络策略保持一致。

 _调试 ServiceFabricNodeStatus_并_Get NetworkControllerReplica_必须从网络控制器节点虚拟机的一个运行 cmdlet。 所有其他 NC 诊断 cmdlet 可以从已连接到网络控制器和位于在网络控制器管理安全组 (Kerberos) 或有权访问用于管理网络控制器的 X.509 证书中的任何主机运行。 
   
### <a name="hyper-v-host-diagnostics"></a>HYPER-V 主机诊断  
在 TechNet 上记录了这些 cmdlet [HYPER-V 网络虚拟化 (HNV) 诊断 Cmdlet 主题](https://docs.microsoft.com/powershell/module/hnvdiagnostics/)。 它们可帮助识别租户虚拟机 （东部/西部） 之间的数据路径中的问题和入口流量通过 SLB VIP （北/南）。 

_调试 VirtualMachineQueueOperation_， _Get CustomerRoute_， _Get PACAMapping_， _Get ProviderAddress_， _Get VMNetworkAdapterPortId_， _Get VMSwitchExternalPortId_，并_测试 EncapOverheadSettings_是它可以从任何 HYPER-V 主机运行的所有本地测试。 其他 cmdlet 调用通过网络控制器的数据路径测试，因此需要访问作为上面 descried 网络控制器。
 
### <a name="github"></a>GitHub
[Microsoft/SDN GitHub 存储库](https://github.com/microsoft/sdn)具有大量的示例脚本和基于这些内置 cmdlet 生成的工作流。 具体而言中, 找到诊断的脚本[诊断](https://github.com/Microsoft/sdn/diagnostics)文件夹。 请帮助我们通过提交拉取请求参与这些脚本。

## <a name="troubleshooting-workflows-and-guides"></a>故障排除工作流和指南  

### <a name="hoster-validate-system-health"></a>[主机]验证系统运行状况
没有名为嵌入的资源_配置状态_中多个网络控制器资源。 配置状态提供了有关包括网络控制器配置和实际的 （运行） 状态的 HYPER-V 主机之间的一致性的系统运行状况的信息。 

若要检查配置状态，请运行以下从任何 HYPER-V 主机连接到网络控制器。

>[!NOTE] 
>值*NetworkController*参数应为基于 X.509 的使用者名称的 FQDN 或 IP 地址 > 为网络控制器创建的证书。
>
>*凭据*参数仅需要指定网络控制器使用 Kerberos 身份验证 （典型 VMM 部署中）。 凭据必须是在网络控制器管理安全组的用户。

```none
Debug-NetworkControllerConfigurationState -NetworkController <FQDN or NC IP> [-Credential <PS Credential>]

# Healthy State Example - no status reported
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController 10.127.132.211 -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways

```

如下所示的示例配置状态消息：

```none
Fetching ResourceType:     servers
---------------------------------------------------------------------------------------------------------
ResourcePath:     https://10.127.132.211/Networking/v1/servers/4c4c4544-0056-4b10-8058-b8c04f395931
Status:           Warning

Source:           SoftwareLoadBalancerManager
Code:             HostNotConnectedToController
Message:          Host is not Connected.
----------------------------------------------------------------------------------------------------------
```

>[!NOTE]
> SLB Mux 传输 VM NIC 的网络接口资源处于故障状态出现错误"虚拟交换机-主机未连接到控制器"在系统中没有一个 bug。 如果传输逻辑网络的 IP 池中的 VM NIC 资源中的 IP 配置设置为 IP 地址，可以安全地忽略此错误。 网关 HNV 提供程序 VM Nic 的网络接口资源处于故障状态出现错误"虚拟交换机-PortBlocked"在系统中没有第二个 bug。 此错误还可忽略如果中的 VM NIC 资源的 IP 配置设置为 null （按照设计）。


下表显示了错误代码、 消息和后续操作，以根据观察到的配置状态采取的列表。

  
| **Code**| **Message**| **操作**|  
|--------|-----------|----------|  
| Unknown| 未知的错误| |  
| HostUnreachable                       | 主机计算机不可访问 | 检查网络控制器和主机之间的管理网络连接 |  
| PAIpAddressExhausted                  | 已用完 PA Ip 地址 | 增加 HNV 提供程序逻辑子网的 IP 池大小 |  
| PAMacAddressExhausted                 | 已用完 PA Mac 地址 | 增加 Mac 池范围 |  
| PAAddressConfigurationFailure         | 未能检测到主机的 PA 地址 | 检查网络控制器和主机之间的管理网络连接。 |  
| CertificateNotTrusted                 | 证书不受信任  |修复用于与主机的通信的证书。 |  
| CertificateNotAuthorized              | 未授权的证书 | 修复用于与主机的通信的证书。 |  
| PolicyConfigurationFailureOnVfp       | 在配置 VFP 策略失败 | 这是运行时失败。  没有明确的变通。 收集日志。 |  
| PolicyConfigurationFailure            | 在将策略推送到这些主机，由于通信故障或其他故障 NetworkController 时出错。| 没有明确的操作。  这是因为目标状态中的网络控制器模块的处理失败。 收集日志。 |  
| HostNotConnectedToController          | 主机尚未连接到网络控制器 | 不应用于主机或主机的端口配置文件不可访问的网络控制器。 验证 HostID 注册表项与服务器资源的实例 ID 匹配 |  
| MultipleVfpEnabledSwitches            | 有多个 VFp 在主机上启用开关  | 删除一个开关，因为网络控制器主机代理仅支持一个 vSwitch 启用了 VFP 扩展 |  
| PolicyConfigurationFailure            | 未能为因证书错误或连接错误而 VmNic 推送 VNet 策略  | 检查是否部署了正确的证书 （证书使用者名称必须匹配主机的 FQDN）。 此外请验证与网络控制器主机连接 |  
| PolicyConfigurationFailure            | 未能为因证书错误或连接错误而 VmNic 推送 vSwitch 策略  | 检查是否部署了正确的证书 （证书使用者名称必须匹配主机的 FQDN）。 此外请验证与网络控制器主机连接|
| PolicyConfigurationFailure            | 未能为因证书错误或连接错误而 VmNic 推送防火墙策略 | 检查是否部署了正确的证书 （证书使用者名称必须匹配主机的 FQDN）。 此外请验证与网络控制器主机连接|
| DistributedRouterConfigurationFailure | 无法在主机 vNic 上配置分布式路由器设置                          | TCPIP 堆栈时出错。 可能需要清理其报告此错误的服务器上的 PA 和灾难恢复主机 Vnic |
| DhcpAddressAllocationFailure          | DHCP 地址分配失败是 VMNic。                                                    | 检查是否在 NIC 资源上配置静态 IP 地址属性 |  
| CertificateNotTrusted<br>CertificateNotAuthorized | 无法连接到 Mux 由于网络或证书错误 | 检查错误消息代码中提供的数值代码： 这对应于 winsock 错误代码。 证书错误是粒度 (例如，无法验证证书，证书未获得授权，等等。) |  
| HostUnreachable                       | MUX 是不正常 （常见用例是 BGPRouter 断开连接） | RRAS （BGP 虚拟机） 或顶部柜顶式 (ToR) 交换机上的 BGP 对等节点已成功为无法访问或未对等互连。 检查软件负载均衡器多路复用器资源和 BGP 对等方 （ToR 或 RRAS 虚拟机） 上的 BGP 设置 |  
| HostNotConnectedToController          | SLB 主机代理未连接  | 检查 SLB 主机代理服务正在运行;原因，请参阅 SLB 主机代理日志 （自动运行） 为何，以防 SLBM (NC) 拒绝 cert 主讲人运行的主机代理状态将显示一些略有区别的信息  |  
| PortBlocked                           | VFP 端口被阻止，因为缺乏 VNET / ACL 策略 | 检查是否有任何其他错误，这可能会导致不配置的策略。 |  
| 重载                            | 负载均衡器 MUX 重载  | MUX 性能问题 |  
| RoutePublicationFailure               | 负载均衡器 MUX 未连接到 BGP 路由器 | 确定是否在 MUX 具有与 BGP 路由器和该 BGP 对等互连的连接已正确设置 |  
| VirtualServerUnreachable              | 负载均衡器 MUX 未连接到 SLB 管理器 | 检查 SLBM 和 MUX 之间的连接 |  
| QosConfigurationFailure               | 未能配置 QOS 策略 | 有足够的带宽可为所有 VM 是否使用 QOS 预留 |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>检查网络控制器的 HYPER-V 主机 （NC 主机代理服务） 之间的网络连接
运行*netstat*以下命令以验证是否 NC 主机代理和网络控制器节点和一个侦听套接字上的 HYPER-V 主机之间的三个已建立连接
- 侦听端口 TCP:6640 上的 HYPER-V 主机 （NC 主机代理服务）
- 从 HYPER-V 的两个已建立的连接主机端口上 6640 到 NC 节点 IP (> 32000) 的临时端口上的 IP
- 从临时端口上的 HYPER-V 主机 IP 到网络控制器 REST IP 端口 6640 上的一个已建立的连接

>[!NOTE]
>如果没有该特定主机上部署租户虚拟机，仅可能在 HYPER-V 主机上的两个已建立的连接。

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>检查主机代理服务
网络控制器通信通过 HYPER-V 主机上的两个主机代理服务：SLB 主机代理和 NC 主机代理。 它是可能的一个或这两项服务未运行。 检查其状态并重启如果它们未运行。

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>检查网络控制器的运行状况
如果不使用三个已建立连接或网络控制器会导致响应，检查所有节点和服务模块都是启动并运行通过使用以下 cmdlet。 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
网络控制器服务模块包括：
- ControllerService
- ApiService
- SlbManagerService
- ServiceInsertion
- FirewallService
- VSwitchService
- GatewayManager
- FnmService
- HelperService
- UpdateService

检查是否 ReplicaStatus**准备好**HealthState 是**确定**。

在生产中部署是使用多节点网络控制器，你还可以查看每个服务是主要在哪个节点和其各个单独的副本状态。

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
检查副本状态是否已准备的每个服务。
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>检查相应的主机 Id 和网络控制器和每个 HYPER-V 主机之间的证书 
在 HYPER-V 主机上运行以下命令来检查主机标识对应于在网络控制器上的服务器资源的实例 Id

```none
Get-ItemProperty "hklm:\system\currentcontrolset\services\nchostagent\parameters" -Name HostId |fl HostId

HostId : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**

Get-NetworkControllerServer -ConnectionUri $uri |where { $_.InstanceId -eq "162cd2c8-08d4-4298-8cb4-10c2977e3cfe"}

Tags             :
ResourceRef      : /servers/4c4c4544-0056-4a10-8059-b8c04f395931
InstanceId       : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**
Etag             : W/"50f89b08-215c-495d-8505-0776baab9cb3"
ResourceMetadata : Microsoft.Windows.NetworkController.ResourceMetadata
ResourceId       : 4c4c4544-0056-4a10-8059-b8c04f395931
Properties       : Microsoft.Windows.NetworkController.ServerProperties
```

*修正*如果使用 SDNExpress 脚本或手动部署更新的 HostId 密钥以匹配服务器资源的实例 Id 在注册表中。 如果使用 VMM，从 VMM 删除 HYPER-V 服务器重新启动的 HYPER-V 主机 （物理服务器） 上的网络控制器主机代理并删除 HostId 注册表项。 然后，重新添加到 VMM 服务器。


检查 (SouthBound) 之间的通信的 HYPER-V 主机 （NC 主机代理服务） 和网络控制器节点 （主机名将为证书的使用者名称） 的 HYPER-V 主机所使用的 X.509 证书的指纹相同。 此外检查网络控制器 REST 证书是否具有使用者名称*CN =<FQDN or IP>*。

```  
# On Hyper-V Host
dir cert:\\localmachine\my  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
...

dir cert:\\localmachine\root

Thumbprint                                Subject
----------                                -------
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**

# On Network Controller Node VM
dir cert:\\localmachine\root  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**
...
```  

您还可以检查以确保使用者名称是什么是每个证书的以下参数应为 （主机名或 NC REST FQDN 或 IP），该证书尚未过期，以及受信任的根中包含证书链中的所有证书颁发机构颁发机构。

- 使用者名称  
- 到期日期  
- 受信任的根证书颁发机构  

*修正*如果多个证书的 HYPER-V 主机上具有相同的使用者名称，网络控制器主机代理将随机选择要向网络控制器。 这可能会与已知的网络控制器的服务器资源的指纹不匹配。 在这种情况下，删除具有相同的使用者名称的 HYPER-V 主机上的证书之一，然后重新启动网络控制器主机代理服务。 如果可以仍然建立连接，删除 HYPER-V 主机上的相同使用者名称的其他证书并删除相应的服务器资源，在 VMM 中。 然后，重新创建在其中将生成新的 X.509 证书并将其安装在 HYPER-V 主机上的 VMM 中的服务器资源。
  

#### <a name="check-the-slb-configuration-state"></a>检查 SLB 配置状态
SLB 配置状态可确定作为到调试 NetworkController cmdlet 输出的一部分。 此 cmdlet 还会输出 JSON 文件、 从每个 HYPER-V 主机 （服务器） 的所有 IP 配置和主机代理数据库表中的本地网络策略中的网络控制器资源的当前集。 

默认情况下，将收集附加跟踪。 若要不收集跟踪，请添加-IncludeTraces: $false 参数。

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>默认输出位置将是 < working_directory > \NCDiagnostics\ 目录。 可以使用更改默认输出目录`-OutputDirectory`参数。 

SLB 配置状态信息可在_诊断 slbstateResults.Json_此目录中的文件。

此 JSON 文件可以划分为以下各节：
 * 构造
   * SlbmVips-此部分列出了由网络控制器到 coodinate 配置和 SLB Mux 和 SLB 主机代理之间的运行状况的 SLB 管理器 VIP 地址的 IP 地址。
   * MuxState-本部分将列出一个值，为提供的状态在 mux 部署每个 SLB Mux 的
   * 路由器配置-此部分将列出上游路由器的 （BGP 对等） 自治系统编号 (ASN)、 传输 IP 地址和 id。 它还将列出 SLB Mux ASN 和传输 IP。
   * 连接主机信息-本部分将列表管理 IP 地址的所有 HYPER-V 主机可用于运行负载均衡工作负荷。
   * Vip 范围-本部分将列出公共和专用 VIP IP 池范围。 SLBM VIP 将包括作为这些范围中的一个已分配 IP。 
   * Mux 路由-本部分将列出包含该特定 mux 路由播发的所有每个 SLB Mux 部署一个值。
 * 租户
   * VipConsolidatedState-本部分将列出每个租户 VIP，其中包括播发的路由前缀、 HYPER-V 主机和 DIP 终结点的连接状态。
    
> [!NOTE]
> SLB 状态可以直接通过使用已确定[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)上提供的脚本[Microsoft SDN GitHub 存储库](https://github.com/microsoft/sdn)。 

#### <a name="gateway-validation"></a>网关验证

**从网络控制器：**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**从网关 VM:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**从上架顶式 (ToR) 交换机：**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP Router**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

除此之外，我们有 （尤其是在基于 SDNExpress 部署），到目前为止过程的问题，从租户隔离舱不获取 GW Vm 上配置的最常见原因似乎是 FabricConfig.psd1 中的 GW 容量是小于相比什么专家尝试将分配给 TenantConfig.psd1 中的网络连接 （S2S 隧道）。 这可以通过将以下命令的输出进行比较轻松地检查：

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[主机]验证数据平面
部署网络控制器、 已创建租户虚拟网络和子网，并已附加到虚拟子网的虚拟机后，可以执行其他构造级别测试托管商检查租户的连接。

#### <a name="check-hnv-provider-logical-network-connectivity"></a>检查 HNV 提供程序逻辑网络连接
后第一个来宾中的 HYPER-V 主机上运行的虚拟机已连接到租户虚拟网络，网络控制器会将两个 HNV 提供程序 IP 地址 （PA IP 地址） 分配到 HYPER-V 主机。 这些 Ip 将来自于 HNV 提供程序逻辑网络的 IP 池，并且由网络控制器管理。  若要找出这两个 HNV IP 地址的

```none
PS > Get-ProviderAddress

# Sample Output
ProviderAddress : 10.10.182.66
MAC Address     : 40-1D-D8-B7-1C-04
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11

ProviderAddress : 10.10.182.67
MAC Address     : 40-1D-D8-B7-1C-05
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11
```

这些 HNV 提供程序 IP 地址 (PA Ip) 分配给以太网适配器在单独的 TCPIP 网络隔离舱中创建，并且具有的适配器名称_VLANX_其中 X 是分配给 HNV 提供程序 （传输） 的逻辑网络的 VLAN。

使用逻辑网络可以与其他隔离舱 ping 通过 HNV 提供程序的两个 HYPER-V 主机之间的连接 (-c Y) 参数，其中 Y 是在其中创建 PAhostVNICs TCPIP 网络隔离舱。 可通过执行确定此隔离舱：

```none
C:\> ipconfig /allcompartments /all

<snip> ...
==============================================================================
Network Information for *Compartment 3*
==============================================================================
   Host Name . . . . . . . . . . . . : SA18n30-2
<snip> ...

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-04
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::5937:a365:d135:2899%39(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.66(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-05
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::28b3:1ab1:d9d9:19ec%44(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.67(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

*Ethernet adapter vEthernet (PAhostVNic):*
<snip> ...
```

>[!NOTE]
> PA 主机 vNIC 适配器不使用的数据路径中，因此不具有分配给"vEthernet (PAhostVNic) 适配器"的 IP。

例如，假定的 HYPER-V 主机 1 和 2，具有 HNV 提供程序 (PA) 的 IP 地址：

|HYPER-V 主机-|-PA IP 地址 1|-PA IP 地址 2|
|---             |---            |---             |
|主机 1 | 10.10.182.64 | 10.10.182.65 |
|主机 2 | 10.10.182.66 | 10.10.182.67 |

我们可以成功进行 ping 操作之间这两个使用以下命令来检查 HNV 提供程序逻辑网络连接。

```none
# Ping the first PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.64

# Ping the second PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.64

# Ping the first PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.65

# Ping the second PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.65
```

*修正*如果 HNV 提供程序 ping 不起作用，则检查包括 VLAN 配置物理网络连接。 每个 HYPER-V 主机上的物理 Nic 不应分配任何特定 VLAN trunk 模式中。 管理主机 vNIC 应能独立于管理逻辑网络的 VLAN。

```none
PS C:\> Get-NetAdapter "Ethernet 4" |fl

Name                       : Ethernet 4
InterfaceDescription       : <NIC> Ethernet Adapter
InterfaceIndex             : 2
MacAddress                 : F4-52-14-55-BC-21
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Up
AdminStatus                : Up
LinkSpeed(Gbps)            : 10
MediaConnectionState       : Connected
ConnectorPresent           : True
*VlanID                     : 0*
DriverInformation          : Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60

# VMM uses the older PowerShell cmdlet <Verb>-VMNetworkAdapterVlan to set VLAN isolation
PS C:\> Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName <Mgmt>

VMName VMNetworkAdapterName Mode     VlanList
------ -------------------- ----     --------
<snip> ...        
       Mgmt                 Access   7
<snip> ...

# SDNExpress deployments use the newer PowerShell cmdlet <Verb>-VMNetworkAdapterIsolation to set VLAN isolation
PS C:\> Get-VMNetworkAdapterIsolation -ManagementOS

<snip> ...

IsolationMode        : Vlan
AllowUntaggedTraffic : False
DefaultIsolationID   : 7
MultiTenantStack     : Off
ParentAdapter        : VMInternalNetworkAdapter, Name = 'Mgmt'
IsTemplate           : True
CimSession           : CimSession: .
ComputerName         : SA18N30-2
IsDeleted            : False

<snip> ...

```
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>检查在 HNV 提供程序逻辑网络上的 MTU 和 Jumbo 帧支持

HNV 提供程序逻辑网络中的另一个常见问题是物理网络端口和/或以太网卡不具有足够大 MTU 配置为处理从 VXLAN （或 NVGRE） 封装的开销。 
>[!NOTE]
> 某些以太网卡和驱动程序支持的新 * EncapOverhead 关键字都将自动设置网络控制器主机代理为 160 的值。 然后，此值将添加到的值 * 其总和用作播发 MTU JumboPacket 关键字。
> 例如 * EncapOverhead = 160 和 * JumboPacket = 1514年 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

若要测试是否 HNV 提供程序逻辑网络支持更大 MTU 大小端到端，请使用_测试 LogicalNetworkSupportsJumboPacket_ cmdlet:
```none
# Get credentials for both source host and destination host (or use the same credential if in the same domain)
$sourcehostcred = Get-Credential
$desthostcred = Get-Credential

# Use the Management IP Address or FQDN of the Source and Destination Hyper-V hosts
Test-LogicalNetworkSupportsJumboPacket -SourceHost sa18n30-2 -DestinationHost sa18n30-3 -SourceHostCreds $sourcehostcred -DestinationHostCreds $desthostcred

# Failure Results
SourceCompartment : 3
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1632
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1472
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.

# TODO: Success Results aftering updating MTU on physical switch ports

```

*修正*
* 调整至少必须为物理交换机端口上的 MTU 大小 1674B （包括 14B 以太网标头和尾部）
* 如果网络接口卡不支持 EncapOverhead 关键字，调整 JumboPacket 关键字至少 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>检查租户 VM NIC 连接
分配给来宾 VM 的每个 VM NIC 具有专用客户地址 (CA) 和 HNV 提供程序地址 (PA) 空间之间的 CA-PA 映射。 这些映射将保留在每个 HYPER-V 主机上的 OVSDB server 表，可在通过执行以下 cmdlet。

```none
# Get all known PA-CA Mappings from this particular Hyper-V Host
PS > Get-PACAMapping

CA IP Address CA MAC Address    Virtual Subnet ID PA IP Address
------------- --------------    ----------------- -------------
10.254.254.2  00-1D-D8-B7-1C-43              4115 10.10.182.67
10.254.254.3  00-1D-D8-B7-1C-43              4115 10.10.182.67
192.168.1.5   00-1D-D8-B7-1C-07              4114 10.10.182.65
10.254.254.1  40-1D-D8-B7-1C-06              4115 10.10.182.66
192.168.1.1   40-1D-D8-B7-1C-06              4114 10.10.182.66
192.168.1.4   00-1D-D8-B7-1C-05              4114 10.10.182.66

```
>[!NOTE]
> 如果希望 CA-PA 映射则不会输出有关给定租户 VM，请检查网络控制器使用的 VM NIC 和 IP 配置资源_Get NetworkControllerNetworkInterface_ cmdlet。 另外，检查 NC 主机代理和网络控制器节点之间建立的连接。

使用此信息，可以现在为租户 VM ping 启动从网络控制器使用托管商_测试 VirtualNetworkConnection_ cmdlet。

## <a name="specific-troubleshooting-scenarios"></a>特定故障排除方案

以下部分提供有关特定情况进行故障排除指南。

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>两个租户虚拟机之间没有网络连接

1.  [租户]请确保在租户虚拟机中的 Windows 防火墙未阻止流量。  
2.  [租户]检查的 IP 地址已分配给租户虚拟机通过运行_ipconfig_。 
3.  [主机]运行**测试 VirtualNetworkConnection**从 HYPER-V 主机以验证相关的两个租户虚拟机之间的连接。 

>[!NOTE]
>VSID 指的是虚拟子网 id。 对于 VXLAN，这是 VXLAN 网络标识符 (VNI)。 可以通过运行找到该值**Get PACAMapping** cmdlet。

#### <a name="example"></a>示例

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

创建 CA ping 之间"绿色 Web VM 1"的主机上的 192.168.1.4 SenderCA ip"sa18n30-2.sa18.nttest.microsoft.com"Mgmt 10.127.132.153 ListenerCA IP 到 ip 的同时附加到虚拟子网 (VSID) 4114 为 192.168.1.5。

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

正在启动 CA 空间 ping 测试启动到为 192.168.1.5 跟踪会话 Ping 成功发件人地址 192.168.1.4 Rtt = 0 毫秒


CA 路由信息：

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

PA 路由信息：

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65
 
 <snip> ...

4.  [租户]检查存在指定的虚拟子网或 VM 网络接口以将阻止流量没有分布式的防火墙策略。    

查询 sa18.nttest.microsoft.com 域中找到 sa18n30nc 在演示环境中的网络控制器 REST API。

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>了解 IP 配置和其引用此 ACL 的虚拟子网

1. [主机]运行``Get-ProviderAddress``在这两个 HYPER-V 主机承载的两个租户中的问题的虚拟机，然后运行``Test-LogicalNetworkConnection``或``ping -c <compartment>``从 HYPER-V 主机，验证在 HNV 提供程序逻辑网络上的连接
2.  [主机]确保在 MTU 设置正确的 HYPER-V 主机上和任何第 2 层切换设备之间的 HYPER-V 主机。 运行``Test-EncapOverheadValue``相关的所有 HYPER-V 主机上。 此外检查之间的所有第 2 层交换机具有 MTU 设置为最低 1674 字节来应对 160 个字节的最大开销。  
3.  [主机]如果 PA IP 地址不存在和/或 CA 连接已断开，检查以确保接收网络策略。 运行``Get-PACAMapping``若要查看是否正确建立封装规则和创建覆盖层的虚拟网络所需的 CA-PA 映射。  
4.  [主机]检查网络控制器主机代理已连接到网络控制器。 运行``netstat -anp tcp |findstr 6640``是否   
5.  [主机]检查主机 ID HKLM / 托管租户虚拟机的服务器资源的实例 ID 相匹配。  
6. [主机]检查端口配置文件 ID 匹配的 VM 网络接口的租户虚拟机实例 ID。  

## <a name="logging-tracing-and-advanced-diagnostics"></a>日志记录、 跟踪和高级的诊断

以下各节提供有关高级诊断，日志记录和跟踪信息。

### <a name="network-controller-centralized-logging"></a>网络控制器集中式日志记录 
 
网络控制器可以自动收集调试程序日志，并将其存储在一个集中位置。 为第一次或更高版本的某一时间部署网络控制器时，可以启用日志收集。 从网络控制器收集的日志并网络由网络控制器托管的元素： 托管计算机、 软件负载均衡器 (SLB) 和网关机。 

这些日志包括网络控制器群集、 网络控制器应用程序、 网关日志、 SLB、 虚拟网络和分布式的防火墙的调试日志。 每当新主机/SLB/网关添加到网络控制器时，在这些计算机上启动日志记录。 同样，从网络控制器中删除主机/SLB/网关后，在这些计算机上停止日志记录。

#### <a name="enable-logging"></a>启用日志记录

安装网络控制器的群集使用时自动启用日志记录**安装 NetworkControllerCluster** cmdlet。 默认情况下，本地收集日志在网络控制器节点上 *%systemdrive%\SDNDiagnostics*。 它是**强烈建议**更改此位置设置为远程文件共享 （而非本地）。 

网络控制器群集日志存储在 *%programData%\Windows Fabric\log\Traces*。 可以指定中央的位置进行使用的日志收集**DiagnosticLogLocation**参数规范，这也是为远程文件共享。 

如果你想要限制对此位置的访问权限，可以提供与访问凭据**LogLocationCredential**参数。 如果提供的凭据来访问日志的位置，您还应提供**CredentialEncryptionCertificate**参数，用于加密的网络控制器节点上本地存储的凭据。  

使用默认设置，建议必须至少 75 GB 的本地节点上的中心位置和 25 GB 可用空间 （如果不使用一个中心位置） 的三节点网络控制器群集。

#### <a name="change-logging-settings"></a>更改日志记录设置

你可以随时使用的日志记录设置``Set-NetworkControllerDiagnostic``cmdlet。 可以更改以下设置：

- **集中式日志位置**。  可以更改用于存储与所有日志的位置``DiagnosticLogLocation``参数。  
- **凭据来访问日志位置**。  可以更改用于访问日志的位置，使用的凭据``LogLocationCredential``参数。  
- **将移动到本地日志记录**。  如果提供了用于存储日志的集中的位置，可以移到网络控制器节点上的本地日志记录``UseLocalLogLocation``参数 （由于较大的磁盘空间要求不推荐）。  
- **日志记录作用域**。  默认情况下，会收集所有日志。 可以更改作用域，以收集仅网络控制器群集日志。  
- **日志记录级别**。  默认日志记录级别为 Informational。 可以将其更改为错误、 警告或 Verbose。  
- **日志老化时间**。  日志存储以循环方式。 无论使用本地日志记录还是集中式日志记录，将默认情况下，具有 3 天的日志记录数据。 你可以使用此时间限制**LogTimeLimitInDays**参数。  
- **日志老化大小**。  默认情况下，将具有最大 75 GB 的日志记录数据，如果使用集中式日志记录和 25 GB，如果使用本地日志记录。 你可以使用此限制**LogSizeLimitInMBs**参数。

#### <a name="collecting-logs-and-traces"></a>收集日志和跟踪

VMM 部署网络控制器的默认方式使用集中式日志记录。 部署网络控制器服务模板时指定这些日志的文件共享位置。

如果文件位置未指定，则本地日志记录将使用每个网络控制器节点上使用日志保存在 C:\Windows\tracing\SDNDiagnostics 下。 这些日志将保存使用以下层次结构：

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- 跟踪

网络控制器使用 Service Fabric (Azure)。 某些问题进行故障排除时，可能需要 Service Fabric 日志。 可以在 C:\ProgramData\Microsoft\Service Fabric 每个网络控制器节点上找到这些日志。

如果用户已运行_调试 NetworkController_ cmdlet，将已使用网络控制器中的服务器资源指定每个 HYPER-V 主机上提供其他日志。 C:\NCDiagnostics 下保留这些日志 （和跟踪，如果启用）

### <a name="slb-diagnostics"></a>SLB 诊断

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>SLBM 构造系统错误 （托管服务提供程序操作）

1.  检查软件负载均衡器管理器 (SLBM) 正常工作以及在业务流程层可以相互通信：SLBM-> SLB Mux 和 SLBM-> SLB 主机代理。 运行[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)从有权访问网络控制器 REST 终结点的任何节点。  
2.  验证*SDNSLBMPerfCounters*在一个网络控制器节点 Vm （最好是主网络控制器节点-Get NetworkControllerReplica） 上的性能监视器：
    1.  负载均衡器 (LB) 引擎是否已连接到 SLBM？ (*SLBM LBEngine 配置总计*> 0)  
    2.  SLBM 至少知道关于其自己的终结点？ (*VIP 的终结点总数*> = 2)  
    3.  HYPER-V (DIP) 主机是否连接到 SLBM 中？ (*HP 客户端连接*= = num 服务器)   
    4.  连接到多路复用器 SLBM？ (*多路复用器连接* == *SLBM 上多路复用器正常运行* == *多路复用器报告正常*= # SLB Mux Vm)。  
3.  确保已成功在 SLB MUX 的对等互连配置 BGP 路由器  
    1.  如果使用 RRAS 的远程访问 （即 BGP 虚拟机） 具有：  
        1.  Get 机应显示已连接  
        2.  Get BgpRouteInformation 应显示在至少一个路由 SLBM VIP 自  
    2.  如果使用物理顶部柜顶式 (ToR) 切换为 BGP 对等方，请查阅您的文档  
        1.  例如: # 显示 bgp 实例  
4.  验证*SlbMuxPerfCounters*并*SLBMUX*在 SLB Mux VM 上的性能监视器计数器
5.  检查配置状态和软件负载均衡器管理器资源中的 VIP 范围  
    1.  Get NetworkControllerLoadBalancerConfiguration ConnectionUri < https://<FQDN or IP>| convertto json-深度 8 (检查在 IP 池中的 VIP 范围，并确保 SLBM VIP 自助 (*LoadBalanacerManagerIPAddress*) 和任何面向租户的 Vip 都在这些范围内）  
        1. Get NetworkControllerIpPool NetworkId"< 公共/专用 VIP 逻辑网络资源 ID >"-SubnetId"< 公共/专用 VIP 逻辑子网资源 ID >"的资源 Id"<IP Pool Resource Id>"-ConnectionUri $uri | convertto json-深度 8 
    2.  Debug-NetworkControllerConfigurationState -  

如果任何上面失败的检查，租户 SLB 状态也将在故障模式。  

**修正**   
根据提供的以下诊断信息，可以解决以下：  
* 请确保已连接 SLB 多路复用器  
  * 修复证书问题  
  * 修复网络连接问题  
* 请确保已成功配置 BGP 对等信息  
* 确保在服务器资源的服务器实例 ID 相匹配的注册表中的主机 ID (引用附录*HostNotConnected*错误代码)  
* 收集日志  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>SLBM 租户错误 （托管服务提供商和租户操作）

1.  [主机]检查*调试 NetworkControllerConfigurationState*以查看负载均衡器的任何资源是否处于错误状态。 请尝试通过以下操作项附录中的表来缓解。   
    1.  检查的 VIP 终结点存在且播发路由  
    2.  检查多少 DIP 终结点已被发现的 VIP 终结点  
2.  [租户]验证正确指定了负载均衡器资源  
    1.  验证 DIP 在 SLBM 中注册的终结点托管租户虚拟机，它们分别对应于负载均衡器后端地址池的 IP 配置  
3.  [主机]如果未发现或连接 DIP 终结点：   
    1.  检查*调试 NetworkControllerConfigurationState*  
        1.  验证该 NC 和 SLB 主机代理已成功连接到网络控制器事件处理协调器使用 ``netstat -anp tcp |findstr 6640)``  
    2.  检查*HostId*中*nchostagent*服务注册密钥 (引用*HostNotConnected*附录中的错误代码) 与相应的服务器资源实例 Id （匹配``Get-NCServer |convertto-json -depth 8``)  
    3.  检查相应虚拟机 NIC 资源的实例 Id 相匹配的虚拟机端口的端口配置文件 id   
4.  [托管提供程序]收集日志   

#### <a name="slb-mux-tracing"></a>SLB Mux 跟踪

此外可通过事件查看器确定软件负载均衡器多路复用器中的信息。 
1. 在事件查看器视图菜单下，单击"显示分析和调试日志"
2. 导航到"应用程序和服务日志"> Microsoft > Windows > SlbMuxDriver > 在事件查看器跟踪 
3. 右键单击它，然后选择"启用日志"

>[!NOTE]
>建议您仅具有想要重现问题时，在短时间启用此日志记录

### <a name="vfp-and-vswitch-tracing"></a>VFP 和 vSwitch 跟踪

从任何 HYPER-V 承载的托管来宾虚拟机连接到租户虚拟网络，你可以收集 VFP 跟踪信息以确定可能存在问题的位置。

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
