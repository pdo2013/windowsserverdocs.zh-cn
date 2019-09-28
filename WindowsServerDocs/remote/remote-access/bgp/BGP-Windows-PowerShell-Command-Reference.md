---
title: BGP Windows PowerShell 命令参考
description: 在编写 Windows Server 2016 中的 Windows PowerShell 脚本时，可以使用本主题作为参考，以便在 RAS 网关和远程访问局域网（LAN）路由器中添加、配置和删除 BGP 功能。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b0240a3-b927-4a1e-b241-5f8f29a9552f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2da5bc3008b9b686ddb2bce460ced39f98183f28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394620"
---
# <a name="bgp-windows-powershell-command-reference"></a>BGP Windows PowerShell 命令参考

>适用于：Windows Server（半年频道）、Windows Server 2016

在编写 Windows PowerShell 脚本时，可以使用本主题作为参考，以便在 RAS 网关和远程访问局域网（LAN）路由器中添加、配置和删除 BGP 功能。  
  
这些 BGP 命令是 Windows Server 2016 的远程访问 Windows PowerShell 命令集的一部分。 本主题将帮助你快速找到要在脚本中使用的 BGP 命令。  
  
有关所有远程访问命令的详细信息，请参阅[远程访问 cmdlet](https://technet.microsoft.com/library/hh918399.aspx)。  
  
## <a name="bgp-command-reference"></a>BGP 命令参考  
以下各节提供了每个 BGP 命令的命令名称、用途和语法，以及指向远程访问引用中命令的链接，其中包含有关每个命令的更多详细信息。  
  
本参考包含以下各节。  
  
-   [添加命令](#bkmk_add)  
  
-   [清除命令](#bkmk_clear)  
  
-   [禁用和启用命令](#bkmk_disable)  
  
-   [获取命令](#bkmk_get)  
  
-   [安装命令](#bkmk_install)  
  
-   [删除命令](#bkmk_remove)  
  
-   [设置命令](#bkmk_set)  
  
-   [启动和停止命令](#bkmk_start)  
  
-   [卸载命令](#bkmk_uninstall)  
  
### <a name="bkmk_add"></a>添加命令  
下面是 BGP Add 命令。  
  
[Add-bgpcustomroute](https://technet.microsoft.com/library/dn262684.aspx)  
  
向 BGP 路由表添加自定义路由。  
  
```  
Add-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Bgp](https://technet.microsoft.com/library/dn262687.aspx)  
  
添加新的 BGP 对等方。  
  
```  
Add-BgpPeer [-Name] <String> -LocalIPAddress <IPAddress> -PeerASN <UInt32> -PeerIPAddress <IPAddress> [-CimSession <CimSession[]> ] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteAggregate](https://technet.microsoft.com/library/mt463113.aspx)  
  
为特定 BGP 路由添加新的聚合路由。  
  
```  
Add-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouter](https://technet.microsoft.com/library/dn262665.aspx)  
  
为指定的租户 ID 添加 BGP 路由器。  
  
```  
Add-BgpRouter -BgpIdentifier <IPAddress> -LocalASN <UInt32> [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicy](https://technet.microsoft.com/library/dn262662.aspx)  
  
将 BGP 路由策略添加到策略存储区。  
  
```  
Add-BgpRoutingPolicy [-Name] <String> [-PolicyType] <PolicyType> {Deny | Allow | ModifyAttribute} [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicyForPeer](https://technet.microsoft.com/library/dn262680.aspx)  
  
将 BGP 路由策略添加到 BGP 对等方。  
  
```  
Add-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_clear"></a>清除命令  
以下是 BGP 的清除命令  
  
[BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463114.aspx)  
  
清除指定 BGP 路由集的路由摆动抑制信息。  
  
```  
Clear-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_disable"></a>禁用和启用命令  
以下是 BGP 的禁用和启用命令  
  
[BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463100.aspx)  
  
为稳定 BGP 路由禁用路由抑制。  
  
```  
Disable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463102.aspx)  
  
为稳定 BGP 路由启用路由抑制。  
  
```  
Enable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_get"></a>获取命令  
下面是针对 BGP 的 Get 命令。  
  
[Add-bgpcustomroute](https://technet.microsoft.com/library/dn262664.aspx)  
  
获取来自 BGP 路由器的自定义路由信息。  
  
```  
Get-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Bgp](https://technet.microsoft.com/library/dn262659.aspx)  
  
获取 BGP 对等节点的配置信息。  
  
```  
Get-BgpPeer [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteAggregate](https://technet.microsoft.com/library/mt463103.aspx)  
  
获取管理员配置的所有聚合 BGP 路由。  
  
```  
Get-BgpRouteAggregate [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463108.aspx)  
  
检索 BGP 路由抑制引擎的配置。  
  
```  
Get-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteInformation](https://technet.microsoft.com/library/dn262667.aspx)  
  
从 BGP 路由表中检索一个或多个网络前缀的 BGP 路由信息。  
  
```  
Get-BgpRouteInformation [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Type <RouteType> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouter](https://technet.microsoft.com/library/dn262660.aspx)  
  
获取 BGP 路由器的配置信息。  
  
```  
Get-BgpRouter [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicy](https://technet.microsoft.com/library/dn262672.aspx)  
  
获取 BGP 路由策略的配置信息。  
  
```  
Get-BgpRoutingPolicy [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-bgpstatistics](https://technet.microsoft.com/library/dn262685.aspx)  
  
检索与 BGP 对等互连相关的消息和路由播发统计信息。  
  
```  
Get-BgpStatistics [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_install"></a>安装命令  
以下是 RAS 网关和 BGP 的安装命令。  
  
[安装-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx)  
  
为 DirectAccess （DA）执行先决条件检查以确保可以安装它、安装用于远程访问的 DA （包括远程客户端管理）或仅对远程客户端进行管理，安装 VPN （远程访问 VPN 和站点到站点 VPN）和安装 BGP 路由。  
  
```  
Parameter Set: MultiTenant  
Install-RemoteAccess [-MultiTenancy] [-CapacityKbps <UInt64> ] [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
  
Parameter Set: Vpn  
Install-RemoteAccess [-VpnType] <String> {Vpn | VpnS2S | SstpProxy | RoutingOnly} [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-IPAddressRange <String[]> ] [-IPv6Prefix <String> ] [-Legacy] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
> [!IMPORTANT]  
> 在多租户模式下安装 RAS 网关时，必须使用**Enable-remoteaccessroutingdomain** Windows PowerShell 命令和 **-Type**参数值**All**来指定是否为每个租户启用 BGP。 下面的示例代码演示了如何在多租户模式下安装 RAS，并为两个租户（Contoso 和 Fabrikam）启用了所有 RAS 功能（点到站点 VPN、站点到站点 VPN 和 BGP 路由）。  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
如果使用远程访问作为 LAN 路由器而不是网关，则仍可使用 BGP，这提供了在 intranet 上进行动态路由的优点。 若要将远程访问安装为 BGP LAN 路由器，请在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
  
```  
Install-RemoteAccess -VpnType RoutingOnly  
```  
  
### <a name="bkmk_remove"></a>删除命令  
以下是 BGP 的删除命令。  
  
[Add-bgpcustomroute](https://technet.microsoft.com/library/dn262669.aspx)  
  
从 BGP 路由器中删除自定义路由。  
  
```  
Remove-BgpCustomRoute [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Bgp](https://technet.microsoft.com/library/dn262675.aspx)  
  
从路由器中删除 BGP 对等方。  
  
```  
Remove-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteAggregate](https://technet.microsoft.com/library/mt463110.aspx)  
  
删除指定聚合 BGP 路由的集合。  
  
```  
Remove-BgpRouteAggregate [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouter](https://technet.microsoft.com/library/dn262678.aspx)  
  
删除 BGP 路由器。  
  
```  
Remove-BgpRouter [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicy](https://technet.microsoft.com/library/dn262656.aspx)  
  
从策略存储中删除路由策略。  
  
```  
Remove-BgpRoutingPolicy [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicyForPeer](https://technet.microsoft.com/library/dn262681.aspx)  
  
从 BGP 对等节点中删除路由策略。  
  
```  
Parameter Set: Remove1  
Remove-BgpRoutingPolicyForPeer [-CimSession <CimSession[]> ] [-Direction <PolicyDirection> {Ingress | Egress} ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-PolicyName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_set"></a>设置命令  
以下是 BGP 的 Set 命令。  
  
[Bgp](https://technet.microsoft.com/library/dn262673.aspx)  
  
更新指定的 BGP 对等节点的配置。  
  
```  
Set-BgpPeer [-Name] <String> [-CimSession <CimSession[]> ] [-ClearPrefixLimit] [-Force] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-LocalIPAddress <IPAddress> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeerASN <UInt32> ] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-PeerIPAddress <IPAddress> ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteAggregate](https://technet.microsoft.com/library/mt463115.aspx)  
  
更新指定聚合 BGP 路由的属性。  
  
```  
Set-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463116.aspx)  
  
配置 BGP 路由抑制引擎。  
  
```  
Set-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-HalfLife <UInt32> ] [-HalfLifeUnreachable <UInt32> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-MaxSuppressTime <UInt32> ] [-PassThru] [-ReuseThreshold <UInt32> ] [-RoutingDomain <String> ] [-SuppressThreshold <UInt32> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouter](https://technet.microsoft.com/library/dn262652.aspx)  
  
为指定的租户 ID 更新本地 BGP 路由器的配置。  
  
```  
Set-BgpRouter [-BgpIdentifier <IPAddress> ] [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalASN <UInt32> ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicy](https://technet.microsoft.com/library/dn262670.aspx)  
  
修改路由策略配置。  
  
```  
Set-BgpRoutingPolicy [-Name] <String> [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RemovePolicyClause <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicyForPeer](https://technet.microsoft.com/library/dn262674.aspx)  
  
为 BGP 对等节点修改 BGP 路由策略。  
  
```  
Set-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_start"></a>启动和停止命令  
以下是 BGP 的启动和停止命令。  
  
[Bgp](https://technet.microsoft.com/library/dn262683.aspx)  
  
开始 BGP 对等节点的路由会话。  
  
```  
Start-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[停止-Bgp](https://technet.microsoft.com/library/dn262661.aspx)  
  
停止 BGP 对等节点的路由会话。  
  
```  
Stop-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_uninstall"></a>卸载命令  
以下是 RAS 网关和 BGP 的卸载命令。  
  
[卸载-RemoteAccess](https://technet.microsoft.com/library/hh918390.aspx)  
  
卸载计算机的远程访问权限，包括所有远程访问特性和功能（RAS 网关、BGP 等）。  
  
```  
Uninstall-RemoteAccess [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-ThrottleLimit <Int32> ] [-VpnType <String> {Vpn | VpnS2S} ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  


