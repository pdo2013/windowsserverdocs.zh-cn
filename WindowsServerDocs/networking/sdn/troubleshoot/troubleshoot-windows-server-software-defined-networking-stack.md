---
title: 解决 Windows Server 软件定义的网络堆栈
description: 本 Windows Server 指南检查常见软件定义网络 (SDN) 的错误和的故障情况和概述利用可用的诊断工具疑难解答流。
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.openlocfilehash: af59ae6746467f9aecf384d1b3cf9af1e8baeb9a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>解决 Windows Server 软件定义的网络堆栈

>适用于：Windows Server（半年通道），Windows Server 2016

本指南检查常见软件定义网络 (SDN) 的错误和的故障情况和概述疑难解答的工作流，利用可用的诊断工具。  

有关 Microsoft 软件定义网络的详细信息，请参阅[软件定义网络](../../sdn/Software-Defined-Networking--SDN-.md)。  

## <a name="error-types"></a>错误类型  
以下列表表示的中从市场中生产部署 Windows Server 2012 R2 的 Hyper-V 网络虚拟 (HNVv1) 使用最常看到的问题类和相符多种方式相同的新软件定义网络 (SDN) 堆栈与 Windows Server 2016 HNVv2 中出现的问题的类型。  

大多数错误可以分成小套类：   
* **无效或不受支持的配置**  
   用户调用 NorthBound API 错误或无效的策略。   

* **策略应用程序中有错误**  
     没有传递到 Hyper-V 主机时，显著延迟和/或所有 Hyper-V 主机（例如之后实时迁移）, 上不是最新的网络控制器的策略。  
* **配置偏移或软件错误**  
 数据路径导致放置数据包的问题。  

* **外部错误相关的到 NIC 硬件 / 驱动程序或包含网络结构**  
 （如 VMQ) 行为错误任务卸载或包含网络结构错误配置（如 MTU)   

 此疑难解答指南检查每个这些错误的类别和建议最佳惯例和诊断工具可用来识别并修复此错误。  

## <a name="diagnostic-tools"></a>诊断工具  

在讨论之前的疑难解答工作流为每台这些类型的错误，让我们来检查可用诊断工具。   
  
若要使用网络控制器（控制路径）诊断工具，必须首先安装 RSAT NetworkController 功能和导入``NetworkControllerDiagnostics``模块：  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

