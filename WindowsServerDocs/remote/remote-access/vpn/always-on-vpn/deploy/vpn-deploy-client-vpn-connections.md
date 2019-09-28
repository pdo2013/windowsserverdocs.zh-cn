---
title: 配置 Windows 10 客户端始终启用 VPN 连接
description: 在此步骤中, 你将了解 ProfileXML 选项和架构, 并将 Windows 10 客户端计算机配置为使用 VPN 连接与该基础结构进行通信。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: 9621f9bdca0416965861112ba23c1c8dd731f67b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404283"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>步骤 6： 配置 Windows 10 客户端 Always On VPN 连接

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**步骤 5：配置 DNS 和防火墙设置](vpn-deploy-dns-firewall.md)<br>
- [**一个**步骤 7：可有可无使用 Azure AD 的 VPN 连接的条件性访问](../../ad-ca-vpn-connectivity-windows10.md)

在此步骤中, 你将了解 ProfileXML 选项和架构, 并将 Windows 10 客户端计算机配置为使用 VPN 连接与该基础结构进行通信。

可以通过 PowerShell、SCCM 或 Intune 配置 Always On VPN 客户端。 所有三个都需要一个 XML VPN 配置文件来配置相应的 VPN 设置。 无需 SCCM 或 Intune 的组织可以自动执行 PowerShell 注册。

>[!NOTE]
>组策略不包括用于配置 Windows 10 远程访问 Always On VPN 客户端的管理模板。  但是, 您可以使用登录脚本。

## <a name="profilexml-overview"></a>ProfileXML 概述

ProfileXML 是 VPNv2 CSP 中的 URI 节点。 不是单独配置每个 VPNv2 CSP 节点 (如触发器、路由列表和身份验证协议), 而是使用此节点通过将所有设置作为单个 XML 块传递到单个 CSP 节点来配置 Windows 10 VPN 客户端。 ProfileXML 架构与 VPNv2 CSP 节点的架构几乎相同, 但有些术语略有不同。

此部署描述的所有传递方法 (包括 Windows PowerShell、System Center Configuration Manager 和 Intune) 都使用 ProfileXML。 可通过两种方法配置此部署中的 ProfileXML VPNv2 CSP 节点:

- **OMA-URI**。 其中一种方法是使用使用 OMA DM 的 MDM 提供程序, 如前面的 " [VPNV2 CSP 节点](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes)" 一节中所述。 使用此方法时, 可以在使用 Intune 时轻松地将 VPN 配置文件配置 XML 标记插入 ProfileXML CSP 节点。

- **Windows Management Instrumentation (WMI) 到 CSP 桥**。 配置 ProfileXML CSP 节点的第二种方法是使用 "WMI 到 CSP" 桥，这是一个名为 " **MDM_VPNv2_01**" 的 wmi 类，可访问 VPNv2 CSP 和 ProfileXML 节点。 当你创建该 WMI 类的新实例时, WMI 将在使用 Windows PowerShell 和 System Center Configuration Manager 时使用该 CSP 创建 VPN 配置文件。

即使这些配置方法不同, 都需要格式正确的 XML VPN 配置文件。 若要使用 ProfileXML VPNv2 CSP 设置, 请使用 ProfileXML 架构来构建 XML, 以配置简单部署方案所需的标记。 有关详细信息, 请参阅[PROFILEXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd)。

在下面找到每个所需的设置及其相应的 ProfileXML 标记。 在 ProfileXML 架构中的特定标记内配置每个设置, 而不是所有这些设置都在本地配置文件下找到。 有关更多标记放置, 请参阅 ProfileXML 架构。

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**连接类型:** 本机 IKEv2

ProfileXML 元素:

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**传递**拆分隧道

ProfileXML 元素:

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**名称解析:** 域名信息列表和 DNS 后缀

ProfileXML 元素:

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**触发器**Always On 和受信任的网络检测

ProfileXML 元素:

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**验证**具有受 TPM 保护的用户证书的 PEAP-GTC

ProfileXML 元素:

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

你可以使用简单标记来配置某些 VPN 身份验证机制。 不过, EAP 和 PEAP 更多。 若要创建 XML 标记, 最简单的方法是使用其 EAP 设置配置 VPN 客户端, 然后将该配置导出到 XML。

有关 EAP 设置的详细信息, 请参阅[eap 配置](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration)。

## <a name="manually-create-a-template-connection-profile"></a>手动创建模板连接配置文件

在此步骤中, 你将使用受保护的可扩展身份验证协议 (PEAP) 来保护客户端和服务器之间的通信。 与简单的用户名和密码不同, 此连接需要使用 VPN 配置文件中的唯一 New-eapconfiguration 部分。

