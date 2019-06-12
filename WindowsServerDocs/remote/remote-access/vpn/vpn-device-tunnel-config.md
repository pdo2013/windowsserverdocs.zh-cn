---
title: 在 Windows 10 中配置 VPN 设备隧道
description: 了解如何在 Windows 10 中创建的 VPN 设备隧道。
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 989216f90e78689b464240cff957bab1d9c1229b
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749568"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>在 Windows 10 中配置 VPN 设备隧道

>适用于：Windows 10 版本 1709

Always On VPN 使你能够创建设备或计算机的专用的 VPN 配置文件。 Always On VPN 连接包括两种类型的隧道： 

- _设备隧道_连接到指定服务器之前用户登录到设备的 VPN 服务器。 预登录连接方案进行设备管理使用设备隧道。

- _用户隧道_连接仅在用户登录到设备后。 用户隧道允许用户通过 VPN 服务器访问组织资源。

与不同_用户隧道_，它仅在用户登录到设备或计算机之后, 连接_设备隧道_允许 VPN 用户登录之前建立的连接。 这两_设备隧道_并_用户隧道_独立操作，使用自己的 VPN 配置文件，可以在同一时间连接并可以使用不同的身份验证方法和其他 VPN 配置设置根据需要。 用户隧道支持 SSTP 和 IKEv2，并设备隧道仅与不支持 SSTP 回退支持 IKEv2。

用户隧道支持在已加入域的已加入域的 （工作组） 或加入 Azure AD-设备为允许企业和 BYOD 方案。 它可在所有 Windows 版本中，并且平台功能都适用于 UWP VPN 插件支持通过第三方。

只能在运行 Windows 10 企业版或教育版本 1709年或更高版本的已加入域的设备上配置设备隧道。 没有设备隧道的第三方控件的支持。


## <a name="device-tunnel-requirements-and-features"></a>设备隧道要求和功能
必须启用计算机证书身份验证的 VPN 连接，并定义根证书颁发机构进行身份验证传入的 VPN 连接。 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![设备隧道功能和要求](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>隧道的 VPN 设备配置

通过设备隧道，下面的 XML 提供了正确的指导，其中仅客户端启动拉取方案的示例配置文件是必需的。  流量筛选器用于限制为仅管理通信设备隧道。  此配置非常适用于 Windows 更新、 典型组策略 (GP) 和 System Center Configuration Manager (SCCM) 更新方案，以及如果没有缓存的凭据，第一次登录的 VPN 连接或密码重置方案。 

由服务器启动推送方案中，如 Windows 远程管理 (WinRM)、 远程 GPUpdate 和远程 SCCM 更新方案 – 必须允许入站的流量上设备隧道，因此无法使用流量筛选器。  如果设备隧道配置文件中启用流量筛选器，然后设备隧道拒绝入站的流量。  此限制将在未来版本中删除。


### <a name="sample-vpn-profilexml"></a>示例 VPN profileXML

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

根据每个特定部署方案的需要，可以使用设备隧道配置的另一个 VPN 功能是[受信任网络检测](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection)。

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>部署和测试

可以通过使用 Windows PowerShell 脚本，并使用 Windows Management Instrumentation (WMI) 桥配置设备隧道。 必须在的上下文中配置 Always On VPN 设备隧道**本地系统**帐户。 若要实现此目的，它将使用所需[PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec)、 利用某个的[PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools)中包含[Sysinternals](https://docs.microsoft.com/sysinternals/)套件的实用程序。

有关如何部署的指导原则的每个设备`(.\Device)`与每个用户`(.\User)`配置文件，请参阅[使用 PowerShell 脚本使用 WMI 桥提供程序](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider)。

运行以下 Windows PowerShell 命令来验证你已成功部署的设备配置文件：

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

输出显示设备的列表\-宽的设备部署的 VPN 配置文件。

### <a name="example-windows-powershell-script"></a>Windows PowerShell 脚本示例

以下 Windows PowerShell 脚本可用于帮助您创建您自己的配置文件创建的脚本。

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

## <a name="additional-resources"></a>其他资源

以下是其他资源来帮助进行 VPN 部署。

### <a name="vpn-client-configuration-resources"></a>VPN 客户端配置资源

以下是 VPN 客户端配置资源。

- [如何创建 VPN 配置文件在 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [配置 Windows 10 客户端 Always On VPN 连接](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [VPN 配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>远程访问服务器网关资源

以下是远程访问服务器 (RAS) 网关资源。

- [使用计算机身份验证证书配置 RRAS](https://technet.microsoft.com/library/dd458982.aspx)
- [IKEv2 VPN 连接故障排除](https://technet.microsoft.com/library/dd941612.aspx)
- [配置基于 IKEv2 的远程访问](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>当使用 Microsoft RAS 网关设备隧道，将需要配置 RRAS 服务器以支持 IKEv2 计算机证书身份验证，从而**允许对 IKEv2 的计算机证书身份验证**身份验证方法，如所述[此处](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29)。 一旦启用此设置，强烈建议**集 VpnAuthProtocol** PowerShell cmdlet，连同**RootCertificateNameToAccept**可选参数，用于确保RRAS IKEv2 连接只被允许的 VPN 客户端证书链接到显式定义内部/专用根证书颁发机构。 或者，**受信任的根证书颁发机构**RRAS 服务器上的存储应已修正，以确保它不包含公共证书颁发机构，如所述[此处](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/)。 类似的方法可能还需要考虑其他 VPN 网关。