若要使用 HNV 诊断（数据路径）诊断工具，必须导入``HNVDiagnostics``模块：
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>网络控制器诊断  
这些 cmdlet 记录中 TechNet 上[网络控制器诊断 Cmdlet 主题](https://docs.microsoft.com/en-us/powershell/module/networkcontrollerdiagnostics/)。 它们帮助识别控制路径网络控制器节点之间网络控制器和运行的 Hyper-V 主机上 NC 主机代理以及之间的网络策略一致性的问题。

 _调试 ServiceFabricNodeStatus_和_获取 NetworkControllerReplica_ cmdlet 必须在其中一个网络控制器节点虚拟机上运行。 其他所有 NC 诊断 cmdlet 可以从任何已连接到网络控制器和在网络控制器管理安全组 (Kerberos) 中的任何一个或有权访问管理网络控制器 X.509 证书主机都运行。 
   
### <a name="hyper-v-host-diagnostics"></a>Hyper-V 主机诊断  
这些 cmdlet 记录中 TechNet 上[Hyper-V 网络虚拟化 (HNV) 诊断 Cmdlet 主题](https://docs.microsoft.com/en-us/powershell/module/hnvdiagnostics/)。 它们帮助识别租户虚拟机（东月西）之间的数据路径中的问题和通过 SLB VIP（北美/南）入口交通。 

_调试 VirtualMachineQueueOperation_，_获取 CustomerRoute_，_获取 PACAMapping_，_获取 ProviderAddress_，_获取 VMNetworkAdapterPortId_，_获取 VMSwitchExternalPortId_，并_测试 EncapOverheadSettings_是可以从任何 Hyper-V 主机运行的所有本地测试。 其他 cmdlet 调用通过网络控制器数据路径测试，因此需要对网络控制器视为 descried 上方的访问权限。
 
### <a name="github"></a>GitHub
[Microsoft / SDN GitHub 举报](https://github.com/microsoft/sdn)具有多个示例脚本以及在这些收件箱 cmdlet 顶部版本的工作流。 特别是，可以在找到诊断脚本[诊断](https://github.com/Microsoft/sdn/diagnostics)文件夹。 请帮助我们对这些脚本会通过提交拉请求。

## <a name="troubleshooting-workflows-and-guides"></a>故障排除其工作流程和指南  

### <a name="hoster-validate-system-health"></a>[宿主]验证系统运行状况
还有名为 embedded 的资源_配置状态_在多个网络控制器资源。 配置状态提供有关系统运行状况，包括网络控制器配置和 Hyper-V 主机上的实际（运行）状态之间一致性信息。 

若要检查配置的状态，运行以下命令从任何 Hyper-V 主机使用连接到网络控制器。

>[!NOTE] 
>值*NetworkController*参数应是基于 X.509 的对象名称 FQDN 或 IP 地址 > 创建网络控制器的证书。
>
>*凭据*参数只需要指定网络控制器是否正在使用 Kerberos 身份验证（典型 VMM 部署中）。 必须在网络控制器管理安全组的用户的凭据。

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

示例配置状态消息如下所示：

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
> 网络接口资源 SLB 输入混合公交 VM nic 处于错误"虚拟交换机用来-主未连接到控制器"故障状态的系统没有 bug。 如果在 VM NIC 资源 IP 配置 IP 地址设置从公交逻辑网络 IP 池中，可以安全地忽略此错误。 对于网关 HNV 提供商 VM Nic 网络接口资源处于错误"虚拟交换机用来-PortBlocked"故障状态的系统没有第二个 bug。 该错误可能还安全地忽略如果在设置中设置 NIC VM 资源 IP 配置为 null（通过设计）。


下表显示了错误代码、消息和后续操作拍摄基于观察到的配置状态的列表。

  
| **代码**| **消息**| **操作**|  
|:--------:|:-----------:|----------:|  
| 未知| 未知的错误| |  
| HostUnreachable                       | 主计算机不可访问 | 检查网络控制器和主机之间管理网络连接 |  
| PAIpAddressExhausted                  | 用完路径 Ip 地址 | 增加 HNV 提供商逻辑网 IP 池大小 |  
| PAMacAddressExhausted                 | 用完路径 Mac 地址 | 增加 Mac 池范围 |  
| PAAddressConfigurationFailure         | 无法检测到主机州地址 | 检查网络控制器和主机之间管理网络连接。 |  
| CertificateNotTrusted                 | 不受信任的证书  |修复用于与主机的通信的证书。 |  
| CertificateNotAuthorized              | 证书未经授权的情况 | 修复用于与主机的通信的证书。 |  
| PolicyConfigurationFailureOnVfp       | 在配置 VFP 策略失败 | 这是运行时故障。  没有明确变通。 收集日志。 |  
| PolicyConfigurationFailure            | 推送到主机，由于通信失败或其他人的策略中出现故障 NetworkController 中的错误。| 没有明确的操作。  这是由于目标状态处理在网络控制器模块失败。 收集日志。 |  
| HostNotConnectedToController          | 主机尚未连接到网络控制器 | 不可以访问来自网络控制器端口配置文件不会应用主机或主机上。 主机 Id 注册表项匹配实例 ID 服务器资源验证 |  
| MultipleVfpEnabledSwitches            | 有多个 VFp 主机上启用开关  | 删除之一开关，由于网络控制器主机代理仅支持个 vSwitch 启用了 VFP 扩展 |  
| PolicyConfigurationFailure            | 无法为由于证书错误或连接错误 VmNic 推送 VNet 策略  | 如果部署了正确的证书检查（证书主题名称必须匹配的主机 FQDN）。 同时验证主机连接了网络控制器 |  
| PolicyConfigurationFailure            | 无法为由于证书错误或连接错误 VmNic 推送 vSwitch 策略  | 如果部署了正确的证书检查（证书主题名称必须匹配的主机 FQDN）。 同时验证主机连接了网络控制器|
| PolicyConfigurationFailure            | 无法为由于证书错误或连接错误 VmNic 推送防火墙策略 | 如果部署了正确的证书检查（证书主题名称必须匹配的主机 FQDN）。 同时验证主机连接了网络控制器|
| DistributedRouterConfigurationFailure | 在主机 vNic 配置分布式路由器设置失败                          | Tcpip 也会堆栈错误。 可能需要在其报告此错误的服务器上路径和灾难主机 vNICs 清理 |
| DhcpAddressAllocationFailure          | DHCP 地址分配 VMNic 失败                                                    | 检查是否配置 NIC 资源上的静态 IP 地址属性 |  
| CertificateNotTrusted<br>CertificateNotAuthorized | 无法连接到输入混合，由于网络或证书错误 | 检查的错误消息代码中提供的数字代码：这对应于 winsock 错误代码。 证书错误都是精细 (例如，无法验证证书，未经授权的证书，等等。) |  
| HostUnreachable                       | 输入混合是不正常（常见的情况下是 BGPRouter 断开连接） | BGP 等 RRAS（BGP 虚拟机）或顶部机架（或）切换成功将无法访问或不对等。 检查 BGP 软件负载平衡集线器资源和 BGP 等（或或 RRAS 虚拟机）上的设置 |  
| HostNotConnectedToController          | SLB 主机代理未连接  | 检查运行 SLB 主机代理服务;原因，请参阅 SLB 主机代理日志（自动运行）原因，以防 SLBM (NC) 拒绝呈现主机代理运行证书状态将显示跨越的信息  |  
| PortBlocked                           | VFP 端口被阻止时，由于缺少 VNET / ACL 策略 | 检查是否有其他任何错误，这可能导致不配置的策略。 |  
| 重载                            | 重载负载平衡器输入混合  | 输入混合性能问题 |  
| RoutePublicationFailure               | 负载平衡器输入混合未连接到 BGP 路由器 | 检查输入混合是否具有连接 BGP 路由器和，BGP 对等正确设置 |  
| VirtualServerUnreachable              | 负载平衡器输入混合未连接到 SLB 管理器 | 检查 SLBM 和输入混合之间的连接 |  
| QosConfigurationFailure               | 无法配置 QOS 策略 | 查看是否适用于所有 VM 如果使用 QOS 预订足够带宽 |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>检查网络控制器和 Hyper-V 主机（NC 主机代理服务）之间的网络连接
运行*netstat*以下命令以确认有三个之间 NC 主机代理网络控制器节点和一个侦听插槽 Hyper-V 主机上的已建立连接
- 侦听端口 TCP:6640 上 Hyper-V 主机（NC 代理服务主机）
- 从 Hyper-V 两个已建立的连接主机上端口 6640 到临时端口 (> 32000) 上 NC 节点 IP IP
- 一个建立暂时端口上的 Hyper-V 主机 IP 从连接到网络控制器其余上端口 6640 ip 地址

>[!NOTE]
>如果在特定主机上部署没有租户虚拟机，仅可能 Hyper-V 主机上的两个已建立的连接。

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>检查主机代理服务
网络控制器的通信方式与 Hyper-V 主机上的两个主机代理服务：SLB 主机代理和 NC 主机代理。 有可能一个或两个这些服务不运行。 查看他们的状态，并重新启动如果它们不运行。

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>检查网络控制器的运行状况
如果不三个已建立连接或网络控制器显示无响应，查看所有节点和服务模块都都且正在运行通过使用以下 cmdlet。 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
这些网络控制器服务模块包括：
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

查看是否 ReplicaStatus**准备**并且 HealthState**确定**。

在生产部署是使用多个节点网络控制器，你也可以检查每个服务都是在主要哪个节点及个别副本状态。

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
检查是否已准备好副本状态为每个服务。
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>相应主机 Id 和网络控制器和每个 Hyper-V 主机之间的证书检查 
运行以下命令以检查主机 Id 对应网络控制器上的服务器资源实例 Id Hyper-V 主机上

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

*补救*如果使用 SDNExpress 脚本或手动部署，更新主机 Id 注册表项中匹配服务器资源实例 Id。 重新启动网络控制器主机代理 Hyper-V 主机（物理服务器）上，如果使用 VMM，从 VMM 删除 Hyper-V 服务器，并删除主机 Id 注册表项。 然后重新添加 VMM 通过服务器。


检查 X.509 证书（主机将证书的对象名称）Hyper-V 主机 (SouthBound) 之间进行通信的 Hyper-V 主机（NC 主机代理服务）和网络控制器节点使用的指纹都相同。 同时检查网络控制器的其余部分证书已使用者名称*CN =<FQDN or IP>*。

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

你也可以检查每个证书，以确保已使用者名称是以下参数预期（主机或 NC 其余 FQDN 或 IP）尚未过期的证书，并且所有中的证书链的证书颁发机构纳入信任的根颁发机构。

- 主题名称  
- 到期日期  
- 受信任的根证书颁发机构  

*补救*多个证书同名主题 Hyper-V 主机上，如果该网络控制器主机代理将中随机选择另一个用于向网络控制器。 这可能会与服务器资源已知网络控制器的指纹不匹配。 在此情况下，删除某一证书具有相同的主题名称 Hyper-V 主机上，然后重新启动该网络控制器主机代理服务。 如果可以仍无法连接，删除其他证书具有相同的主题名称 Hyper-V 主机上，并删除相应服务器资源 VMM 中。 然后，重新创建中 VMM，这将生成新的 X.509 证书，并将它安装在 Hyper-V 主机上的服务器资源。
  

#### <a name="check-the-slb-configuration-state"></a>检查 SLB 配置状态
作为到 Debug-NetworkController cmdlet 的输出可以确定 SLB 配置的状态。 此 cmdlet 还将输出当前的一套 JSON 文件、所有 IP 配置每个 Hyper-V 主机（服务器）从本地网络策略从主机代理数据库表中的网络控制器资源。 

默认情况下，将收集其他跟踪。 若要不会收集跟踪，添加-IncludeTraces: $false 参数。

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>默认输出位置将 < working_directory > \NCDiagnostics\ 目录。 可通过使用更改默认输出目录`-OutputDirectory`参数。 

可以在中找到 SLB 配置状态信息_诊断 slbstateResults.Json_此目录中的文件。

此 JSON 文件可以分为以下各部分：
 * 结构
   * SlbmVips-此部分中列出的 IP 地址 SLB 管理器 vip 所使用的网络控制器 coodinate 配置和 SLB Muxes 和 SLB 主机代理之间的运行状况。
   * MuxState-此部分中将列表一个值，为每个 SLB 输入混合部署提供输入混合的状态
   * 路由器配置-此部分中将列表上游路由器的（BGP 等）自主系统 (ASN)、公交的 IP 地址和 id。 它还将列出 SLB Muxes ASN 和公交 IP。
   * 连接主机信息的此部分中将列表中管理 IP 地址的所有可用的运行负载平衡工作负载 Hyper-V 主机。
   * Vip 波幅的此部分中将列表公开并专用 VIP IP 池的范围。 将包含在从一个这些范围分配 IP SLBM VIP。 
   * 输入混合路线-此部分中将列表一个值，为每个 SLB 输入混合部署包含所有该特定输入混合路线公布。
 * 租户
   * VipConsolidatedState-此部分中将列表中的连接状态的每个租户 VIP 包括公布路线前缀 Hyper-V 主机和冲动端点。
    
> [!NOTE]
> 可以通过直接确定 SLB 状态[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)脚本适用于[Microsoft SDN GitHub 存储库](https://github.com/microsoft/sdn)。 

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

**从顶部机架（或）切换：**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP 路由器**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

此外，从我们到目前为止（尤其是在过 SDNExpress 基于部署）的问题租户盒未收到配置网关虚拟机的功能的最常见原因似乎在 FabricConfig.psd1 网关容量比小于尝试将分配给网络连接（S2S 隧道）的人们中 TenantConfig.psd1 事实。 这可以轻松地检查通过比较输出以下命令：

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[宿主]验证数据平面
部署了网络控制器、已创建租户虚拟网络和子网，并且已附加到虚拟子网虚拟机的功能后，可以通过主机，以检查租户连接执行其他结构级别测试。

#### <a name="check-hnv-provider-logical-network-connectivity"></a>检查 HNV 提供商逻辑网络连接
在已连接到租户虚拟网络 VM Hyper-V 主机上运行的第一个来宾之后, 网络控制器将为 Hyper-V 主机分配两个 HNV 提供商 IP 地址（州 IP 地址）。 这些 IPs 将来自 HNV 提供商逻辑网络 IP 池，并将由网络控制器。  若要了解什么是这两个 HNV IP 地址的

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

这些 HNV 提供商 IP 地址 (州 IPs) 分配给在单独的 tcpip 也会网络盒中创建的以太网适配器和的适配器名称_VLANX_ X 所在 VLAN 分配给 HNV 提供商（传输）逻辑网络。

使用逻辑网络，可以通过使用其他盒 ping HNV 提供商的两个 Hyper-V 主机之间的连接 (的 c Y) 参数 Y 所在都 PAhostVNICs tcpip 也会网络盒。 可以通过执行确定此盒：

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
> 州主机 vNIC 适配器未使用的数据路径中，因此不具有 IP 分配给"vEthernet (PAhostVNic) 适配器"。

例如，假设 Hyper-V 主机 1 和 2 有 HNV 提供商（州）的 IP 地址：

|Hyper-V 主机|-州 IP 地址 1|-州 IP 地址 2|
|---             |---            |---             |
|主机 1 | 10.10.182.64 | 10.10.182.65 |
|主机 2 | 10.10.182.66 | 10.10.182.67 |

我们可使用以下命令以检查 HNV 提供商逻辑网络连接两者之间 ping。

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

*补救*如果 HNV 提供商 ping 不起作用，则检查包括 VLAN 配置物理网络连接。 每个 Hyper-V 主机上的物理 Nic 应未分配的特定 VLAN 主干模式中。 管理主机 vNIC 应该能独立于管理逻辑网络 VLAN。

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
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>检查 MTU 和巨型框架逻辑 HNV 提供商的网络上的支持

HNV 提供商逻辑网络中的另一个常见问题是物理网络端口和/或以太网卡不具有足够大 MTU 配置为处理从 VXLAN（或 NVGRE）封装开销。 
>[!NOTE]
> 某些以太网卡和驱动程序支持的新 * EncapOverhead 关键字，它将自动通过值为 160 到该网络控制器主机代理设置。 然后，此值将添加到的值 * JumboPacket 关键字用作宣传 MTU 其的总和。
> 例如 * EncapOverhead = 160 和 * JumboPacket = 1514 年 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

若要是否 HNV 提供商逻辑网络支持变得更大 MTU 大小端到端测试，使用_测试 LogicalNetworkSupportsJumboPacket_ cmdlet:
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

*补救*
* 调整 MTU 大小至少应为物理开关端口 1674B（包括 14B 以太网页眉和预告片）
* 如果你的网络接口卡不支持 EncapOverhead 关键字，调整至少应为 JumboPacket 关键字 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>检查租户 VM NIC 连接
分配给访客 VM 每个 VM NIC 具有专用客户地址（加拿大）和 HNV 提供商地址（路径）空间之间有加利福尼亚州对应关系。 这些映射保存在 OVSDB 服务器表中每个 Hyper-V 主机上，并且可以通过执行以下 cmdlet 找到。

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
> 如果你预期的加利福尼亚州映射不输出为给定的租户 VM 中，请检查网络控制器使用 VM NIC 和 IP 配置资源_获取 NetworkControllerNetworkInterface_ cmdlet。 此外，检查 NC 主机代理和网络控制器节点之间的已建立的连接。

使用此信息，租户 VM ping 可立即由启动网络控制器使用宿主_测试 VirtualNetworkConnection_ cmdlet。

## <a name="specific-troubleshooting-scenarios"></a>特定疑难解答方案

以下部分提供指南疑难解答特定的方案。

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>两个租户虚拟机之间的任何网络连接

1.  [租户]请确保在租户虚拟机 Windows 防火墙不会阻止交通。  
2.  [租户]检查的 IP 地址已分配给租户虚拟机通过运行_ipconfig_。 
3.  [宿主]运行**测试 VirtualNetworkConnection**从 Hyper-V 主机，以验证问题中的两个租户虚拟机之间的连接。 

>[!NOTE]
>VSID 指虚拟子网 id。 就 VXLAN，而言，这是 VXLAN 网络标识符 (VNI)。 你可以通过运行找到此值**获取 PACAMapping** cmdlet。

#### <a name="example"></a>示例

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

创建的到主机上与管理 IP"sa18n30-2.sa18.nttest.microsoft.com"的 10.127.132.153 ListenerCA IP 的 192.168.1.5 连接到虚拟子网 (VSID) 4114 192.168.1.4 SenderCA IP"绿色 Web VM 1"之间的 CA ping。

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

从此 CA 空间 ping 测试地址 192.168.1.4 Rtt 从开始到 192.168.1.5 跟踪会话 Ping 成功 = 0 ms


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

对路由的信息：

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65
 
 <snip> ...

4.  [租户]检查存在任何虚拟子网或 VM 网络接口这会阻止交通上指定的分布式的防火墙策略。    

查询 sa18.nttest.microsoft.com 域中，在演示环境 sa18n30nc 在网络控制器 REST API。

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>看一看 IP 配置和虚拟子网其中所引用此 ACL

1. [宿主]运行``Get-ProviderAddress``在这两个 Hyper-V 主机举办这两个租户虚拟机的问题，然后运行``Test-LogicalNetworkConnection``或``ping -c <compartment>``从 Hyper-V 主机，以验证 HNV 提供商逻辑网络上的连接
2.  [宿主]确保 MTU 设置正确 Hyper-V 主机上和任何的第二层切换 Hyper-V 主机之间的设备。 运行``Test-EncapOverheadValue``所有 Hyper-V 主机，述上。 同时检查之间的所有第二层开关已 MTU 设置为最 1674 字节以字节 160 开销最大的帐户。  
3.  [宿主]如果未亮起路径 IP 地址和/或连接 CA 是中断，请确保已收到网络策略。 运行``Get-PACAMapping``以查看是否正确建立封装规则和程序创建覆盖虚拟网络所需的加利福尼亚州映射。  
4.  [宿主]检查网络控制器主机代理已连接到网络控制器。 运行``netstat -anp tcp |findstr 6640``以查看是否   
5.  [宿主]主机 ID HKLM 中查看 / 匹配服务器资源宿主租户虚拟机的实例 ID。  
6. [宿主]检查端口配置文件 ID 匹配租户虚拟机 VM 网络接口实例 ID。  

## <a name="logging-tracing-and-advanced-diagnostics"></a>日志记录、跟踪和高级诊断功能

以下部分提供有关日志记录并跟踪高级诊断功能的信息。

### <a name="network-controller-centralized-logging"></a>网络控制器集中日志记录 
 
网络控制器自动可以收集日志调试程序并将其存储在中心位置。 当你部署网络控制器的第一次或任何时间更高版本时，可启用日志集锦。 日志会从网络控制器、收集和网络元素由网络控制器：主机计算机、软件负载平衡 (SLB) 和网关机。 

这些日志包括网络控制器群集、网络控制器应用程序、网关日志、SLB、虚拟网络和分布式的防火墙调试日志。 新的主/SLB/关添加到网络控制器后，只要在这些计算机上启动日志记录。 同样，从网络控制器删除主机/SLB/关后，在这些计算机上停止记录。

#### <a name="enable-logging"></a>启用日志记录

当你安装了网络控制器群集使用自动启用日志记录**安装 NetworkControllerCluster** cmdlet。 默认情况下，会收集日志本地网络控制器节点上*%systemdrive%\SDNDiagnostics*。 它是**强烈建议**你更改此位置是远程文件共享（而非本地）。 

存储在网络控制器群集日志*%programData%\Windows Fabric\log\Traces*。 你可以指定与日志集合集中**DiagnosticLogLocation**与这也是推荐的参数是远程文件共享。 

如果你希望限制对该位置的访问，你可以提供带有访问凭据**LogLocationCredential**参数。 如果您提供的凭据以访问日志的位置，你应提供**CredentialEncryptionCertificate**参数，用于加密存储在本地网络控制器节点上的凭据。  

使用默认设置，建议，如果你有的本地节点在中心位置，并 25 GB 可用空间至少 75 GB（不使用中心位置）3 节点网络控制器群集。

#### <a name="change-logging-settings"></a>更改登录设置

你可以更改登录设置随时使用``Set-NetworkControllerDiagnostic``cmdlet。 可以更改以下设置：

- **集中日志的位置**。  你可以更改的位置来存储的所有日志``DiagnosticLogLocation``参数。  
- **凭据以访问日志的位置**。  你可以更改的凭据，若要访问日志的位置，``LogLocationCredential``参数。  
- **移动到本地记录**。  如果您已提供用于存储日志集中的位置，可以回退到本地网络控制器节点上登录``UseLocalLogLocation``参数（不推荐由于大型磁盘空间要求）。  
- **日志记录范围**。  默认情况下，会收集所有的日志。 你可以更改范围收集仅网络控制器群集日志。  
- **日志记录级别**。  默认日志记录级别是信息。 你可以将其更改为错误、警告或详细。  
- **日志老化时间**。  日志会存储在循环方式。 无论是使用本地记录还是集中日志记录，默认情况下，将具有 3 天的数据日志记录。 你可以更改具有此时间限制**LogTimeLimitInDays**参数。  
- **日志老化大小**。  默认情况下，你将有数据日志记录的最大的 75 GB，如果使用集中日志记录并 25 GB，如果使用本地日志记录。 你可以更改与此限制**LogSizeLimitInMBs**参数。

#### <a name="collecting-logs-and-traces"></a>收集日志和跟踪

VMM 部署使用默认情况下网络控制器的集中日志记录。 这些日志文件共享位置时部署网络控制器服务模板指定。

如果文件位置尚未指定、本地日志记录将与结合使用每个网络控制器节点上下 C:\Windows\tracing\SDNDiagnostics 保存日志。 使用以下分级保存这些日志：

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- 跟踪

网络控制器使用 (Azure) 服务结构。 解决某些问题时，可能需要服务结构日志。 在 C:\ProgramData\Microsoft\Service 结构每个网络控制器节点上找不到这些日志。

如果用户是否已运行_调试 NetworkController_ cmdlet，将已指定与服务器资源网络控制器在每个 Hyper-V 主机上提供更多日志。 下 C:\NCDiagnostics 保持这些日志（和跟踪如果已启用）

### <a name="slb-diagnostics"></a>SLB 诊断

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>SLBM 结构错误（托管服务提供商操作）

1.  请检查软件负载平衡管理器 (SLBM) 是否正常工作和业务流程层可以彼此响应：SLBM-> SLB 输入混合和 SLBM-> SLB 主机代理。 运行[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)从任何节点有权访问网络控制器其余端点。  
2.  验证*SDNSLBMPerfCounters*某个网络控制器节点虚拟机（最好的主网络控制器节点-Get-NetworkControllerReplica）PerfMon 中：
    1.  连接到 SLBM 负载平衡（磅）的引擎？ (*SLBM LBEngine 配置总*> 0)  
    2.  并不 SLBM 至少了解自己端点？ (*VIP 端点总计*> = 2)  
    3.  连接到 SLBM Hyper-V（冲动）主机？ (*连接 HP 客户端*= num 服务器 =)   
    4.  连接到 Muxes SLBM？ (*Muxes 连接* == *上 SLBM Muxes 健康* == *Muxes 报告健康*= # SLB Muxes 虚拟机的功能)。  
3.  确保已成功对与 SLB 输入混合等 BGP 路由器配置  
    1.  如果 RRAS 使用远程访问（即 BGP 虚拟机）：  
        1.  Get-BgpPeer 应显示连接  
        2.  Get-BgpRouteInformation 应显示至少为路线为 SLBM 自行 VIP  
    2.  如果使用物理顶部的-机架（或）切换为 BGP 等，请咨询你的文档  
        1.  例如: # 显示 bgp 实例  
4.  验证*SlbMuxPerfCounters*和*SLBMUX*中上 SLB 输入混合 VM PerfMon 计数器
5.  检查配置状态和资源软件负载平衡管理器中的 VIP 范围  
    1.  Get-NetworkControllerLoadBalancerConfiguration ConnectionUri < https://<FQDN or IP>|convertto json-深度 8 (检查 VIP 区域中 IP 池，并确保 SLBM 自行-VIP (*LoadBalanacerManagerIPAddress*) 并且任何租户面向 VIPs 这些范围内)  
        1. Get-NetworkControllerIpPool NetworkId"< 公众/专用 VIP 逻辑网络资源 ID >"-"< 公众/专用 VIP 逻辑子网资源 ID >"SubnetId-资源 Id"<IP Pool Resource Id>"-ConnectionUri $uri | convertto json-深度 8 
    2.  Debug-NetworkControllerConfigurationState-  

如果任何失效上方的检查，租户 SLB 状态也将失败模式中。  

**补救**   
根据下面提供的诊断信息，可解决以下：  
* 请确保已连接 SLB 路复  
  * 修复证书的问题  
  * 解决网络连接问题  
* 确保成功配置 BGP 等信息  
* 确保在注册表中主机 ID 匹配服务器资源中 Server 实例 ID (参考附录*HostNotConnected*错误代码)  
* 收集日志  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>SLBM 租户错误（托管服务提供商和租户操作）

1.  [宿主]检查*调试 NetworkControllerConfigurationState*以查看是否存在任何负载平衡器资源错误状态。 尝试缓解按照即席附录表。   
    1.  检查存在和广告路线 VIP 端点  
    2.  查看发现了多少冲动端点 VIP 端点  
2.  [租户]验证负载平衡资源指定正确  
    1.  验证冲动端点 SLBM 中注册了这举办租户虚拟机，分别对应于负载平衡器后端地址池 IP 配置  
3.  [宿主]如果未发现冲动端点或连接：   
    1.  检查*调试 NetworkControllerConfigurationState*  
        1.  验证该 NC 和 SLB 主机代理成功是连接到网络控制器事件处理协调器使用 ``netstat -anp tcp |findstr 6640)``  
    2.  检查*主机 Id*中*nchostagent*服务注册密钥 (参考*HostNotConnected*附录中的错误代码) 匹配相应服务器资源实例 Id (``Get-NCServer |convertto-json -depth 8``)  
    3.  检查虚拟机端口端口配置文件号码匹配相应虚拟机 NIC 资源的实例 Id   
4.  [举办的提供商]收集日志   

#### <a name="slb-mux-tracing"></a>SLB 输入混合跟踪

此外可以通过事件查看器取决于从软件负载平衡 Muxes 的信息。 
1. 单击"显示分析和调试日志"下事件查看器视图菜单
2. 导航到"应用程序和服务日志"> Microsoft > Windows > SlbMuxDriver > 跟踪事件查看器中 
3. 右键单击，然后选择"启用记录"

>[!NOTE]
>建议你只能在你尝试重现问题时，很短的时间启用此日志记录

### <a name="vfp-and-vswitch-tracing"></a>VFP 和 vSwitch 跟踪

从集成 VM 附加到租户虚拟网络来宾任何 Hyper-V 主机，可以收集 VFP 跟踪，以确定可能所在的问题。

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