你可以使用 Windows 中的 "设置" 来创建模板 VPN 配置文件, 而不是描述如何从头开始创建 XML 标记。 创建模板 VPN 配置文件后, 使用 Windows PowerShell 从该模板使用 New-eapconfiguration 部分, 以创建以后在部署中部署的最终 ProfileXML。

### <a name="record-nps-certificate-settings"></a>记录 NPS 证书设置

创建模板之前，请从服务器的证书中记录 NPS 服务器的主机名或完全限定的域名（FQDN），以及颁发证书的 CA 的名称。

**方法**

1. 在 NPS 服务器上, 打开 "网络策略服务器"。

2. 在 NPS 控制台中的 "策略" 下, 单击 "**网络策略**"。

3. 右键单击 "**虚拟专用网络 (VPN) 连接**", 然后单击 "**属性**"。

4. 单击 "**约束**" 选项卡, 然后单击 "**身份验证方法**"。

5. 在 EAP 类型中, **单击 "Microsoft:受保护的 EAP (** PEAP), 然后单击 "**编辑**"。

6. 记录**颁发给**和**颁发者**的证书的值。

    在即将到来的 VPN 模板配置中使用这些值。 例如，如果服务器的 FQDN 为 nps01.corp.contoso.com，主机名为 NPS01.XML，则证书名称基于服务器的 FQDN 或 DNS 名称，例如 nps01.corp.contoso.com。

7. 取消 "编辑受保护的 EAP 属性" 对话框。

8. 取消 "虚拟专用网络 (VPN) 连接属性" 对话框。

9. 关闭网络策略服务器。

>[!NOTE]
>如果有多个 NPS 服务器, 请在每个服务器上完成这些步骤, 以使 VPN 配置文件可以在其使用时进行验证。

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>在已加入域的客户端计算机上配置模板 VPN 配置文件

现在, 你已有必要的信息, 请在加入域的客户端计算机上配置模板 VPN 配置文件。 在此过程中使用的用户帐户类型 (即标准用户或管理员) 并不重要。

但是，如果在配置证书自动注册之前尚未重新启动计算机，请在配置模板 VPN 连接之前执行此操作，以确保在其上注册可用的证书。

>[!NOTE]
>无法手动添加 VPN 的任何高级属性, 如 NRPT 规则、Always On、受信任的网络检测等。在下一步中, 你将创建一个测试 VPN 连接以验证 VPN 服务器的配置, 并且可以与服务器建立 VPN 连接。

**手动创建单个测试 VPN 连接**

1.  以**VPN 用户**组成员的身份登录到已加入域的客户端计算机。

2.  在 "开始" 菜单上键入 " **VPN**", 然后按 enter。

3.  在详细信息窗格中, 单击 "**添加 VPN 连接**"。

4.  在 "VPN 提供程序" 列表中, 单击 " **Windows (内置)** "。

5.  在 "连接名称" 中, 键入**Template**。

6.  在 "服务器名称或地址" 中, 键入 VPN 服务器的**外部**FQDN (例如, **vpn.contoso.com**)。

7.  单击“保存”。

8.  在 "相关设置" 下, 单击 "**更改适配器选项**"。

9.  右键单击 "**模板**", 然后单击 "**属性**"。

10. 在 "**安全**" 选项卡上的 " **VPN 类型**" 中, 单击 " **IKEv2**"。

11. 在 "数据加密" 中, 单击 "**最大强度加密**"。

12. 单击 "**使用可扩展的身份验证协议 (EAP)** ";然后, 在 "**使用可扩展的身份验证协议 (EAP)** " 中, 单击 " **Microsoft:受保护的 EAP (PEAP) (已**启用加密)。

