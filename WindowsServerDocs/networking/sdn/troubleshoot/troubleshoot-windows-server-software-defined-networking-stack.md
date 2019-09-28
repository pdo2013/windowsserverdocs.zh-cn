---
title: Windows Server 软件定义的网络堆栈疑难解答
description: 此 Windows Server 指南检查常见的软件定义网络（SDN）错误和故障情况，并概述利用可用的诊断工具的故障排除工作流。
manager: ravirao
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: 22dcfb318a0e60bd1694496288f3e63b2780d643
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355501"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Windows Server 软件定义的网络堆栈疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

本指南将介绍常见的软件定义网络（SDN）错误和故障情况，并概述利用可用的诊断工具的故障排除工作流。  

有关 Microsoft 软件定义的网络的详细信息，请参阅[软件定义的网络](../../sdn/Software-Defined-Networking--SDN-.md)。  

## <a name="error-types"></a>错误类型  
以下列表表示在 Windows server 2012 R2 中从市场生产部署中最常遇到的与 Hyper-v 网络虚拟化（HNVv1）有关的问题的类，并以多种方式与 Windows Server 2016 中出现的相同类型的问题相符与新的软件定义网络（SDN）堆栈 HNVv2。  

大多数错误可以分为一小部分类：   
* **配置无效或不受支持**  
   用户不正确地调用 NorthBound API 或策略无效。   

* **策略应用程序错误**  
     来自网络控制器的策略未传递到 Hyper-v 主机、在所有 Hyper-v 主机上明显延迟和/或不是最新的策略（例如，在实时迁移后）。  
* **配置偏移或软件 bug**  
  导致数据包被丢弃的数据路径问题。  

* **与 NIC 硬件/驱动程序或是网络结构相关的外部错误**  
  错误的任务卸载（如 VMQ）或是的网络构造配置错误（例如，MTU）   

  此故障排除指南检查每个错误类别，并推荐可用于识别和修复错误的最佳实践和诊断工具。  

## <a name="diagnostic-tools"></a>诊断工具  

在讨论每种错误的故障排除工作流之前，让我们来看看可用的诊断工具。   

若要使用网络控制器（控制路径）诊断工具，必须先安装 NetworkController 功能并导入 ``NetworkControllerDiagnostics`` 模块：  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

若要使用 HNV 诊断（数据路径）诊断工具，你必须导入 ``HNVDiagnostics`` 模块：

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>网络控制器诊断  
这些 cmdlet 记录在 TechNet 上的[网络控制器诊断 Cmdlet 主题](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/)中。 它们有助于在网络控制器节点之间以及在 Hyper-v 主机上运行的网络控制器与 NC 主机代理之间确定网络策略一致性问题。

 必须从网络控制器节点虚拟机之一运行_ServiceFabricNodeStatus_和_NetworkControllerReplica_ cmdlet。 所有其他 NC 诊断 cmdlet 都可以从连接到网络控制器的任何主机运行，也可以在网络控制器管理安全组（Kerberos）中运行，也可以访问 x.509 证书来管理网络控制器。 

