---
title: 配置 Windows 10 客户端始终启用 VPN 连接
description: 此步骤中，了解 ProfileXML 选项和架构，并配置 Windows 10 客户端计算机与该基础结构与 VPN 连接进行通信。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: fdf640d7e98781ca054ab982027bdfe8c3fb64d6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749626"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>步骤 6： 配置 Windows 10 客户端 Always On VPN 连接

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

- [**上一：** 步骤 5：配置 DNS 和防火墙设置](vpn-deploy-dns-firewall.md)<br>
- [**下一步：** 步骤 7：（可选）针对 VPN 连接使用 Azure AD 条件性访问](../../ad-ca-vpn-connectivity-windows10.md)

此步骤中，将了解 ProfileXML 选项和架构，并配置 Windows 10 客户端计算机与该基础结构与 VPN 连接进行通信。

你可以配置 Always On VPN 客户端通过 PowerShell、 SCCM 或 Intune。 所有这三个需要 XML VPN 配置文件来配置适当的 VPN 设置。 自动化 PowerShell 注册为不使用 SCCM 或 Intune 的组织有可能。

>[!NOTE]
>组策略不包括管理模板配置 Windows 10 的远程访问始终在 VPN 客户端。  但是，可以使用登录脚本。

## <a name="profilexml-overview"></a>ProfileXML 概述

ProfileXML 是 VPNv2 CSP 中的 URI 节点。 而不是单独配置每个 VPNv2 CSP 节点，如触发器路由列表和身份验证协议-使用此节点来配置 Windows 10 VPN 客户端提供作为单个 XML 块到单个 CSP 节点的所有设置。 ProfileXML 架构的架构匹配的 VPNv2 CSP 节点几乎相同，但一些术语稍有不同。

描述此部署，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune 的所有交付方法中使用 ProfileXML。 有两种方法在此部署中配置 ProfileXML VPNv2 CSP 节点：

- **OMA DM**。 一种方法是使用 MDM 提供程序使用 OMA DM，如前面部分中讨论[VPNv2 CSP 节点](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes)。 使用此方法，可以轻松地将 VPN 配置文件配置 XML 标记插入 ProfileXML CSP 节点使用 Intune 时。

- **Windows Management Instrumentation (WMI) 到 CSP 桥**。 配置 ProfileXML CSP 节点的第二种方法是使用 WMI CSP 桥 — 调用 WMI 类**MDM_VPNv2_01**— VPNv2 CSP 和 ProfileXML 节点能够访问。 当创建该 WMI 类的新实例时，WMI 使用 CSP 使用 Windows PowerShell 和 System Center Configuration Manager 时创建的 VPN 配置文件。

即使这些配置方法的行为有所不同，这两要求的格式正确的 XML VPN 配置文件。 若要使用 ProfileXML VPNv2 CSP 设置，请使用 ProfileXML 架构配置简单的部署方案所需标记构造 XML。 有关详细信息，请参阅[ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd)。

你在下面找到的每个所需的设置和其相应的 ProfileXML 标记。 在 ProfileXML 架构中的特定标记中配置每个设置并不是全部位于本机配置文件下。 其他标记放置虚拟机，请参阅 ProfileXML 架构。

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**连接类型：** Native IKEv2

ProfileXML 元素：

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**路由：** 分拆隧道

ProfileXML 元素：

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**名称解析：** 域名称信息列表和 DNS 后缀

ProfileXML 元素：

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**触发：** 始终启用以及受信任网络检测

ProfileXML 元素：

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**身份验证：** PEAP TLS 的受 TPM 保护用户证书

ProfileXML 元素：

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

可以使用简单的标记来配置某些 VPN 身份验证机制。 但是，EAP 和 PEAP 是更为复杂。 若要创建 XML 标记的最简单方法是使用其 EAP 设置，来配置 VPN 客户端，然后将该配置导出到 XML。

有关 EAP 设置的详细信息，请参阅[EAP 配置](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration)。

## <a name="manually-create-a-template-connection-profile"></a>手动创建的模板连接配置文件

在此步骤中，您可以使用受保护的可扩展身份验证协议 (PEAP) 来保护客户端和服务器之间的通信。 与不同的是简单的用户名和密码，此连接需要在要工作的 VPN 配置文件中的唯一 EAPConfiguration 部分。

而不是介绍如何从头开始创建 XML 标记，用于设置在 Windows 中创建 VPN 配置文件的模板。 在创建 VPN 配置文件的模板，您使用 Windows PowerShell 使用从该模板来部署更高版本中创建部署最终 ProfileXML EAPConfiguration 部分。

### <a name="record-nps-certificate-settings"></a>记录 NPS 证书设置

在创建模板之前, 记下的主机名或从服务器的证书对 NPS 服务器的完全限定的域名 (FQDN) 和颁发证书的 CA 的名称。