13. 单击 "**属性**" 以打开 "受保护的 EAP 属性" 对话框, 并完成以下步骤:

    a. 在 "**连接到这些服务器**" 框中, 键入从此部分前面的 nps 服务器身份验证设置中检索到的 nps 服务器的名称 (例如, nps01.xml)。

    >[!NOTE]
    >你键入的服务器名称必须与证书中的名称匹配。 此名称已在此部分前面部分恢复。 如果名称不匹配, 则连接将失败, 并指出 "由于 RAS/VPN 服务器上配置的策略, 连接被阻止"。

    b.  在 "受信任的根证书颁发机构" 下，选择颁发给 NPS 服务器的证书的根 CA （例如，contoso-CA）。

    c.  在 "连接前通知" 中，单击 "**不请求用户授权新服务器或受信任的 ca**"。

    d.  在 "选择身份验证方法" 中, 单击 "**智能卡或其他证书**", 然后单击 "**配置**"。 "智能卡或其他证书属性" 对话框将打开。

    e.  单击 "**在此计算机上使用证书"** 。

    f.  在 "连接到这些服务器" 框中, 输入你在前面步骤中从 NPS 服务器身份验证设置中检索到的 NPS 服务器的名称。

    g.  在 "受信任的根证书颁发机构" 下，选择颁发 NPS 服务器证书的根 CA。

    h.  选择 "**不提示用户授权新服务器或受信任的证书颁发机构**" 复选框。

    i.  单击 **"确定"** 以关闭 "智能卡或其他证书属性" 对话框。

    j.  单击 **"确定"** 以关闭 "受保护的 EAP 属性" 对话框。

14. 单击 **"确定"** 以关闭 "模板属性" 对话框。

15. 关闭 "网络连接" 窗口。

16. 在 "设置" 中, 通过单击 "**模板**", 然后单击 "**连接**" 来测试 VPN。

>[!IMPORTANT]
>请确保已成功连接到 VPN 服务器的模板 VPN 连接。 这样做可确保 EAP 设置正确, 然后在下一个示例中使用它们。 必须至少连接一次, 然后才能继续;否则, 配置文件将不包含连接到 VPN 所需的所有信息。

## <a name="create-the-profilexml-configuration-files"></a>创建 ProfileXML 配置文件

