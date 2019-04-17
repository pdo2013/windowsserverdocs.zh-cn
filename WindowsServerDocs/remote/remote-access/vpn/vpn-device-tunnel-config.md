---
title: Windows 10 中配置 VPN 设备隧道
description: 了解如何在 Windows 10 中创建 VPN 设备隧道。
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 005721873ad3a0df942bc9e23eba13728965ccba
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067031"
---
# Windows 10 中配置 VPN 设备隧道

>适用于： Windows 10 版本 1709

始终启用 VPN 使你能够创建设备或计算机的专用的 VPN 配置文件。 始终启用 VPN 连接包括两种类型的隧道： 

- _设备隧道_连接到指定的 VPN 服务器之前用户登录到设备。 预登录连接方案和设备管理目的使用设备隧道。

- _用户隧道_连接仅在用户登录到设备后。 用户隧道允许用户通过 VPN 服务器访问组织的资源。

与_用户隧道_，这仅连接到设备或计算机用户登录后，_设备隧道_允许 VPN 以在用户下次登录之前建立连接。 _设备隧道_和_用户隧道_独立运行其 VPN 配置文件可以在同一时间进行连接，可以根据需要使用不同的身份验证方法和其他 VPN 配置设置。 用户隧道支持 SSTP 和 IKEv2，并且仅与 SSTP 回退不支持的设备隧道支持 IKEv2。

用户隧道上支持已加入域的已加入域的 （工作组） 或 Azure AD – 设备以允许企业版和 BYOD 方案。 可用的所有 Windows 版本，并且适用于通过支持 UWP VPN 插件的第三方的平台功能。

仅可以在运行 Windows 10 企业版或教育版版本 1709年或更高版本的已加入域的设备上配置设备隧道。 没有支持的设备隧道的第三方控件。


## 设备隧道要求和功能
你必须启用 VPN 连接的计算机证书身份验证，并定义的根证书颁发机构进行身份验证传入的 VPN 连接。 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![设备隧道功能和要求](../../media/device-tunnel-feature-and-requirements.png)

## VPN 设备隧道配置

通过设备隧道，下面的 XML 提供良好指南其中仅客户端发起松开方案的示例配置文件是必需的。  流量筛选器均以管理通信仅限制设备隧道。  此配置适用于 Windows 更新、 典型组策略 (GP) 和 System Center Configuration Manager (SCCM) 更新方案，以及 VPN 连接，而无需缓存的凭据，首次登录或密码重置的方案。 

服务器启动推送的情况下，如 Windows 远程管理 (WinRM)、 远程 GPUpdate 和远程 SCCM 更新方案 – 你必须允许入站的流量上设备隧道，因此不能用于流量筛选器。  如果你打开流量筛选器的设备隧道配置文件，然后设备隧道拒绝的入站的流量。  此项限制将在未来版本中删除。


### 示例 VPN profileXML

下面是示例 VPN profileXML。

``` xml
<VPNProfile>  
  <NativeProfile>  
<Servers>vpn.contoso.com</Servers>  
<NativeProtocolType>IKEv2</NativeProtocolType>  
<Authentication>  
  <MachineMethod>Certificate</MachineMethod>  
</Authentication>  
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
 <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
<DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
  </NativeProfile> 
  <!-- use host routes(/32) to prevent routing conflicts -->  
  <Route>  
<Address>10.10.0.2</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
  <Route>  
<Address>10.10.0.3</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
<!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
  <TrafficFilter>  
<RemoteAddressRanges>10.10.0.2, 10.10.0.3</RemoteAddressRanges>  
  </TrafficFilter>
<!-- need to specify always on = true --> 
  <AlwaysOn>true</AlwaysOn> 
<!-- new node to specify that this is a device tunnel -->  
 <DeviceTunnel>true</DeviceTunnel>
<!--new node to register client IP address in DNS to enable manage out -->
<RegisterDNS>true</RegisterDNS>
</VPNProfile>
```

根据每个特定部署方案的需要，可通过设备隧道配置的另一个 VPN 功能是[受信任的网络检测](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection)。

```
 <!-- inside/outside detection --> 
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection> 
```

## 部署和测试

你可以通过使用 Windows PowerShell 脚本，并使用 Windows Management Instrumentation \(WMI\) 桥配置设备隧道。 始终启用 VPN 设备隧道必须在**本地系统**帐户的上下文中进行配置。 若要实现此目的，将需要使用[PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec)，包含实用程序[Sysinternals](https://docs.microsoft.com/sysinternals/)套在[PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools)之一。

有关如何部署指南每台设备`(.\Device)`与每个用户`(.\User)`配置文件，请参阅[使用 PowerShell 脚本和 WMI Bridge 提供程序](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider)。 

运行以下 Windows PowerShell 命令，以验证已成功部署了设备配置文件：

    `Get-VpnConnection -AllUserConnection`

输出显示在设备上部署的 device\ 级 VPN 配置文件的列表。


### 示例 Windows PowerShell 脚本

你可以使用以下 Windows PowerShell 脚本以帮助创建你自己的脚本创建配置文件。

```PowerShell
Param(
[string]$xmlFilePath,
[string]$ProfileName
)

$a = Test-Path $xmlFilePath
echo $a

$ProfileXML = Get-Content $xmlFilePath

echo $XML

$ProfileNameEscaped = $ProfileName -replace ' ', '%20'

$Version = 201606090004

$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'

$nodeCSPURI = './Vendor/MSFT/VPNv2'
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"

$session = New-CimSession

try
{
$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
$newInstance.CimInstanceProperties.Add($property)

$session.CreateInstance($namespaceName, $newInstance)
$Message = "Created $ProfileName profile."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}
$Message = "Complete."
Write-Host "$Message"
```

## 其他资源

以下是其他资源来帮助你的 VPN 部署。

### VPN 客户端配置资源

这些是 VPN 客户端配置资源。

- [如何创建 VPN 配置文件在 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [配置 Windows 10 客户端始终启用 VPN 连接](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [VPN 配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### 远程访问服务器 \(RAS\) 网关资源

以下是 RAS 网关资源。

- [使用计算机身份验证证书配置 RRAS](https://technet.microsoft.com/library/dd458982.aspx)
- [IKEv2 VPN 连接疑难解答](https://technet.microsoft.com/library/dd941612.aspx)
- [配置基于 IKEv2 的远程访问](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>使用时设备隧道 Microsoft RAS 网关，你将需要将 RRAS 服务器配置为支持通过启用**IKEv2 的允许计算机证书身份验证**身份验证方法，如所述的 IKEv2 计算机证书身份验证[此处](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29)。 启用此设置后，强烈建议**设置 VpnAuthProtocol** PowerShell cmdlet，以及**RootCertificateNameToAccept**可选参数，用于确保仅为允许 RRAS IKEv2 连接VPN 客户端的证书为显式定义内部/专用根证书颁发机构。 或者，应修改 RRAS 服务器上的**受信任的根证书颁发机构**存储，以确保它不讨论[此处](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/)为包含公共证书颁发机构。 类似的方法可能还需要考虑其他 VPN 网关。

---