### <a name="hyper-v-host-diagnostics"></a>Hyper-v 主机诊断  
这些 cmdlet 记录在 TechNet 上的[Hyper-v 网络虚拟化（HNV）诊断 Cmdlet 主题](https://docs.microsoft.com/powershell/module/hnvdiagnostics/)中。 它们有助于识别租户虚拟机（东部/西部）之间的数据路径中的问题，以及通过 SLB VIP （北部/南）的入口流量。 

_调试-VirtualMachineQueueOperation_、 _CustomerRoute_、 _PACAMapping_、 _ProviderAddress_、 _VMNetworkAdapterPortId_ _、VMSwitchExternalPortId、、和_ _。EncapOverheadSettings_是可从任何 hyper-v 主机运行的所有本地测试。 其他 cmdlet 通过网络控制器调用数据路径测试，因此需要访问网络控制器，如以上 descried 所示。

### <a name="github"></a>GitHub
[Microsoft/SDN GitHub](https://github.com/microsoft/sdn)存储库包含多个基于这些内置 cmdlet 构建的示例脚本和工作流。 特别是，可以在[诊断](https://github.com/Microsoft/sdn/diagnostics)文件夹中找到诊断脚本。 请通过提交拉取请求来帮助我们参与这些脚本。

## <a name="troubleshooting-workflows-and-guides"></a>工作流和指南疑难解答  

### <a name="hoster-validate-system-health"></a>宿主验证系统运行状况
几个网络控制器资源中有一个名为 "_配置状态_" 的嵌入资源。 配置状态提供有关系统运行状况的信息，包括网络控制器的配置与 Hyper-v 主机上的实际（正在运行）状态之间的一致性。 

若要检查配置状态，请从连接到网络控制器的任何 Hyper-v 主机上运行以下各项。

>[!NOTE] 
>*NetworkController*参数的值应为基于为网络控制器创建的 x.509 > 证书的使用者名称的 FQDN 或 IP 地址。
>
>仅当网络控制器使用 Kerberos 身份验证（在 VMM 部署中为典型）时，才需要指定*Credential*参数。 凭据必须为网络控制器管理安全组中的用户。

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

下面显示了一个示例配置状态消息：

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
> 系统中有一个 bug，SLB Mux 传输 VM NIC 的网络接口资源处于故障状态，出现错误 "虚拟交换机主机未连接到控制器"。 如果 VM NIC 资源中的 IP 配置设置为传输逻辑网络的 IP 池的 IP 地址，则可以安全地忽略此错误。 系统中的第二个 bug 是网关 HNV 提供程序 VM Nic 的网络接口资源处于故障状态，出现错误 "虚拟交换机-PortBlocked"。 如果 VM NIC 资源中的 IP 配置设置为 null （按设计），则也可以安全地忽略此错误。


下表显示了根据观察到的配置状态要采取的错误代码、消息和跟进操作的列表。


| **编写**| **Message**| **Action**|  
|--------|-----------|----------|  
| Unknown| 未知错误| |  
| HostUnreachable                       | 主机无法访问 | 检查网络控制器与主机之间的管理网络连接 |  
| PAIpAddressExhausted                  | 已耗尽 PA Ip 地址 | 增加 HNV 提供程序逻辑子网的 IP 池大小 |  
| PAMacAddressExhausted                 | 已耗尽 PA Mac 地址 | 增加 Mac 池范围 |  
| PAAddressConfigurationFailure         | 未能将 PA 地址联结到主机 | 检查网络控制器与主机之间的管理网络连接。 |  
| CertificateNotTrusted                 | 证书不受信任  |修复用于与主机进行通信的证书。 |  
| CertificateNotAuthorized              | 证书未授权 | 修复用于与主机进行通信的证书。 |  
| PolicyConfigurationFailureOnVfp       | 配置 VFP 策略失败 | 这是运行时失败。  没有明确的解决办法。 收集日志。 |  
| PolicyConfigurationFailure            | 由于通信故障或 NetworkController 中的其他错误，无法将策略推送到主机。| 没有明确的操作。  这是由于网络控制器模块中的目标状态处理失败引起的。 收集日志。 |  
| HostNotConnectedToController          | 主机尚未连接到网络控制器 | 端口配置文件未应用于主机或主机无法从网络控制器访问。 验证主机 ID 注册表项是否与服务器资源的实例 ID 匹配 |  
| MultipleVfpEnabledSwitches            | 主机上已启用多个 VFp 启用的开关  | 删除其中一个交换机，因为网络控制器主机代理仅支持一个启用了 VFP 扩展的 vSwitch |  
| PolicyConfigurationFailure            | 由于证书错误或连接错误，无法为 VmNic 推送 VNet 策略  | 检查是否部署了正确的证书（证书使用者名称必须与主机的 FQDN 相匹配）。 同时验证主机与网络控制器的连接 |  
| PolicyConfigurationFailure            | 由于证书错误或连接错误，无法为 VmNic 推送 vSwitch 策略  | 检查是否部署了正确的证书（证书使用者名称必须与主机的 FQDN 相匹配）。 同时验证主机与网络控制器的连接|
| PolicyConfigurationFailure            | 由于证书错误或连接错误，无法为 VmNic 推送防火墙策略 | 检查是否部署了正确的证书（证书使用者名称必须与主机的 FQDN 相匹配）。 同时验证主机与网络控制器的连接|
| DistributedRouterConfigurationFailure | 未能在主机 vNic 上配置分布式路由器设置                          | TCPIP 堆栈错误。 可能需要在报告此错误的服务器上清除 PA 和 DR 主机 Vnic |
| DhcpAddressAllocationFailure          | VMNic 的 DHCP 地址分配失败                                                    | 检查是否在 NIC 资源上配置了静态 IP 地址属性 |  
| CertificateNotTrusted<br>CertificateNotAuthorized | 由于网络或证书错误，无法连接到 Mux | 检查错误消息代码中提供的数值代码：这对应于 winsock 错误代码。 证书错误是精细的（例如，无法验证证书，证书未授权，等等）。 |  
| HostUnreachable                       | MUX 不正常（常见情况是 BGPRouter 断开连接） | RRAS 上的 BGP 对等机（BGP 虚拟机）或上架（ToR）交换机无法访问或未成功进行对等互连。 检查软件负载平衡器多路复用器资源和 BGP 对等机上的 BGP 设置（ToR 或 RRAS 虚拟机） |  
| HostNotConnectedToController          | 未连接 SLB 主机代理  | 检查 SLB 主机代理服务是否正在运行;出于某些原因，请参阅 SLB 主机代理日志（自动运行），在此情况下，SLBM （NC）拒绝由主机代理运行状态提供的证书将显示微妙信息  |  
| PortBlocked                           | 由于缺少 VNET/ACL 策略，VFP 端口被阻止 | 检查是否有任何其他错误，这可能会导致未配置策略。 |  
| 重载                            | 负载平衡 MUX  | MUX 性能问题 |  
| RoutePublicationFailure               | Loadbalancer MUX 未连接到 BGP 路由器 | 检查 MUX 是否已连接到 BGP 路由器并且已正确设置 BGP 对等互连 |  
| VirtualServerUnreachable              | Loadbalancer MUX 未连接到 SLB 管理器 | 检查 SLBM 和 MUX 之间的连接 |  
| QosConfigurationFailure               | 未能配置 QOS 策略 | 如果使用了 QOS 保留，请查看是否有足够的带宽可用于所有 VM |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>检查网络控制器和 Hyper-v 主机之间的网络连接（NC 主机代理服务）
运行下面的*netstat*命令，验证在 NC 主机代理和网络控制器节点与 hyper-v 主机上的一个侦听套接字之间是否有三个已建立的连接
- 在 Hyper-v 主机上侦听端口 TCP：6640（NC 主机代理服务）
- 从端口6640上的 Hyper-v 主机 IP 到临时端口上的 NC 节点 IP 的两个已建立连接（> 32000）
- 一种建立的连接，从临时端口上的 Hyper-v 主机 IP 到端口6640上的网络控制器 REST IP 的连接

>[!NOTE]
>如果在该特定主机上没有部署任何租户虚拟机，Hyper-v 主机上可能只有两个已建立的连接。

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>检查主机代理服务
网络控制器与 Hyper-v 主机上的两个主机代理服务通信：SLB 主机代理和 NC 主机代理。 其中一项或两项服务可能未运行。 检查其状态，如果未运行，则重新启动。

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>检查网络控制器的运行状况
如果没有三个已建立的连接，或者网络控制器出现无响应状态，请使用以下 cmdlet 查看所有节点和服务模块是否已启动并正在运行。 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
网络控制器服务模块为：
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

检查 ReplicaStatus 是否**准备就绪**，HealthState 是否**正常**。

在生产部署中，对于多节点网络控制器，还可以检查每个服务的主节点及其单个副本状态。

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
检查每个服务的副本状态是否为 "就绪"。

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>检查网络控制器和每个 Hyper-v 主机之间的相应 HostIDs 和证书 
在 Hyper-v 主机上运行以下命令，检查主机 Id 是否与网络控制器上服务器资源的实例 Id 相对应

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

*修正*如果使用 SDNExpress 脚本或手动部署，请在注册表中更新 HostId 密钥，使其与服务器资源的实例 Id 匹配。 如果使用 VMM，请在 Hyper-v 主机（物理服务器）上重新启动网络控制器主机代理，删除 VMM 中的 Hyper-v 服务器并删除主机 Id 注册表项。 然后，通过 VMM 重新添加服务器。


检查 hyper-v 主机（NC 主机代理服务）和网络控制器节点之间的（SouthBound）通信的 Hyper-v 主机（主机名将是证书的使用者名称）所使用的 x.509 证书的指纹是否相同。 同时检查网络控制器的 REST 证书是否具有*CN = <FQDN or IP>* 的使用者名称。

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

你还可以检查每个证书的以下参数，以确保使用者名称是预期的（主机名或 NC REST FQDN 或 IP），证书尚未过期，证书链中的所有证书颁发机构都包含在受信任的根中无权.

- 使用者名称  
- 到期日期  
- 受根证书颁发机构信任  

*修正*如果 Hyper-v 主机上的多个证书具有相同的使用者名称，则网络控制器主机代理会随机选择一个证书以提供给网络控制器。 这可能与网络控制器已知的服务器资源的指纹不匹配。 在这种情况下，请在 Hyper-v 主机上删除一个具有相同使用者名称的证书，然后重新启动网络控制器主机代理服务。 如果仍无法建立连接，请在 Hyper-v 主机上删除具有相同使用者名称的另一个证书，并在 VMM 中删除相应的服务器资源。 然后，在 VMM 中重新创建服务器资源，这将生成新的 x.509 证书，并将其安装在 Hyper-v 主机上。


#### <a name="check-the-slb-configuration-state"></a>检查 SLB 配置状态
可以在 NetworkController cmdlet 的输出中确定 SLB 配置状态。 此 cmdlet 还将输出 JSON 文件中的当前网络控制器资源集、每个 Hyper-v 主机（服务器）的所有 IP 配置和主机代理数据库表中的本地网络策略。 

默认情况下会收集其他跟踪。 若要不收集跟踪，请添加-IncludeTraces： $false 参数。

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>默认输出位置将是 < working_directory > \NCDiagnostics\ 目录。 可以使用 @no__t 的参数更改默认的输出目录。 

可在此目录中的_slbstateResults_文件中找到 SLB 配置状态信息。

此 JSON 文件可细分为以下部分：
 * 构造
   * SlbmVips-此部分列出了网络控制器用于 coodinate SLB Mux 和 SLB 主机代理之间的配置和运行状况的 SLB 管理器 VIP 地址的 IP 地址。
   * MuxState-此部分将列出已部署的每个 SLB Mux 的一个值，提供 mux 的状态
   * 路由器配置-此部分将列出上游路由器的（BGP 对等）自治系统编号（ASN）、传输 IP 地址和 ID。 它还将列出 SLB Mux ASN 和中转 IP。
   * 连接的主机信息-此部分将列出管理 IP 地址可用于运行负载平衡工作负载的所有 Hyper-v 主机。
   * Vip 范围-此部分将列出公用和专用 VIP IP 池范围。 SLBM VIP 将作为分配的 IP 包含在这些范围之一中。 
   * Mux 路由-对于部署的每个 SLB Mux，此部分将列出一个值，其中包含该特定 Mux 的所有路由播发。
 * 租户
   * VipConsolidatedState-此部分将列出每个租户 VIP 的连接状态，包括播发路由前缀、Hyper-v 主机和 DIP 终结点。

> [!NOTE]
> 可以通过使用[MICROSOFT SDN GitHub 存储库](https://github.com/microsoft/sdn)中提供的[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)脚本直接已确定 SLB 状态。 

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

**从网关 VM：**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**从机架顶部（ToR）交换机：**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP 路由器**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

除此之外，从我们到目前为止所见到的问题（尤其是在基于 SDNExpress 的部署上），最常见的原因是，不是在 GW Vm 上配置租户隔离人员尝试分配到 TenantConfig. psd1 中的网络连接（S2S 隧道）。 可以通过比较以下命令的输出来轻松检查此项：

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>宿主验证数据平面
部署网络控制器、创建了租户虚拟网络和子网，并已将 Vm 附加到虚拟子网后，宿主可以执行其他结构级别测试来检查租户连接性。

#### <a name="check-hnv-provider-logical-network-connectivity"></a>检查 HNV 提供程序逻辑网络连接
在 Hyper-v 主机上运行的第一个来宾 VM 已连接到租户虚拟网络后，网络控制器会将两个 HNV 提供程序 IP 地址（PA IP 地址）分配给 Hyper-v 主机。 这些 Ip 来自 HNV 提供程序逻辑网络的 IP 池，由网络控制器管理。  了解这两个 HNV IP 地址是什么

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

这些 HNV 提供程序 IP 地址（PA ip）分配给在单独 TCPIP 网络隔离舱中创建的以太网适配器，适配器名称为_VLANX_ ，其中 X 是分配给 HNV 提供程序（传输）逻辑网络的 VLAN。

使用 HNV 提供程序逻辑网络的两个 Hyper-v 主机之间的连接可以通过具有额外隔离舱（-c Y）参数的 ping 来完成，其中 Y 是创建 PAhostVNICs 的 TCPIP 网络隔离舱。 可以通过执行以下操作来确定此隔离舱：

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
> 数据路径中未使用 PA 主机 vNIC 适配器，因此没有将 IP 分配给 "vEthernet （PAhostVNic）适配器"。

例如，假定 Hyper-v 主机1和2具有 HNV Provider （PA）的 IP 地址：

|-Hyper-v 主机-|-PA IP 地址1|-PA IP 地址2|
|---             |---            |---             |
|主机1 | 10.10.182.64 | 10.10.182.65 |
|主机2 | 10.10.182.66 | 10.10.182.67 |

可以使用以下命令在两者之间进行 ping 操作，以检查 HNV 提供程序逻辑网络连接。

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

*修正*如果 HNV 提供程序 ping 不起作用，请检查物理网络连接，包括 VLAN 配置。 每个 Hyper-v 主机上的物理 Nic 应该处于干线模式，未分配任何特定 VLAN。 管理主机 vNIC 应该与管理逻辑网络的 VLAN 隔离。

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

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>在 HNV 提供程序逻辑网络上检查 MTU 和超长帧支持

HNV 提供程序逻辑网络中的另一个常见问题是，物理网络端口和/或以太网卡没有配置足够的 MTU 来处理 VXLAN （或 NVGRE）封装的系统开销。 
>[!NOTE]
> 某些以太网卡和驱动程序支持新的 * EncapOverhead 关键字，网络控制器主机代理会自动将其设置为160的值。 然后，会将此值添加到 "JumboPacket" 关键字的值，其求和用作播发的 MTU。
> 例如 * EncapOverhead = 160 和 * JumboPacket = 1514 = = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

若要测试 HNV 提供程序逻辑网络是否支持端到端的更大 MTU 大小，请使用_LogicalNetworkSupportsJumboPacket_ cmdlet：
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
* 调整物理交换机端口上的 MTU 大小至少为1674B （包括14B 以太网标头和尾部）
* 如果 NIC 卡不支持 EncapOverhead 关键字，请将 JumboPacket 关键字调整为至少1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>检查租户 VM NIC 连接
分配给来宾 VM 的每个 VM NIC 在专用客户地址（CA）和 HNV 提供商地址（PA）空间之间具有 CA PA 映射。 这些映射保存在每个 Hyper-v 主机上的 OVSDB server 表中，可通过执行以下 cmdlet 来查找。

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
> 如果所需的 CA PA 映射不是针对给定租户 VM 的输出，请使用_NetworkControllerNetworkInterface_ Cmdlet 检查网络控制器上的 VM NIC 和 IP 配置资源。 同时，检查 NC 主机代理和网络控制器节点之间建立的连接。

使用此信息，现在可以通过使用_VirtualNetworkConnection_ Cmdlet 的网络控制器发起 VM 来启动租户 VM ping。

## <a name="specific-troubleshooting-scenarios"></a>特定故障排除方案

以下部分提供了解决特定方案的指南。

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>两个租户虚拟机之间没有网络连接

1.  组织确保租户虚拟机中的 Windows 防火墙不会阻止通信。  
2.  组织通过运行_ipconfig_检查是否已将 IP 地址分配给租户虚拟机。 
3.  宿主从 Hyper-v 主机上运行**VirtualNetworkConnection** ，验证所涉及的两个租户虚拟机之间的连接。 

>[!NOTE]
>VSID 是指虚拟子网 ID。 对于 VXLAN，这是 VXLAN 网络标识符（VNI）。 可以通过运行**PACAMapping** cmdlet 来查找此值。

#### <a name="example"></a>示例

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

在主机 "sa18n30-2.sa18.nttest.microsoft.com" 上，在 "绿色 Web VM 1" 与 SenderCA IP of 192.168.1.4 之间进行 ping 操作，并将管理 IP 10.127.132.153 的管理 IP 连接到虚拟子网（LISTENERCA）4114。

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

启动 CA 空间 ping 测试启动跟踪会话 Ping 到为192.168.1.5 的操作成功从地址 192.168.1.4 Rtt = 0 毫秒


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

4. 组织检查将阻止流量的虚拟子网或 VM 网络接口上是否没有指定的分布式防火墙策略。    

在 sa18.nttest.microsoft.com 域中的 sa18n30nc 上查询在演示环境中找到的网络控制器 REST API。

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>查看正在引用此 ACL 的 IP 配置和虚拟子网

1. 宿主在托管两个有问题的租户虚拟机的 Hyper-v 主机上运行 ``Get-ProviderAddress``，然后从 Hyper-v 主机运行 @no__t 或 ``ping -c <compartment>`` 来验证 HNV 提供程序逻辑网络上的连接
2.  宿主请确保 hyper-v 主机和 Hyper-v 主机之间的任何第2层交换设备上的 MTU 设置正确。 在所有有问题的 Hyper-v 主机上运行 ``Test-EncapOverheadValue``。 还要检查两者之间的所有第2层交换机是否设置为至少1674字节，以最大限度地降低160字节的系统开销。  
3.  宿主如果 PA IP 地址不存在并且/或 CA 连接中断，请检查以确保已收到网络策略。 运行 ``Get-PACAMapping``，查看是否已正确确定创建覆盖虚拟网络所需的封装规则和 CA-PA 映射。  
4.  宿主检查网络控制器主机代理是否已连接到网络控制器。 运行 ``netstat -anp tcp |findstr 6640`` 以查看   
5.  宿主检查 HKLM/中的主机 ID 是否与托管租户虚拟机的服务器资源的实例 ID 匹配。  
6. 宿主检查端口配置文件 ID 是否与租户虚拟机的 VM 网络接口的实例 ID 匹配。  

## <a name="logging-tracing-and-advanced-diagnostics"></a>日志记录、跟踪和高级诊断

以下各节提供了有关高级诊断、日志记录和跟踪的信息。

### <a name="network-controller-centralized-logging"></a>网络控制器集中式日志记录 

网络控制器可以自动收集调试器日志，并将它们存储在一个集中的位置。 首次部署网络控制器或在以后的任何时间都可以启用日志收集。 日志由网络控制器和网络控制器管理的网络元素收集：主机、软件负载平衡器（SLB）和网关计算机。 

这些日志包括网络控制器群集、网络控制器应用程序、网关日志、SLB、虚拟网络和分布式防火墙的调试日志。 每当将新主机/SLB/网关添加到网络控制器时，将在那些计算机上启动日志记录。 同样，从网络控制器中删除主机/SLB/网关后，在这些计算机上将停止日志记录。

#### <a name="enable-logging"></a>启用日志记录

使用**NetworkControllerCluster** Cmdlet 安装网络控制器群集时，将自动启用日志记录。 默认情况下，会在 *%systemdrive%\SDNDiagnostics*的网络控制器节点上本地收集日志。 **强烈建议**将此位置更改为远程文件共享（而非本地）。 

网络控制器群集日志存储在 *%ProgramData%\Windows Fabric\log\Traces*中。 您可以使用**DiagnosticLogLocation**参数为日志收集指定一个集中的位置，并建议该位置也是远程文件共享。 

如果要限制对此位置的访问权限，可以使用**LogLocationCredential**参数提供访问凭据。 如果提供用于访问日志位置的凭据，则还应提供**CredentialEncryptionCertificate**参数，该参数用于对本地存储在网络控制器节点上的凭据进行加密。  

如果使用默认设置，则建议在中心位置至少具有 75 GB 的可用空间，在3节点网络控制器群集的本地节点（如果不使用中央位置）上有 25 GB 的可用空间。

#### <a name="change-logging-settings"></a>更改日志记录设置

你可以随时使用 @no__t cmdlet 更改日志记录设置。 可以更改以下设置：

- **集中式日志位置**。  您可以更改用于存储所有日志的位置，并 ``DiagnosticLogLocation`` 参数。  
- **用于访问日志位置的凭据**。  你可以使用 @no__t 参数将凭据更改为访问日志位置。  
- **转到本地日志记录**。  如果已提供存储日志的集中位置，则可以使用 ``UseLocalLogLocation`` 参数移回本地网络控制器节点上的日志记录（由于磁盘空间需求较大，不建议这样做）。  
- **日志记录范围**。  默认情况下，将收集所有日志。 你可以更改范围以仅收集网络控制器群集日志。  
- **日志记录级别**。  默认日志记录级别为 "信息"。 可以将其更改为 "错误"、"警告" 或 "详细"。  
- **日志老化时间**。  日志以循环方式存储。 默认情况下，你将有3天的日志记录数据，无论你使用的是本地日志记录还是集中式日志记录。 可以通过**LogTimeLimitInDays**参数更改此时间限制。  
- **日志老化大小**。  默认情况下，如果使用集中式日志记录，将拥有最大 75 GB 的日志记录数据，如果使用本地日志记录，则为 25 GB。 可以通过**LogSizeLimitInMBs**参数更改此限制。

#### <a name="collecting-logs-and-traces"></a>收集日志和跟踪

默认情况下，VMM 部署对网络控制器使用集中式日志记录。 部署网络控制器服务模板时，将指定这些日志的文件共享位置。

如果尚未指定文件位置，则将在 C:\Windows\tracing\SDNDiagnostics. 下保存日志的每个网络控制器节点上使用本地日志记录。 使用以下层次结构保存这些日志：

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- 断

网络控制器使用（Azure） Service Fabric。 排查某些问题时可能需要 Service Fabric 日志。 可在 C:\ProgramData\Microsoft\Service Fabric 中的每个网络控制器节点上找到这些日志。

如果用户已运行_NetworkController_ cmdlet，则会在与网络控制器中的服务器资源一起指定的每个 hyper-v 主机上提供其他日志。 这些日志（和跟踪（如果启用）保留在 C:\NCDiagnostics 下。

### <a name="slb-diagnostics"></a>SLB 诊断

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>SLBM 构造错误（托管服务提供程序操作）

1.  检查软件负载平衡器管理器（SLBM）是否正常运行，以及业务流程层是否可以互相通信：SLBM > SLB Mux 和 SLBM > SLB 主机代理。 从具有对网络控制器 REST 终结点的访问权限的任何节点中运行[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) 。  
2.  在其中一个网络控制器节点 Vm （最好是主网络控制器节点-NetworkControllerReplica）上验证*SDNSLBMPerfCounters* in PerfMon：
    1.  负载均衡器（LB）引擎是否已连接到 SLBM？ （*SLBM LBEngine 配置*> 0）  
    2.  SLBM 是否至少知道自己的终结点？ （*VIP 端点总数*> = 2）  
    3.  Hyper-v （DIP）主机是否连接到 SLBM？ （*连接的 HP 客户端*= = num server）   
    4.  SLBM 是否连接到 Mux？ （*Mux 连接* == *MUX 在 @no__t SLBM 上正常运行*-3*mux 报告正常状态*= # SLB mux vm）。  
3.  确保配置的 BGP 路由器已成功与 SLB MUX 对等互连  
    1.  如果将 RRAS 用于远程访问（即 BGP 虚拟机）：  
        1.  Bgp 应显示已连接  
        2.  BgpRouteInformation 应至少显示 SLBM 自 VIP 的路由  
    2.  如果使用物理上架（ToR）交换机作为 BGP 对等互连，请参阅文档  
        1.  例如： # show bgp instance  
4.  在 SLB Mux VM 上验证 PerfMon 中的*SlbMuxPerfCounters*和*SLBMUX*计数器
5.  检查软件负载平衡器管理器资源中的配置状态和 VIP 范围  
    1.  NetworkControllerLoadBalancerConfiguration-ConnectionUri < https：//<FQDN or IP> |convertto-html 8 （在 IP 池中检查 VIP 范围并确保 SLBM 自 VIP （*LoadBalanacerManagerIPAddress*）和任何面向租户的 vip 在这些范围内）  
        1. NetworkControllerIpPool-NetworkId "< 公用/专用 VIP 逻辑网络资源 ID >"-SubnetId "< 公共/专用 VIP 逻辑子网资源 ID >"-ResourceId "<IP Pool Resource Id>"-ConnectionUri $uri | convertto-html-第深度8 
    2.  NetworkControllerConfigurationState-  

如果上述任何检查失败，则租户 SLB 状态也将处于失败模式。  

**修正**   
根据提供的以下诊断信息，修复以下问题：  
* 确保已连接 SLB Multiplexers (  
  * 修复证书问题  
  * 修复网络连接问题  
* 确保已成功配置 BGP 对等互连信息  
* 确保注册表中的主机 ID 与服务器资源中的服务器实例 ID 匹配（ *HostNotConnected*错误代码的参考附录）  
* 收集日志  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>SLBM 租户错误（托管服务提供商和租户操作）

1.  宿主检查*NetworkControllerConfigurationState*以查看是否有任何 LoadBalancer 资源处于错误状态。 请尝试按照附录中的 "操作项" 表进行操作。   
    1.  检查 VIP 终结点是否存在和播发路由  
    2.  检查已为 VIP 终结点发现了多少个 DIP 终结点  
2.  组织验证是否正确指定了负载均衡器资源  
    1.  验证在 SLBM 中注册的 DIP 终结点是否托管与 LoadBalancer 后端地址池 IP 配置相对应的租户虚拟机  
3.  宿主如果未发现或未连接 DIP 终结点：   
    1.  检查*NetworkControllerConfigurationState*  
        1.  使用 @no__t 验证 NC 和 SLB 主机代理是否已成功连接到网络控制器事件协调器  
    2.  检查*nchostagent* service regkey 中的*主机*id （附录中的引用*HostNotConnected*错误代码）是否与相应的服务器资源的实例 Id 匹配（``Get-NCServer |convertto-json -depth 8``）  
    3.  检查虚拟机端口的端口配置文件 id 与相应的虚拟机 NIC 资源的实例 Id 匹配   
4.  [宿主提供程序]收集日志   

#### <a name="slb-mux-tracing"></a>SLB Mux 跟踪

还可以通过事件查看器确定软件负载平衡器 Mux 中的信息。 
1. 单击 "事件查看器视图" 菜单下的 "显示分析和调试日志"。
2. 在事件查看器中导航到 "应用程序和服务日志" > Microsoft > Windows > SlbMuxDriver > 跟踪 
3. 右键单击它并选择 "启用日志"

>[!NOTE]
>建议你仅在尝试重现问题时短时间启用此日志记录

### <a name="vfp-and-vswitch-tracing"></a>VFP 和 vSwitch 跟踪

在承载附加到租户虚拟网络的来宾 VM 的任何 Hyper-v 主机上，可以收集 VFP 跟踪来确定问题可能出现在何处。

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