在完成本部分之前, 请确保已创建并测试了模板[连接配置文件](#manually-create-a-template-connection-profile)部分所述的模板 VPN 连接。 若要确保配置文件包含连接到 VPN 所需的所有信息, 需要测试 VPN 连接。

列表1中的 Windows PowerShell 脚本在桌面上创建两个文件, 这两个文件都包含基于你之前创建的模板连接配置文件的**new-eapconfiguration**标记:

- **VPN_Profile。** 此文件包含在 VPNv2 CSP 中配置 ProfileXML 节点所需的 XML 标记。 将此文件用于与 OMA 兼容的 MDM 服务 (如 Intune) 配合使用。

- **VPN_Profile。** 此文件是一个 Windows PowerShell 脚本, 你可以在客户端计算机上运行该脚本, 以配置 VPNv2 CSP 中的 ProfileXML 节点。 还可以通过 System Center Configuration Manager 部署此脚本来配置 CSP。 你无法在远程桌面会话 (包括 Hyper-v 增强会话) 中运行此脚本。

>[!IMPORTANT]
>下面的示例命令需要 Windows 10 版本1607或更高版本。

**创建 VPN_Profile 和 VPN_Proflie**

1. 登录到已加入域的客户端计算机, 该客户端计算机包含模板 VPN 配置文件, 该用户帐户与部分[手动创建模板连接配置文件](#manually-create-a-template-connection-profile)的用户帐户相同。

2. 将列表1粘贴到 Windows PowerShell 集成脚本环境 (ISE) 中, 并自定义注释中所述的参数。 这些 $Template、$ProfileName、$Servers、$DnsSuffix、$DomainName、$TrustedNetwork 和 $DNSServers。 注释中介绍了每个设置的完整说明。

3. 运行脚本以在桌面上生成**VPN_Profile**和**VPN_Profile** 。

#### <a name="listing-1-understanding-makeprofileps1"></a>列表1。 了解 MakeProfile

本部分介绍了可用于了解如何创建 VPN 配置文件的示例代码, 特别是在 VPNv2 CSP 中配置 ProfileXML 时。

通过此示例代码组合脚本并运行脚本后, 该脚本将生成两个文件:VPN_Profile 和 VPN_Profile。 使用 VPN_Profile 在符合 OMA 标准的 MDM 服务中配置 ProfileXML，如 Microsoft Intune。

使用 Windows PowerShell 或 System Center Configuration Manager 中的**VPN_Profile**脚本在 windows 10 桌面上配置 ProfileXML。

>[!NOTE]
>若要查看完整的示例脚本, 请参阅[MakeProfile Full script](#makeprofileps1-full-script)部分。

#### <a name="parameters"></a>Parameters

配置以下参数:

**$Template**。 要从中检索 EAP 配置的模板的名称。

**$ProfileName**。 配置文件的唯一字母数字标识符。 配置文件名称不能包含正斜杠 (/)。 如果配置文件名称包含空格或其他非字母数字字符, 则必须根据 URL 编码标准正确地对其进行转义。

**$Servers**。 VPN 网关的公用或可路由 IP 地址或 DNS 名称。 它可以指向网关的外部 IP 或服务器场的虚拟 IP。 例如, 208.147.66.130 或 vpn.contoso.com。

**$DnsSuffix**。 指定逗号分隔的一个或多个 DNS 后缀。 该列表中的第一个也用作 VPN 接口的主要连接特定的 DNS 后缀。 整个列表也将添加到 SuffixSearchList 中。

**$DomainName**。 用于指示策略应用到的命名空间。 发出名称查询时, DNS 客户端会将查询中的名称与 DomainNameInformationList 下的所有命名空间进行比较, 以找到匹配项。 此参数可以是以下类型之一:

- FQDN-完全限定的域名
- 后缀-将追加到短名称查询进行 DNS 解析的域后缀。 若要指定一个后缀, 请在 DNS 后缀前面加上一个句点 (.)。

**$DNSServers**。 要用于命名空间的以逗号分隔的 DNS 服务器 IP 地址的列表。

**$TrustedNetwork**。 用逗号分隔的字符串标识可信网络。 如果用户位于其公司无线网络上, 并且设备可直接访问受保护的资源, 则 VPN 不会自动连接。

下面是在下面的命令中使用的参数的示例值。 确保为环境更改这些值。

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

以下示例命令获取模板配置文件中的 EAP 设置:

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
$ProfileXML = @("
<VPNProfile>
  <DnsSuffix>$DnsSuffix</DnsSuffix>
  <NativeProfile>
<Servers>$Servers</Servers>
<NativeProtocolType>IKEv2</NativeProtocolType>
<Authentication>
  <UserMethod>Eap</UserMethod>
  <Eap>
    <Configuration>
     $EAPSettings
    </Configuration>
  </Eap>
</Authentication>
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
  </NativeProfile>
  <AlwaysOn>true</AlwaysOn>
  <RememberCredentials>true</RememberCredentials>
  <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
  <DomainNameInformation>
<DomainName>$DomainName</DomainName>
<DnsServers>$DNSServers</DnsServers>
</DomainNameInformation>
</VPNProfile>
")
```

### <a name="output-vpn_profilexml-for-intune"></a>输出 VPN_Profile for Intune

您可以使用以下示例命令来保存配置文件 XML 文件:

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpn_profileps1-for-the-desktop-and-system-center-configuration-manager"></a>输出 VPN_Profile 用于桌面和 System Center Configuration Manager

下面的示例代码通过使用 VPNv2 CSP 中的 ProfileXML 节点配置 AlwaysOn IKEv2 VPN 连接。

可以在 Windows 10 桌面或 System Center Configuration Manager 中使用此脚本。

### <a name="define-key-vpn-profile-parameters"></a>定义关键 VPN 配置文件参数

```xml
$Script = '$ProfileName = ''' + $ProfileName + ''''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

### <a name="escape-special-characters-in-the-profile"></a>对配置文件中的特殊字符进行转义

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>定义 WMI 到 CSP 桥属性

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>为 VPN 配置文件确定用户 SID:

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

### <a name="define-wmi-session"></a>定义 WMI 会话:

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>检测并删除以前的 VPN 配置文件:

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

### <a name="create-the-vpn-profile"></a>创建 VPN 配置文件:

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
Write-Host "$Message"
```

### <a name="save-the-profile-xml-file"></a>保存配置文件 XML 文件

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>MakeProfile 完整脚本

大多数示例使用 Set-wmiinstance Windows PowerShell cmdlet 将 ProfileXML 插入到 MDM_VPNv2_01 WMI 类的新实例中。 

但是，这在 System Center Configuration Manager 不起作用，因为你无法在最终用户的上下文中运行包。 因此，此脚本使用通用信息模型在用户的上下文中创建 WMI 会话，然后在该会话中创建 MDM_VPNv2_01 WMI 类的新实例。 此 WMI 类使用 WMI 到 CSP 桥配置 VPNv2 CSP。 因此, 通过添加类实例, 可以配置 CSP。 

>[!IMPORTANT]
>WMI 到 CSP 桥要求按设计需要本地管理权限。 若要部署每用户 VPN 配置文件, 应使用 SCCM 或 MDM。

>[!NOTE]
>脚本 VPN_Profile 使用当前用户的 SID 标识用户的上下文。 由于远程桌面会话中没有可用的 SID, 该脚本在远程桌面会话中不起作用。 同样, 它也不能在 Hyper-v 增强会话中工作。 如果要测试虚拟机中 Always On VPN 的远程访问，请在运行此脚本之前禁用客户端 Vm 上的增强会话。

下面的示例脚本包含之前部分中的所有代码示例。 确保将示例值更改为适用于您的环境的值。
    
   ```makeProfile.ps1
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
    
    $ProfileXML = @("
    <VPNProfile>
      <DnsSuffix>$DnsSuffix</DnsSuffix>
      <NativeProfile>
    <Servers>$Servers</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
        $EAPSettings
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>$DomainName</DomainName>
    <DnsServers>$DNSServers</DnsServers>
    </DomainNameInformation>
    </VPNProfile>
    ")
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
     $Script = @("
      `$ProfileName = '$ProfileName'
      `$ProfileNameEscaped = `$ProfileName -replace ' ', '%20'
 
      `$ProfileXML = '$ProfileXML'
 
      `$ProfileXML = `$ProfileXML -replace '<', '&lt;'
      `$ProfileXML = `$ProfileXML -replace '>', '&gt;'
      `$ProfileXML = `$ProfileXML -replace '`"', '&quot;'
 
      `$nodeCSPURI = `"./Vendor/MSFT/VPNv2`"
      `$namespaceName = `"root\cimv2\mdm\dmmap`"
      `$className = `"MDM_VPNv2_01`"
 
      try
      {
      `$username = Gwmi -Class Win32_ComputerSystem | select username
      `$objuser = New-Object System.Security.Principal.NTAccount(`$username.username)
      `$sid = `$objuser.Translate([System.Security.Principal.SecurityIdentifier])
      `$SidValue = `$sid.Value
      `$Message = `"User SID is `$SidValue.`"
      Write-Host `"`$Message`"
      }
      catch [Exception]
      {
      `$Message = `"Unable to get user SID. User may be logged on over Remote Desktop: `$_`"
      Write-Host `"`$Message`"
      exit
      }
 
      `$session = New-CimSession
      `$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Type`", `"PolicyPlatform_UserContext`", `$false)
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Id`", `"`$SidValue`", `$false)
 
      try
      {
    `$deleteInstances = `$session.EnumerateInstances(`$namespaceName, `$className, `$options)
    foreach (`$deleteInstance in `$deleteInstances)
    {
        `$InstanceId = `$deleteInstance.InstanceID
        if (`"`$InstanceId`" -eq `"`$ProfileNameEscaped`")
        {
            `$session.DeleteInstance(`$namespaceName, `$deleteInstance, `$options)
            `$Message = `"Removed `$ProfileName profile `$InstanceId`"
            Write-Host `"`$Message`"
        } else {
            `$Message = `"Ignoring existing VPN profile `$InstanceId`"
            Write-Host `"`$Message`"
        }
    }
      }
      catch [Exception]
      {
    `$Message = `"Unable to remove existing outdated instance(s) of `$ProfileName profile: `$_`"
    Write-Host `"`$Message`"
    exit
      }
 
      try
      {
    `$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance `$className, `$namespaceName
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ParentID`", `"`$nodeCSPURI`", `"String`", `"Key`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"InstanceID`", `"`$ProfileNameEscaped`", `"String`",      `"Key`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ProfileXML`", `"`$ProfileXML`", `"String`", `"Property`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$session.CreateInstance(`$namespaceName, `$newInstance, `$options)
    `$Message = `"Created `$ProfileName profile.`"

    Write-Host `"`$Message`"
      }
      catch [Exception]
      {
    `$Message = `"Unable to create `$ProfileName profile: `$_`"
    Write-Host `"`$Message`"
    exit
      }
 
      `$Message = `"Script Complete`"
      Write-Host `"`$Message`"
      ")
 
      $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>使用 Windows PowerShell 配置 VPN 客户端

若要在 Windows 10 客户端计算机上配置 VPNv2 CSP，请运行在[创建配置文件 XML](#create-the-profile-xml)部分中创建的 VPN_Profile Windows PowerShell 脚本。 以管理员身份打开 Windows PowerShell;否则，会收到错误消息，_拒绝访问_。

运行 VPN_Profile 来配置 VPN 配置文件后，你可以在 Windows PowerShell ISE 中运行以下命令，随时验证是否已成功：

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**Get-wmiobject cmdlet 中的成功结果**

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

ProfileXML 配置在结构、拼写、配置和有时是字母大小写方面必须正确。 如果在结构中看到不同于列表1的内容, 则 ProfileXML 标记可能包含错误。

如果需要对标记进行故障排除, 将它放在 XML 编辑器中比在 Windows PowerShell ISE 中对其进行故障排除更为容易。 在任一情况下, 请从配置文件的最简单版本开始, 并一次添加一个组件, 直到再次出现问题。

## <a name="configure-the-vpn-client-by-using-system-center-configuration-manager"></a>使用 System Center Configuration Manager 配置 VPN 客户端

在 System Center Configuration Manager 中, 你可以使用 ProfileXML CSP 节点来部署 VPN 配置文件, 就像在 Windows PowerShell 中一样。 在这里，你将使用在[创建 ProfileXML 配置文件](#create-the-profilexml-configuration-files)部分中创建的 VPN_Profile Windows PowerShell 脚本。

若要使用 System Center Configuration Manager 将 VPN 配置文件 Always On 部署到 Windows 10 客户端计算机, 必须首先创建一组要将配置文件部署到的计算机或用户。 在此方案中, 请创建一个用户组来部署配置脚本。

### <a name="create-a-user-group"></a>创建用户组

1.  在 Configuration Manager 控制台中, 打开 "资产和\\符合性用户集合"。

2.  在 "**主页**" 功能区上的 "**创建**" 组中, 单击 "**创建用户集合**"。

3.  在 "常规" 页上, 完成以下步骤:

    a. 在 "**名称**" 中, 键入**VPN 用户**。

    b. 单击 "**浏览**", 单击 "**所有用户**", 然后单击 **"确定"** 。

    c. 单击“下一步”。

4.  在 "成员身份规则" 页上, 完成以下步骤:

    a.  在 "**成员身份规则**" 中, 单击 "**添加规则**", 然后单击 "**直接规则**"。 在此示例中，你将向用户集合添加单个用户。 不过, 你可以使用查询规则为较大规模的部署动态添加此集合中的用户。

    b.  在“欢迎”页上，单击“下一步”。

    c.  在 "搜索资源" 页上的 "**值**" 中, 键入要添加的用户的名称。 资源名称包括用户的域。 若要包含基于部分匹配的结果, 请在 **%** 搜索条件的任一末尾插入字符。 例如，若要查找包含字符串 "lori" 的所有用户，请键入 **% lori%** 。 单击“下一步”。

    d.  在 "选择资源" 页上, 选择要添加到组的用户, 然后单击 "**下一步**"。

    e.  在 "摘要" 页上, 单击 "**下一步**"。

    f.  在 "完成" 页上, 单击 "**关闭**"。

6.  返回到 "创建用户集合向导" 的 "成员身份规则" 页, 然后单击 "**下一步**"。

7.  在 "摘要" 页上, 单击 "**下一步**"。

8.  在 "完成" 页上, 单击 "**关闭**"。

创建用于接收 VPN 配置文件的用户组后, 你可以创建包和程序来部署在[创建 ProfileXML 配置文件](#create-the-profilexml-configuration-files)部分中创建的 Windows PowerShell 配置脚本。

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>创建包含 ProfileXML 配置脚本的包

1.  在站点服务器计算机帐户可以访问的网络共享上托管脚本 VPN_Profile。

2.  在 Configuration Manager 控制台中, 打开 "**软件\\库"\\"应用程序管理包**"。

3.  在 "**主页**" 功能区上的 "**创建**" 组中, 单击 "**创建包**" 以启动 "创建包和程序向导"。

4.  在 "包" 页上, 完成以下步骤:

    a. 在 "**名称**" 中, 键入**WINDOWS 10 Always On VPN 配置文件**。

    b. 选中 "**此包包含源文件**" 复选框, 然后单击 "**浏览**"。

    c. 在 "设置源文件夹" 对话框中，单击 "**浏览**"，选择包含 VPN_Profile 的文件共享，然后单击 **"确定"** 。
        请确保选择一个网络路径, 而不是本地路径。 换句话说, 路径应类似于 *\\\\"vpnscript*", 而不是*c:\\vpnscript*。

1.  单击“下一步”。

2.  在 "程序类型" 页上, 单击 "**下一步**"。

3.  在 "标准程序" 页上, 完成以下步骤:

    a.  在 "**名称**" 中, 键入 " **VPN 配置文件脚本**"。

    b.  在**命令行**中，键入**set-executionpolicy "VPN_Profile"** 。

    c.  在**运行模式**下, 单击 "**以管理权限运行**"。

    d.  单击“下一步”。

4.  在 "要求" 页上, 完成以下步骤:

    a.  选择 **"此程序只能在指定的平台上运行"** 。

    b.  选中 "**所有 windows 10 (32 位)** " 和 "**所有 windows 10 (64 位)** " 复选框。

    c.  在**估计的磁盘空间**中, 键入**1**。

    d.  在 "**最大允许运行时间 (分钟)** " 中, 键入**15**。

    e.  单击“下一步”。

5.  在 "摘要" 页上, 单击 "**下一步**"。

6.  在 "完成" 页上, 单击 "**关闭**"。

创建包和程序后, 需要将其部署到**VPN 用户**组。

### <a name="deploy-the-profilexml-configuration-script"></a>部署 ProfileXML 配置脚本

1.  在 Configuration Manager 控制台中, 打开 "软件\\库"\\"应用程序管理包"。

2.  在 "**包**" 中, 单击 " **WINDOWS 10 Always On VPN 配置文件**"。

3.  在 "**程序**" 选项卡上的详细信息窗格底部, 右键单击 " **VPN 配置文件脚本**", 单击 "**属性**", 然后完成以下步骤:

    a.  在 "**高级**" 选项卡上, 将**此程序分配到计算机时**, 单击 "**一次", 为登录的每个用户单击 "一次**"。

    b.  单击 **“确定”** 。

4.  右键单击 " **VPN 配置文件脚本**", 然后单击 "**部署**" 以启动 "部署软件向导"。

5.  在 "常规" 页上, 完成以下步骤:

    a.  在 "**集合**" 旁边, 单击 "**浏览**"。

    b.  在 "**集合类型**" 列表 (左上方) 中, 单击 "**用户集合**"。

    c.  单击 " **VPN 用户**", 然后单击 **"确定"** 。

    d.  单击“下一步”。

6.  在 "内容" 页上, 完成以下步骤:

    a.  单击 "**添加**", 然后单击 "**分发点**"。

    b.  在 "**可用分发点**" 中, 选择要将 ProfileXML 配置脚本分发到的分发点, 然后单击 **"确定"** 。

    c.  单击“下一步”。

7.  在 "部署设置" 页上, 单击 "**下一步**"。

8.  在 "计划" 页上, 完成以下步骤:

    a.  单击 "**新建**" 以打开 "分配计划" 对话框。

    b.  单击 "**此事件之后立即分配**", 然后单击 **"确定"** 。

    c.  单击“下一步”。

9.  在 "用户体验" 页上, 完成以下步骤:

    1.  选中 "**软件安装**" 复选框。

    2.  单击 "**摘要**"。

10. 在 "摘要" 页上, 单击 "**下一步**"。

11. 在 "完成" 页上, 单击 "**关闭**"。

部署 ProfileXML 配置脚本后, 使用在生成用户集合时所选的用户帐户登录到 Windows 10 客户端计算机。 验证 VPN 客户端的配置。

>[!NOTE]
>脚本 VPN_Profile 在远程桌面会话中不起作用。 同样, 它也不能在 Hyper-v 增强会话中工作。 如果要测试虚拟机中 Always On VPN 的远程访问，请在客户端 Vm 上禁用增强会话，然后再继续。

### <a name="verify-the-configuration-of-the-vpn-client"></a>验证 VPN 客户端的配置

1.  在控制面板中的 "**系统\\安全**" 下, 单击**Configuration Manager**。 

2.  在 "Configuration Manager 属性" 对话框中的 "**操作**" 选项卡上, 完成以下步骤:

    a.  单击 "**计算机策略检索 & 评估周期**", 单击 "**立即运行**", 然后单击 **"确定"** 。

    b.  单击 "**用户策略检索 & 评估周期**", 单击 "**立即运行**", 然后单击 **"确定"** 。

    c.  单击 **“确定”** 。

3.  关闭 "控制面板"。

不久, 你应该会看到新的 VPN 配置文件。

## <a name="configure-the-vpn-client-by-using-intune"></a>使用 Intune 配置 VPN 客户端

要使用 Intune 将 Windows 10 远程访问部署 Always On VPN 配置文件, 你可以使用[创建 ProfileXML 配置文件](#create-the-profilexml-configuration-files)部分中创建的 VPN 配置文件来配置 ProfileXML CSP 节点, 或者可以使用提供的基本 EAP XML 示例在线订购.

>[!NOTE]
>Intune 现在使用 Azure AD 组。 如果 Azure AD Connect 将 VPN 用户组从本地同步到 Azure AD, 并且将用户分配给 VPN 用户组, 则可以继续。

创建 VPN 设备配置策略, 为添加到组中的所有用户配置 Windows 10 客户端计算机。 由于 Intune 模板提供 VPN 参数，因此请仅 @no__t 复制 VPN_ProfileXML 文件的 0EapHostConfig > \</EapHostConfig > 部分。

### <a name="create-the-always-on-vpn-configuration-policy"></a>创建 Always On VPN 配置策略

1.  登录到 [Azure 门户](https://portal.azure.com/)。

2.  中转到 " **Intune** > **设备配置** > " "配置**文件**"。

3.  单击 "**创建配置文件**" 以启动创建配置文件向导。

4.  输入 VPN 配置文件的**名称**和描述 (可选)。

1.   在 "**平台**" 下, 选择 " **Windows 10 或更高版本**", 然后从 "配置文件类型" 下拉选择 " **VPN** "。

     >[!TIP]
     >若要创建自定义 VPN profileXML, 请参阅[使用 Intune 应用 profileXML](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune)以了解相关说明。

2. 在 "**基本 VPN** " 选项卡下, 验证或设置以下设置:

    - **连接名称:** 在 "**设置**" 下的 " **vpn** " 选项卡中, 输入在客户端计算机上显示的 vpn 连接的名称, 例如 " _Contoso AutoVPN_"。  
    
    - **服务器**单击 "添加" 添加一个或多个 VPN 服务器 **。**
    
    - **描述**和**IP 地址或 FQDN:** 输入 VPN 服务器的描述和 IP 地址或 FQDN。 这些值必须与 VPN 服务器的身份验证证书中的使用者名称一致。 
    
    - **默认服务器:** 如果这是默认的 VPN 服务器, 请将设置为**True**。 这样做可以使此服务器作为设备用于建立连接的默认服务器。 
    
    - **连接类型:** 设置为**IKEv2**。  
    
    - **Always On:** 设置为 "**启用**" 以在登录时自动连接到 VPN, 并保持连接状态, 直到用户手动断开连接。
    
    - **每次登录时记住凭据**:用于缓存凭据的布尔值 (true 或 false)。 如果设置为 true, 则尽可能缓存凭据。

3.  将以下 XML 字符串复制到文本编辑器中:
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  将 > TrustedRootCA 替换为**5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 是示例\/中的 4b < TrustedRootCA >, 其中包含本地根证书颁发机构的证书指纹。 \<**
  
    >[!Important]
    >请不要使用下面的\<TrustedRootCA >\</TrustedRootCA > 部分中的示例指纹。  TrustedRootCA 必须是颁发 RRAS 和 NPS 服务器的服务器身份验证证书的本地根证书颁发机构的证书指纹。 **这不能是云根证书, 也不能是中间颁发 CA 证书指纹**。

5.  将示例 XML 中的 **\<ServerNames\<>/ServerNames >** 替换为在其中进行身份验证的已加入域的 NPS 的 FQDN。 

6.  复制修改后的 XML 字符串, 并将其粘贴到 "基本 VPN" 选项卡下的 " **EAP XML** " 框, 然后单击 **"确定"**
    使用 EAP Always On VPN 设备配置策略是在 Intune 中创建的。


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>将 Always On VPN 配置策略与 Intune 同步

若要测试配置策略, 请以添加到**ALWAYS ON VPN 用户**组的用户身份登录到 Windows 10 客户端计算机, 然后与 Intune 同步。

1.  在 "开始" 菜单上, 单击 "**设置**"。

2.  在 "设置" 中, 单击 "**帐户**", 然后单击 "**访问工作单位或学校**"。

3.  单击 MDM 配置文件, 然后单击 "**信息**"。

4.  单击 "**同步**" 以强制执行 Intune 策略评估和检索。

5.  关闭设置。 同步后, 会看到计算机上有可用的 VPN 配置文件。

## <a name="next-steps"></a>后续步骤

已完成部署 Always On VPN。  对于可以配置的其他功能, 请参阅下表:

|如果你想要 。  |然后查看 。  |
|---------|---------|
|为 VPN 配置条件访问    |[步骤 7.可有可无使用 Azure AD](../../ad-ca-vpn-connectivity-windows10.md)配置 VPN 连接的条件性访问:在此步骤中, 可以使用[Azure Active Directory (Azure AD) 条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)来微调已授权的 VPN 用户访问资源的方式。 使用 Azure AD 虚拟专用网络 (VPN) 连接的条件性访问, 你可以帮助保护 VPN 连接。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。         |
|了解有关高级 VPN 功能的详细信息  |[高级 VPN 功能](always-on-vpn-adv-options.md#advanced-vpn-features):本页提供有关如何启用 VPN 流量筛选器的指导, 如何使用应用程序触发器配置自动 VPN 连接, 以及如何将 NPS 配置为仅允许使用 Azure AD 颁发的证书的客户端进行 VPN 连接。        |