**过程：**

1. 在 NPS 服务器上，打开网络策略服务器。

2. 在 NPS 控制台中，在策略中单击**网络策略**。

3. 右键单击**虚拟专用网络 (VPN) 连接**，然后单击**属性**。

4. 单击**约束**选项卡，然后单击**身份验证方法**。

5. 在 EAP 类型中，单击**Microsoft:受保护的 EAP (PEAP)** ，然后单击**编辑**。

6. 记录的值**颁发给证书**并**颁发者**。

    即将推出的 VPN 模板配置中使用这些值。 例如，如果服务器的 FQDN 是 nps01.corp.contoso.com 和主机名是 NPS01，证书名称基于服务器的 FQDN 或 DNS 名称 — 例如，nps01.corp.contoso.com。

7. 取消编辑受保护的 EAP 属性对话框。

8. 取消虚拟专用网络 (VPN) 连接属性对话框。

9. 关闭网络策略服务器。

>[!NOTE]
>如果有多个 NPS 服务器，完成每个这些步骤，以便 VPN 配置文件可以验证每个应使用它们。

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>在已加入域的客户端计算机上配置 VPN 配置文件的模板

现在，已加入域的客户端计算机上配置 VPN 配置文件的模板所需的信息。 类型的用户帐户对此过程的一部分使用 （也就是说，标准用户或管理员） 并不重要。

但是，如果配置证书自动注册以来尚未重新启动计算机，请在配置 VPN 连接，以确保具有可用的证书在其上注册的模板之前。

>[!NOTE]
>没有方法来将 VPN，如 NRPT 规则，Always On 任何高级的属性手动添加受信任网络检测，等等。在下一步的步骤中，创建测试 VPN 连接，以验证 VPN 服务器的配置，并且可以建立 VPN 连接到服务器。

**手动创建单个测试 VPN 连接**

1.  作为的成员登录到已加入域的客户端计算机**VPN 用户**组。

2.  在开始菜单中，键入**VPN**，然后按 Enter。

3.  在详细信息窗格中，单击**添加 VPN 连接**。

4.  在 VPN 提供商列表中，单击**Windows （内置）** 。

5.  在连接名称键入**模板**。

6.  在服务器名称或地址，键入**外部**VPN 服务器的 FQDN (例如， **vpn.contoso.com**)。

7.  单击“保存”  。

8.  在相关设置中，单击**更改适配器选项**。

9.  右键单击**模板**，然后单击**属性**。

10. 上**安全**选项卡上，在**类型的 VPN**，单击**IKEv2**。

11. 在数据加密，请单击**最大强度加密**。

12. 单击**使用可扩展身份验证协议 (EAP)** ; 然后，在**使用可扩展身份验证协议 (EAP)** ，单击**Microsoft:受保护的 EAP (PEAP) （已启用加密）** 。

13. 单击**属性**打开受保护的 EAP 属性对话框中，并完成以下步骤：

    a. 在中**连接到这些服务器**框中，键入之前在本部分中 (例如，NPS01) 的 NPS 服务器身份验证设置中检索到的 NPS 服务器的名称。

    >[!NOTE]
    >您键入的服务器名称必须与证书中的名称匹配。 恢复此名称前面在本部分中。 如果名称不匹配，则连接将失败，指出，"，连接被阻止由于 RAS/VPN 服务器上配置的策略。"

    b.  在受信任的根证书颁发机构下选择的根 CA 颁发 NPS 服务器证书 (例如，contoso 的 CA)。

    c.  在连接之前通知中，单击**不要求用户授权新服务器或受信任的 Ca**。

    d.  在选择身份验证方法中，单击**智能卡或其他证书**，然后单击**配置**。 智能卡或其他证书属性对话框将打开。

    e.  单击**在此计算机上使用证书**。

    f.  在连接到这些服务器中，输入在前面步骤中的 NPS 服务器身份验证设置中检索到的 NPS 服务器的名称。

    g.  在受信任的根证书颁发机构下选择根 CA 颁发 NPS 服务器证书。

    h.  选择**不提示用户授权新服务器或受信任的证书颁发机构**复选框。

    i.  单击**确定**关闭智能卡或其他证书属性对话框。

    j.  单击**确定**以关闭受保护的 EAP 属性对话框。

14. 单击**确定**以关闭模板属性对话框。

15. 关闭网络连接窗口。

16. 在设置中，通过单击测试 VPN**模板**，然后单击**Connect**。

>[!IMPORTANT]
>请确保模板与你的 VPN 服务器的 VPN 连接成功。 这样做可确保 EAP 设置正确，然后在下一个示例中使用它们。 必须至少一次然后再继续; 连接否则，该配置文件将不包含连接到 VPN 所必需的所有信息。

## <a name="create-the-profilexml-configuration-files"></a>创建 ProfileXML 配置文件

完成本部分之前, 请确保已创建并测试模板 VPN 连接的部分[手动创建的模板连接配置文件](#manually-create-a-template-connection-profile)描述。 测试 VPN 连接，才可确保配置文件包含到 VPN 连接所需的所有信息。

列表 1 中的 Windows PowerShell 脚本创建在桌面上，两个文件都包含**EAPConfiguration**标记基于以前创建的模板连接配置文件：

- **VPN_Profile.xml.** 此文件包含 VPNv2 CSP 中配置 ProfileXML 节点所需的 XML 标记。 使用 Intune 等 OMA DM – 兼容 MDM 服务使用此文件。

- **VPN_Profile.ps1.** 此文件是可以运行的 Windows PowerShell 脚本来配置 ProfileXML 节点 VPNv2 CSP 中的客户端计算机上。 此外可以通过部署此脚本通过 System Center Configuration Manager 中配置的 CSP。 无法在远程桌面会话，包括 HYPER-V 增强的会话中运行此脚本。

>[!IMPORTANT]
>下面的示例命令需要 Windows 10 内部版本 1607年或更高版本。

**创建 VPN_Profile.xml 和 VPN_Proflie.ps1**

1. 登录到包含该模板具有相同的用户的 VPN 配置文件的已加入域的客户端计算机帐户，部分[手动创建的模板连接配置文件](#manually-create-a-template-connection-profile)所述。

2. 将列表 1 中粘贴到 Windows PowerShell 集成脚本环境 (ISE) 中，并自定义在注释中所述的参数。 这些是 $Template、 $ProfileName、 $Servers、 $DnsSuffix、 $DomainName、 $TrustedNetwork 和 $DNSServers。 在注释是每个设置的完整说明。

3. 运行脚本，以生成**VPN_Profile.xml**并**VPN_Profile.ps1**在桌面上。

#### <a name="listing-1-understanding-makeprofileps1"></a>代码清单 1。 了解 MakeProfile.ps1

本部分介绍可用于深入了解有关如何创建 VPN 配置文件，专用于配置 ProfileXML VPNv2 CSP 中的示例代码。

此示例代码从组合脚本并运行该脚本后，该脚本将生成两个文件：VPN_Profile.xml 和 VPN_Profile.ps1。 使用 VPN_Profile.xml OMA DM 符合 MDM 服务，如 Microsoft Intune 中配置 ProfileXML。

使用**VPN_Profile.ps1** ProfileXML 配置 Windows 10 桌面版上的 Windows PowerShell 或 System Center Configuration Manager 中的脚本。

>[!NOTE]
>若要查看完整的示例脚本，请参阅明[MakeProfile.ps1 完整脚本](#makeprofileps1-full-script)。

#### <a name="parameters"></a>Parameters

配置以下参数：

**$Template**。 要从其中检索 EAP 配置的模板的名称。

**$ProfileName**。 配置文件的唯一字母数字标识符。 配置文件名称不得包含正斜杠 （/）。 如果配置文件名称包含空格或其他非字母数字字符，它必须正确转义根据 URL 编码标准。

**$Servers**。 公共或可路由 IP 地址或 VPN 网关的 DNS 名称。 网关的 IP 或服务器场的虚拟 IP，它可以指向外部。 示例，208.147.66.130 或 vpn.contoso.com。

**$DnsSuffix**. 指定一个或多个逗号分隔的 DNS 后缀。 在列表中的第一个还用作 VPN 接口的主连接特定 DNS 后缀。 整个列表也将添加到 SuffixSearchList。

**$DomainName**。 用于指示该策略应用到的命名空间。 当发出名称查询时，DNS 客户端会比较下 DomainNameInformationList 查找匹配项的命名空间的所有查询中的名称。 此参数可以是以下类型之一：

- FQDN 的完全限定的域名
- 后缀的域后缀将追加到 DNS 解析的短名称查询。 若要指定一个后缀，前面添加句点 （.） 到 DNS 后缀。

**$DNSServers**。 列表以逗号分隔的 DNS 服务器 IP 地址用于命名空间。

**$TrustedNetwork**。 若要标识的受信任的网络的逗号分隔的字符串。 VPN 用户时受保护的资源是设备直接访问其公司无线网络上未自动连接。

以下是使用以下命令中的参数的示例值。 请确保您的环境更改这些值。

```xml
$TemplateName = 'Template'
$ProfileName = 'Contoso AlwaysOn VPN'
$Servers = 'vpn.contoso.com'
$DnsSuffix = 'corp.contoso.com'
$DomainName = '.corp.contoso.com'
$DNSServers = '10.10.0.2,10.10.0.3'
$TrustedNetwork = 'corp.contoso.com'
```

### <a name="prepare-and-create-the-profile-xml"></a>准备并创建配置文件 XML

下面的示例命令从模板配置文件中获取 EAP 设置：

```xml
$Connection = Get-VpnConnection -Name $TemplateName
if(!$Connection)
{
$Message = "Unable to get $TemplateName connection profile: $_"
Write-Host "$Message"
exit
}
$EAPSettings= $Connection.EapConfigXmlStream.InnerXml
```

### <a name="create-the-profile-xml"></a>创建配置文件 XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

```xml
$ProfileXML =
'<VPNProfile>
  <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
  <NativeProfile>
<Servers>' + $Servers + '</Servers>
<NativeProtocolType>IKEv2</NativeProtocolType>
<Authentication>
  <UserMethod>Eap</UserMethod>
  <Eap>
    <Configuration>
  '+ $EAPSettings + '
    </Configuration>
  </Eap>
</Authentication>
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
  </NativeProfile>
  <AlwaysOn>true</AlwaysOn>
  <RememberCredentials>true</RememberCredentials>
  <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
  <DomainNameInformation>
<DomainName>' + $DomainName + '</DomainName>
<DnsServers>' + $DNSServers + '</DnsServers>
</DomainNameInformation>
</VPNProfile>'
```

### <a name="output-vpnprofilexml-for-intune"></a>Intune 的输出 VPN_Profile.xml

可以使用下面的示例命令以保存配置文件 XML 文件：

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>输出 VPN_Profile.ps1 桌面和 System Center Configuration Manager

下面的代码示例通过使用中 VPNv2 CSP ProfileXML 节点中配置 AlwaysOn IKEv2 VPN 连接。

在 Windows 10 桌面版或 System Center Configuration Manager 中，可以使用此脚本。

### <a name="define-key-vpn-profile-parameters"></a>定义密钥的 VPN 配置文件参数

```xml
$Script = '$ProfileName = ''' + $ProfileName + '''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

## <a name="define-vpn-profilexml"></a>定义 VPN ProfileXML

```xml
$ProfileXML = ''' + $ProfileXML + '''
```

### <a name="escape-special-characters-in-the-profile"></a>在配置文件中的特殊字符进行转义

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>定义 WMI CSP 桥属性

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>确定 VPN 配置文件的用户 SID:

```xml
try
{
$username = Gwmi -Class Win32_ComputerSystem | select username
$objuser = New-Object System.Security.Principal.NTAccount($username.username)
$sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
$SidValue = $sid.Value
$Message = "User SID is $SidValue."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
Write-Host "$Message"
exit
}
```

### <a name="define-wmi-session"></a>定义 WMI 会话：

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>检测并删除以前的 VPN 配置文件：

```xml
try
{
  $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
  foreach ($deleteInstance in $deleteInstances)
  {
    $InstanceId = $deleteInstance.InstanceID
    if ("$InstanceId" -eq "$ProfileNameEscaped")
    {
        $session.DeleteInstance($namespaceName, $deleteInstance, $options)
        $Message = "Removed $ProfileName profile $InstanceId"
        Write-Host "$Message"
    } else {
        $Message = "Ignoring existing VPN profile $InstanceId"
        Write-Host "$Message"
    }
  }
}
catch [Exception]
{
  $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
  Write-Host "$Message"
  exit
}
```

### <a name="create-the-vpn-profile"></a>创建 VPN 配置文件：

```xml
try
{
  $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
  $newInstance.CimInstanceProperties.Add($property)
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
  $newInstance.CimInstanceProperties.Add($property)
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
  $newInstance.CimInstanceProperties.Add($property)
  $session.CreateInstance($namespaceName, $newInstance, $options)
  $Message = "Created $ProfileName profile."


  Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}

$Message = "Script Complete"
Write-Host "$Message"'
```

### <a name="save-the-profile-xml-file"></a>保存配置文件 XML 文件

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>MakeProfile.ps1 Full Script

大多数示例使用 Set-wmiinstance Windows PowerShell cmdlet 将 ProfileXML 插入 MDM_VPNv2_01 WMI 类的新实例。 

但是，这不会工作，System Center Configuration Manager 中因为不能在最终用户的上下文中运行包。 因此，此脚本使用通用信息模型中用户的上下文中，创建 WMI 会话，然后在该会话中创建 MDM_VPNv2_01 WMI 类的新实例。 此 WMI 类使用 WMI CSP 桥配置 VPNv2 CSP。 因此，通过添加类实例，配置 CSP。 

>[!IMPORTANT]
>WMI CSP 桥设计需要本地管理权限。 每个用户的 VPN 配置文件部署，则应当使用 SCCM 或 mdm。

>[!NOTE]
>脚本 VPN_Profile.ps1 使用当前用户的 SID 来标识用户的上下文。 由于没有 SID 都是在远程桌面会话中可用，该脚本无法在远程桌面会话中工作。 同样，它不适 HYPER-V 增强的会话中。 如果您要在虚拟机中测试远程访问 Always On VPN，禁用增强的会话在客户端 Vm 上运行此脚本之前。

以下示例脚本中包括所有前面的部分中的代码示例。 请确保将示例值更改为适合您的环境的值。
    
   ```XML
    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'

    
    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml
    
    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'
    
    $ProfileXML = ''' + $ProfileXML + '''
    
    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"
    
    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    
    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
    
        try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."

        Write-Host "$Message"
    }
    catch [Exception]
    {
        $Message = "Unable to create $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>使用 Windows PowerShell 配置 VPN 客户端

若要配置 Windows 10 客户端计算机上的 VPNv2 CSP，请运行中创建的 VPN_Profile.ps1 Windows PowerShell 脚本[创建配置文件 XML](#create-the-profile-xml)部分。 以管理员身份; 打开 Windows PowerShell否则，你将收到的错误消息，_访问被拒绝_。

运行后 VPN_Profile.ps1 配置 VPN 配置文件，你可以验证在任何时候，它已成功通过 Windows PowerShell ISE 中运行以下命令：

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**成功的结果从 Get-wmiobject cmdlet**

```powershell
__GENUS : 2
__CLASS : MDM_VPNv2_01
__SUPERCLASS:
__DYNASTY   : MDM_VPNv2_01
__RELPATH   : MDM_VPNv2_01.InstanceID="Contoso%20AlwaysOn%20VPN",ParentID
  ="./Vendor/MSFT/VPNv2"
__PROPERTY_COUNT: 10
__DERIVATION: {}
__SERVER: WIN01
__NAMESPACE : root\cimv2\mdm\dmmap
__PATH  : \\WIN01\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="Conto
  so%20AlwaysOn%20VPN",ParentID="./Vendor/MSFT/VPNv2"
AlwaysOn: True
ByPassForLocal  :
DnsSuffix   : corp.contoso.com
EdpModeId   :
InstanceID  : Contoso%20AlwaysOn%20VPN
LockDown:
ParentID: ./Vendor/MSFT/VPNv2
ProfileXML  : <VPNProfile><RememberCredentials>true</RememberCredentials>
  <AlwaysOn>true</AlwaysOn><DnsSuffix>corp.contoso.com</DnsSu
  ffix><TrustedNetworkDetection>corp.contoso.com</TrustedNetw
  orkDetection><NativeProfile><Servers>vpn.contoso.com;vpn.co
  ntoso.com</Servers><RoutingPolicyType>SplitTunnel</RoutingP
  olicyType><NativeProtocolType>Ikev2</NativeProtocolType><Au
  thentication><UserMethod>Eap</UserMethod><MachineMethod>Eap
  </MachineMethod><Eap><Configuration><EapHostConfig xmlns="h
  ttp://www.microsoft.com/provisioning/EapHostConfig"><EapMet
  hod><Type xmlns="https://www.microsoft.com/provisioning/EapC
  ommon">25</Type><VendorId xmlns="https://www.microsoft.com/p
  rovisioning/EapCommon">0</VendorId><VendorType xmlns="http:
  //www.microsoft.com/provisioning/EapCommon">0</VendorType><
  AuthorId xmlns="https://www.microsoft.com/provisioning/EapCo
  mmon">0</AuthorId></EapMethod><Config xmlns="https://www.mic
  rosoft.com/provisioning/EapHostConfig"><Eap xmlns="https://w
  ww.microsoft.com/provisioning/BaseEapConnectionPropertiesV1
  "><Type>25</Type><EapType xmlns="https://www.microsoft.com/p
  rovisioning/MsPeapConnectionPropertiesV1"><ServerValidation
  ><DisableUserPromptForServerValidation>true</DisableUserPro
  mptForServerValidation><ServerNames>NPS</ServerNames><Trust
  edRootCA>3f 07 88 e8 ac 00 32 e4 06 3f 30 f8 db 74 25 e1
  2e 5b 84 d1 </TrustedRootCA></ServerValidation><FastReconne
  ct>true</FastReconnect><InnerEapOptional>false</InnerEapOpt
  ional><Eap xmlns="https://www.microsoft.com/provisioning/Bas
  eEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="
  https://www.microsoft.com/provisioning/EapTlsConnectionPrope
  rtiesV1"><CredentialsSource><CertificateStore><SimpleCertSe
  lection>true</SimpleCertSelection></CertificateStore></Cred
  entialsSource><ServerValidation><DisableUserPromptForServer
  Validation>true</DisableUserPromptForServerValidation><Serv
  erNames>NPS</ServerNames><TrustedRootCA>3f 07 88 e8 ac 00
  32 e4 06 3f 30 f8 db 74 25 e1 2e 5b 84 d1 </TrustedRootCA><
  /ServerValidation><DifferentUsername>false</DifferentUserna
  me><PerformServerValidation xmlns="https://www.microsoft.com
  /provisioning/EapTlsConnectionPropertiesV2">true</PerformSe
  rverValidation><AcceptServerName xmlns="https://www.microsof
  t.com/provisioning/EapTlsConnectionPropertiesV2">true</Acce
  ptServerName></EapType></Eap><EnableQuarantineChecks>false<
  /EnableQuarantineChecks><RequireCryptoBinding>false</Requir
  eCryptoBinding><PeapExtensions><PerformServerValidation xml
  ns="https://www.microsoft.com/provisioning/MsPeapConnectionP
  ropertiesV2">true</PerformServerValidation><AcceptServerNam
  e xmlns="https://www.microsoft.com/provisioning/MsPeapConnec
  tionPropertiesV2">true</AcceptServerName></PeapExtensions><
  /EapType></Eap></Config></EapHostConfig></Configuration></E
  ap></Authentication></NativeProfile><DomainNameInformation>
  <DomainName>corp.contoso.com</DomainName><DnsServers>10.10.
      0.2,10.10.0.3</DnsServers><AutoTrigger>true</AutoTrigger></
  DomainNameInformation></VPNProfile>
RememberCredentials : True
TrustedNetworkDetection : corp.contoso.com
PSComputerName  : WIN01
```

ProfileXML 配置必须在结构、 拼写、 配置和有时字母大小写正确。 如果你看到内容为列表 1 中不同结构中，ProfileXML 标记可能包含错误。

如果需要进行故障排除标记，则更轻松地将其放在 XML 编辑器比以解决在 Windows PowerShell ISE 中。 在任一情况下，配置文件中，最简单版本开始并添加的组件返回直至问题一次一个地再次发生。

## <a name="configure-the-vpn-client-by-using-system-center-configuration-manager"></a>使用 System Center Configuration Manager 中配置 VPN 客户端

在 System Center Configuration Manager 中，可以通过使用 ProfileXML CSP 节点中，就像在 Windows PowerShell 中一样来部署 VPN 配置文件。 在这里，使用的部分中创建的 VPN_Profile.ps1 Windows PowerShell 脚本[创建 ProfileXML 配置文件](#create-the-profilexml-configuration-files)。

若要使用 System Center Configuration Manager 将远程访问 Always On VPN 配置文件部署到 Windows 10 客户端计算机，您必须首先创建一组计算机或用户向其部署配置文件。 在此方案中，创建一个要部署的配置脚本的用户组。

### <a name="create-a-user-group"></a>创建用户组

1.  在 Configuration Manager 控制台中，打开资产和符合性\\用户集合。

2.  上**主页**功能区中，在**创建**组中，单击**创建用户集合**。

3.  在常规页上完成以下步骤：

    a. 在中**名称**，类型**VPN 用户**。

    b. 单击**浏览**，单击**的所有用户**然后单击**确定**。

    c. 单击“下一步”  。

4.  在成员身份规则页上，完成以下步骤：

    a.  在中**成员身份规则**，单击**添加规则**，然后单击**直接规则**。 在此示例中，您将单个用户添加到用户集合。 但是，可能会使用查询规则将用户添加到此集合中的更大规模部署动态。

    b.  在“欢迎”  页上，单击“下一步”  。

    c.  在搜索资源页上，在**值**，键入你想要添加的用户的名称。 资源名称中包含用户的域。 若要包含基于部分匹配的结果，请插入 **%** 字符在您的搜索条件的任一端。 例如，若要查找包含字符串"lori"的所有用户，请键入 **%lori%** 。 单击“下一步”  。

    d.  在选择资源页上，选择的用户想要添加到组，然后单击**下一步**。

    e.  在摘要页上单击**下一步**。

    f.  在完成页上单击**关闭**。

6.  返回在创建用户集合向导的成员身份规则页上，单击**下一步**。

7.  在摘要页上单击**下一步**。

8.  在完成页上单击**关闭**。

创建用户组能够接收 VPN 配置文件后，可以创建包和程序部署的部分中创建的 Windows PowerShell 配置脚本[创建 ProfileXML 配置文件](#create-the-profilexml-configuration-files)。

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>创建包含 ProfileXML 配置脚本的包

1.  托管站点服务器计算机帐户可以访问的网络共享上的脚本 VPN_Profile.ps1。

2.  在 Configuration Manager 控制台中，打开**软件库\\应用程序管理\\包**。

3.  上**主页**功能区中，在**创建**组中，单击**创建包**开始创建包和程序向导。

4.  在包页面上，完成以下步骤：

    a. 在中**名称**，类型**Windows 10 Alwayson VPN 配置文件**。

    b. 选择**此包包含源文件**复选框，然后单击**浏览**。

    c. 在设置源文件夹对话框中，单击**浏览**，选择包含 VPN_Profile.ps1，文件共享，然后单击**确定**。
        请确保你选择的网络路径，而不是本地路径。 换而言之，路径应类似于 *\\fileserver\\vpnscript*，而不*c:\\vpnscript*。

1.  单击“下一步”  。

2.  在程序类型页上单击**下一步**。

3.  在标准程序页上，完成以下步骤：

    a.  在中**名称**，类型**VPN 配置文件脚本**。

    b.  在中**命令行**，类型**PowerShell.exe-ExecutionPolicy 绕过的文件"VPN_Profile.ps1"** 。

    c.  在中**运行模式**，单击**使用管理权限运行**。

    d.  单击“下一步”  。

4.  在要求页上，完成以下步骤：

    a.  选择**仅在指定的平台上可以运行此程序**。

    b.  选择**所有 Windows 10 （32 位）** 并**所有 Windows 10 （64 位）** 复选框。

    c.  在中**估计磁盘空间**，类型**1**。

    d.  在中**最大允许运行的时间 （分钟）** ，类型**15**。

    e.  单击“下一步”  。

5.  在摘要页上单击**下一步**。

6.  在完成页上单击**关闭**。

与包和创建程序，您需要将其部署到**VPN 用户**组。

### <a name="deploy-the-profilexml-configuration-script"></a>部署 ProfileXML 配置脚本

1.  在 Configuration Manager 控制台中，打开软件库\\应用程序管理\\包。

2.  在中**包**，单击**Windows 10 Alwayson VPN 配置文件**。

3.  上**程序**选项卡上，底部的详细信息窗格中，右键单击**VPN 配置文件脚本**，单击**属性**，并完成以下步骤：

    a.  上**高级**选项卡上，在**当此程序分配到计算机**，单击**一次的每个用户登录**。

    b.  单击 **“确定”** 。

4.  右键单击**VPN 配置文件脚本**然后单击**部署**以启动部署软件向导。

5.  在常规页上完成以下步骤：

    a.  旁边**集合**，单击**浏览**。

    b.  在中**集合类型**列表中 （左上角），单击**用户集合**。

    c.  单击**VPN 用户**，然后单击**确定**。

    d.  单击“下一步”  。

6.  在内容页上完成以下步骤：

    a.  单击**外**，然后单击**分发点**。

    b.  在中**可用的分发点**，选择你想要分发 ProfileXML 配置脚本，并单击的分发点**确定**。

    c.  单击“下一步”  。

7.  在部署设置页上单击**下一步**。

8.  在计划页上，完成以下步骤：

    a.  单击**新建**打开分配计划对话框。

    b.  单击**此事件之后立即分配**，然后单击**确定**。

    c.  单击“下一步”  。

9.  在用户体验页上，完成以下步骤：

    1.  选择**软件安装**复选框。

    2.  单击**摘要**。

10. 在摘要页上单击**下一步**。

11. 在完成页上单击**关闭**。

部署 ProfileXML 配置脚本后，请登录到 Windows 10 客户端计算机与所选生成的用户集合时的用户帐户。 验证 VPN 客户端的配置。

>[!NOTE]
>脚本 VPN_Profile.ps1 远程桌面会话中不起作用。 同样，它不适 HYPER-V 增强的会话中。 如果您要在虚拟机中测试远程访问 Always On VPN，禁用增强的会话在客户端 Vm 上，然后再继续。

### <a name="verify-the-configuration-of-the-vpn-client"></a>验证 VPN 客户端的配置

1.  在控制面板中下,**系统\\安全**，单击**Configuration Manager**。 

2.  在 Configuration Manager 属性对话框中，在**操作**选项卡上，完成以下步骤：

    a.  单击**计算机策略检索和评估周期**，单击**立即运行**，然后单击**确定**。

    b.  单击**用户策略检索和评估周期**，单击**立即运行**，然后单击**确定**。

    c.  单击 **“确定”** 。

3.  关闭控制面板。

稍后，您应看到新的 VPN 配置文件。

## <a name="configure-the-vpn-client-by-using-intune"></a>使用 Intune 配置 VPN 客户端

若要使用 Intune 来部署 Windows 10 的远程访问始终在 VPN 配置文件，可以配置 ProfileXML CSP 节点使用的部分中创建的 VPN 配置文件[创建 ProfileXML 配置文件](#create-the-profilexml-configuration-files)，也可以使用基 EAP下面提供的 XML 示例。

>[!NOTE]
>Intune 现在使用 Azure AD 组。 如果 Azure AD Connect 同步到 Azure AD 中从本地的 VPN 用户组和用户分配到 VPN 用户组，您就可以开始学习。

创建 VPN 设备配置策略添加到组的所有用户的 Windows 10 客户端计算机配置。 由于 Intune 模板提供 VPN 参数，只需复制\<EapHostConfig > \</EapHostConfig > VPN_ProfileXML 文件的一部分。

### <a name="create-the-always-on-vpn-configuration-policy"></a>创建 Always On VPN 配置策略

1.  登录到 [Azure 门户](https://portal.azure.com/)。

2.  转到**Intune** > **设备配置** > **配置文件**。

3.  单击**创建配置文件**启动创建配置文件向导。

4.  输入**名称**VPN 配置文件和 （可选） 描述。

1.   下**平台**，选择**Windows 10 或更高版本**，然后选择**VPN**从配置文件类型下拉列表。

     >[!TIP]
     >如果要创建自定义 VPN profileXML，请参阅[使用 Intune 应用 ProfileXML](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune)有关的说明。

2. 下**基础 VPN**选项卡上，验证或将以下设置：

    - **连接名称：** 输入 VPN 连接的名称，因为其出现在客户端计算机上**VPN**选项卡上的**设置**，例如， _Contoso AutoVPN_。  
    
    - **服务器：** 通过单击添加一个或多个 VPN 服务器**添加。**
    
    - **描述**和**IP 地址或 FQDN:** 输入说明和 IP 地址或 VPN 服务器的 FQDN。 这些值必须与 VPN 服务器的身份验证证书中的使用者名称保持一致。 
    
    - **默认服务器：** 如果这是默认的 VPN 服务器，设置为 **，则返回 True**。 执行此操作启用此服务器作为设备用于建立连接的默认服务器。 
    
    - **连接类型：** 设置为**IKEv2**。  
    
    - **Alwayson:** 设置为**启用**到在中使用的登录自动 VPN 连接并保持连接，直到用户手动断开连接。
    
    - **在每次登录时记住凭据**:布尔值 （true 或 false） 的缓存凭据。 如果应尽可能缓存设置为 true，凭据。

3.  将以下 XML 字符串复制到文本编辑器：
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  替换 **\<TrustedRootCA > 5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 是 4b <\/TrustedRootCA >** 在示例中的本地根证书颁发机构的证书指纹在这两个位置。
  
    >[!Important]
    >不使用中的示例指纹\<TrustedRootCA >\</TrustedRootCA > 下面的部分。  TrustedRootCA 必须颁发 RRAS 和 NPS 服务器的服务器身份验证证书的本地根证书颁发机构的证书指纹。 **这不能将云根证书，也不中间颁发 CA 证书指纹**。

5.  替换 **\<ServerNames > NPS.contoso.com\</ServerNames >** 中的身份验证发生在已加入域的 NPS fqdn 的 XML 示例。 

6.  复制修改后的 XML 字符串并粘贴到**EAP Xml**在基础 VPN 选项卡下的框，然后单击**确定**。
    在 Intune 中创建使用 EAP Always On VPN 设备配置策略。


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Always On VPN 配置策略与 Intune 同步

若要测试配置策略，以登录到 Windows 10 客户端计算机用户添加到您**始终对 VPN 用户**组，然后使用 Intune 同步。

1.  在开始菜单上单击**设置**。

2.  在设置中，单击**帐户**，然后单击**访问工作或学校**。

3.  单击 MDM 配置文件，然后单击**信息**。

4.  单击**同步**强制 Intune 策略评估和检索。

5.  关闭设置。 同步后，查看计算机上可用的 VPN 配置文件。

## <a name="next-steps"></a>后续步骤

完成部署 Always On VPN。  你可以配置其他功能，请参阅下表：

|如果想要...  |然后请参阅...  |
|---------|---------|
|为 VPN 配置条件访问    |[步骤 7.（可选）配置针对 VPN 连接使用 Azure AD 条件性访问](../../ad-ca-vpn-connectivity-windows10.md):在此步骤中，您可以微调如何授权的 VPN 用户访问你使用的资源[Azure Active Directory (Azure AD) 条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 使用针对虚拟专用网络 (VPN) 连接的 Azure AD 条件性访问，可帮助保护 VPN 连接。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。         |
|了解有关高级 VPN 功能的详细信息  |[高级 VPN 功能](always-on-vpn-adv-options.md#advanced-vpn-features):此页提供有关如何启用 VPN 流量筛选器、 如何配置自动 VPN 连接使用应用的触发器，以及如何将 NPS 配置为仅允许使用由 Azure AD 颁发的证书的客户端的 VPN 连接的指南。        |
