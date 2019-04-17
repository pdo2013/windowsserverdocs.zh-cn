---
title: 配置 Windows 10 客户端始终启用 VPN 连接
description: 在此步骤中，你了解 ProfileXML 选项和架构，并配置 Windows 10 客户端计算机与通过 VPN 连接的基础结构进行通信。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: 6b383076686092e20448977bed3766f7d7d1c2b8
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031311"
---
# 步骤 6： 配置 Windows 10 客户端始终启用 VPN 连接

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10


& #171; [**以前：** 步骤 5。配置 DNS 和防火墙设置](vpn-deploy-dns-firewall.md)<br>
& #187;[**下一步：** 步骤 7。（可选）使用 Azure AD 的 VPN 连接条件访问](../../ad-ca-vpn-connectivity-windows10.md)

在此步骤中，你了解 ProfileXML 选项和架构，并配置 Windows 10 客户端计算机与通过 VPN 连接的基础结构进行通信。 

你可以配置始终启用 VPN 客户端通过 PowerShell、 SCCM 或 Intune。 所有三个需要 XML VPN 配置文件配置适当的 VPN 设置。 自动 PowerShell 无需 SCCM 或 Intune 的组织的注册有可能。

>[!NOTE]
>组策略不包括管理模板来配置 windows 10 远程访问始终启用 VPN 客户端。  但是，你可以使用登录脚本。

## ProfileXML 概述

ProfileXML 是 VPNv2 CSP 中的 URI 节点。 而不是逐个配置每个 VPNv2 CSP 节点 — 例如触发器，路由列表和身份验证协议-使用此节点配置 windows 10 VPN 客户端通过作为单个 XML 块到单个云解决方案提供商节点中提供的所有设置。 ProfileXML 架构与 VPNv2 CSP 节点的架构的匹配几乎相同，但一些术语略有不同。

使用此部署描述，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune 的所有传递方法中的 ProfileXML。 有两种方法在此部署中配置的 ProfileXML VPNv2 CSP 节点：

- **OMA DM**。 一种方法是使用 MDM 提供程序使用 OMA DM，如前面所述的部分[VPNv2 CSP 节点](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes)。 使用此方法，你可以轻松地将 VPN 配置文件配置 XML 标记插入 ProfileXML 云解决方案提供商节点时使用 Intune。

- **Windows Management Instrumentation (WMI) 到 CSP 桥**。 配置 ProfileXML 云解决方案提供商节点的第二个方法是使用 WMI 到 CSP 桥-WMI 类调用**MDM_VPNv2_01**—，还可以访问 VPNv2 CSP 和 ProfileXML 节点。 当你创建该 WMI 类的新实例时，WMI 使用云解决方案提供商时使用 Windows PowerShell 和 System Center Configuration Manager 创建 VPN 配置文件。

即使这些配置方法不同，都需要的格式正确的 XML VPN 配置文件。 若要使用 ProfileXML VPNv2 CSP 设置，请使用 ProfileXML 架构配置必要的简单的部署方案的标记构造 XML。 有关详细信息，请参阅[ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd)。

下面你找到每个所需的设置和其相应的 ProfileXML 标记。 在 ProfileXML 架构中的特定标记中配置的每个设置，并非所有的本机配置文件下找到。 有关其他标记放置，请参阅 ProfileXML 架构。

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**连接类型：** 本机 IKEv2

ProfileXML 元素：

    <NativeProtocolType>IKEv2</NativeProtocolType>

**路由：** 拆分隧道

ProfileXML 元素：

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**名称解析：** 域名信息列表和 DNS 后缀

ProfileXML 元素：

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**触发：** 始终启用和受信任的网络检测

ProfileXML 元素：

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**身份验证：** PEAP TLS 使用 TPM 保护的用户证书

ProfileXML 元素：

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

你可以使用简单的标记配置一些 VPN 身份验证机制。 但是，EAP 和 PEAP 是更多地参与。 若要创建的 XML 标记的最简单方法是使用其 EAP 的设置配置的 VPN 客户端，然后将该配置导出为 XML。 

有关 EAP 设置的详细信息，请参阅[EAP 配置](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration)。 

## <a name="bkmk_profile"></a>手动创建模板连接配置文件

在此步骤中，你可以使用受保护的可扩展身份验证协议 \(PEAP\) 来保护客户端和服务器之间的通信。 与不同的简单的用户名和密码，该连接要求要处理的 VPN 配置文件中的唯一 EAPConfiguration 部分。 

而不是描述了如何从头开始创建的 XML 标记，你使用设置在 Windows 中创建模板 VPN 配置文件。 创建模板 VPN 配置文件之后, 你使用 Windows PowerShell 来使用更高版本中部署创建部署最终 ProfileXML 该模板中的 EAPConfiguration 部分。

### 记录 NPS 证书设置

创建模板之前, 需要注意的主机名或服务器证书 NPS 服务器的完全限定的域名 (FQDN) 和颁发证书的 CA 的名称。

**步骤：**

1.  在 NPS 服务器上，打开网络策略服务器。

2.  在 NPS 控制台中，在策略单击**网络策略**。

3.  右键单击**虚拟专用网络 \(VPN\) 连接**，然后单击**属性**。

4.  单击**限制**选项卡，然后单击**身份验证方法**。

5.  在 EAP 类型中，单击**Microsoft： 受保护的 EAP (PEAP)**，并单击**编辑**。

6.  **证书颁发给**和**颁发者**记录的值。<p>在即将推出的 VPN 模板配置中使用这些值。 例如，如果服务器的 FQDN 是 nps01.corp.contoso.com 并且主机名 NPS01，证书名称基于服务器-例如，nps01.corp.contoso.com 的 FQDN 或 DNS 名称。

7.  取消编辑受保护的 EAP 属性对话框。

8.  取消虚拟专用网络 (VPN) 连接属性对话框。

9.  关闭网络策略服务器。

>[!NOTE]
>如果你有多个 NPS 服务器，完成每个硬盘驱动器上的这些步骤，以便在 VPN 配置文件可以验证每个应使用它们。

### 配置加入 domain\ 的客户端计算机上的模板 VPN 配置文件

既然你已配置已加入域的客户端计算机上的模板 VPN 配置文件的必要信息。 你使用的用户帐户的类型 \ （即，标准用户或 administrator\） 有关此过程的一部分并不重要。 

但是，如果你尚未配置证书自动注册后重新启动计算机，执行此操作之前配置 VPN 连接，以确保你有可用的证书注册其上的模板。

>[!NOTE]
>没有方法手动添加的 VPN，如 NRPT 规则，始终上，任意高级的属性受信任的网络检测，等等。在下一步的步骤中，你创建一个测试 VPN 连接，以确认配置的 VPN 服务器，并且你可以建立 VPN 连接到服务器。

**手动创建一个测试 VPN 连接**

1.  **VPN 用户**组的成员身份登录到已加入域的客户端计算机。

2.  在开始菜单中，键入**VPN**，然后按 Enter。

3.  在详细信息窗格中，单击**添加 VPN 连接**。

4.  在 VPN 提供程序列表中，单击**Windows （内置）**。

5.  在连接名称中，键入**模板**。

6.  在服务器名称或地址，键入**外部**VPN 服务器的 FQDN \ (例如，**vpn.contoso.com**\)。

7.  单击**保存**。

8.  下相关的设置，单击**更改适配器选项**。

9.  右键单击**模板**，然后单击**属性**。

10. 在**安全**选项卡**VPN 类型**，单击**IKEv2**。

11. 在数据加密，请单击**最大强度加密**。

12. 单击**使用可扩展身份验证协议 (EAP)**;然后，在**使用可扩展身份验证协议 (EAP)**，单击**Microsoft： 受保护 EAP (PEAP) （启用加密）**。

13. 单击**属性**以打开受保护的 EAP 属性对话框中，并完成以下步骤：

    a. 在**连接到这些服务器**框中，键入从 NPS 服务器身份验证设置在本部分 (例如，NPS01) 的前面部分中检索 NPS 服务器的名称。

    >[!NOTE]
    >键入的服务器名称必须与证书中的名称相匹配。 恢复此名称在本部分的前面部分中。 如果名称不匹配，连接将失败，指出，"连接已阻止由于 RAS/VPN 服务器上配置的策略。

    b.  在受信任的根证书颁发机构下选择根 CA 颁发 NPS 服务器的证书 (例如，contoso CA)。

    c.  在之前连接的通知，单击**不要求用户要授权新服务器或受信任的 Ca**。

    d.  在选择身份验证方法中，单击**智能卡或其他证书**，然后单击**配置**。 智能卡或其他证书属性对话框打开。

    e.  单击**使用此计算机上的证书**。

    f.  在连接到这些服务器中，输入您从 NPS 服务器身份验证设置之前步骤中检索到的 NPS 服务器的名称。

    g.  在受信任的根证书颁发机构，选择根 CA 颁发 NPS 服务器证书。

    h.  选择**不提示用户验证新服务器或受信任的证书颁发机构**复选框。

    i.  单击**确定**以关闭智能卡或其他证书属性对话框。

    j.  单击**确定**关闭受保护的 EAP 属性对话框。

10. 单击**确定**关闭模板属性对话框。

11. 关闭网络连接窗口。

12. 在设置中，通过单击某个**模板**，然后单击**连接**测试 VPN。

>[!IMPORTANT]
>请确保模板 VPN 连接到 VPN 服务器成功。 执行此操作可确保 EAP 设置正确，然后在下一个示例中使用它们。 你必须至少一次然后再继续; 连接否则，该配置文件将不包含连接到 VPN，所需的所有信息。

## <a name="bkmk_ProfileXML"></a>创建 ProfileXML 配置文件

之前完成此部分，请确保你已创建和测试的模板[手动创建模板连接配置文件](#bkmk_profile)部分介绍的 VPN 连接。 测试 VPN 连接，需要确保个人资料包含连接到 VPN，所需的所有信息。

一览 1 中的 Windows PowerShell 脚本创建两个文件在桌面上，这两种包含基于你之前创建的模板连接配置文件的**EAPConfiguration**标记：

-   **VPN_Profile.xml。** 此文件包含所需配置 ProfileXML 节点 VPNv2 CSP 中的 XML 标记。 OMA DM – 兼容 MDM 服务，如 Intune 中使用此文件。

-   **VPN_Profile.ps1。** 此文件是你可以运行一个 Windows PowerShell 脚本配置 ProfileXML 节点 VPNv2 CSP 中的客户端计算机上。 你还可以通过部署此脚本通过 System Center Configuration Manager 配置云解决方案提供商。 不能在远程桌面会话中，包括 HYPER-V 增强会话运行此脚本。

>[!IMPORTANT]
>下面的示例命令需要 Windows 10 版本 1607年或更高版本。

**创建 VPN_Profile.xml 和 VPN_Proflie.ps1**

1. 登录到包含该模板具有相同的用户的 VPN 配置文件的已加入域的客户端计算机帐户的部分"手动创建模板连接配置文件"所述。

2. 将列表 1 粘贴到 Windows PowerShell 集成脚本环境 \(ISE\)，并自定义在注释中所述的参数。 这些是 $Template、 $ProfileName、 $Servers、 $DnsSuffix、 $DomainName、 $TrustedNetwork，以及 $DNSServers。 在注释中每个设置的完整说明。

3.  运行脚本，在桌面上生成**VPN_Profile.xml**和**VPN_Profile.ps1** 。

#### 列表 1。 了解 MakeProfile.ps1

本部分介绍可以使用以了解如何创建 VPN 配置文件，专门用于配置 ProfileXML VPNv2 CSP 中的示例代码。

此示例代码从装配脚本并运行该脚本，该脚本会生成两个文件后： VPN_Profile.xml 和 VPN_Profile.ps1。 使用 VPN_Profile.xml OMA DM 兼容 MDM 服务，如 Microsoft Intune 中配置 ProfileXML。

使用 Windows PowerShell 或 System Center Configuration Manager 中的**VPN_Profile.ps1**脚本在 Windows 10 桌面版上配置 ProfileXML。

>[!NOTE]
>若要查看完整的示例脚本，请参阅部分[MakeProfile.ps1 完整脚本](#bkmk_fullscript)。

#### 参数

配置以下参数：

**$Template**。 要从中检索 EAP 配置的模板的名称。

**$ProfileName**。 配置文件的唯一字母数字标识符。 配置文件名称不能包含正斜杠 （/）。 如果配置文件的名称有空格或其他非字母数字字符，它必须正确转义根据 URL 编码标准。

**$Servers**。 公共或可路由 IP 地址或 VPN 网关的 DNS 名称。 网关 IP 或服务器场的虚拟 IP，它可以指向外部。 示例中，208.147.66.130 或 vpn.contoso.com。

**$DnsSuffix**。 指定一个或多个逗号分隔的 DNS 后缀。 在列表中的第一个也用作 VPN 接口的主要的特定于连接的 DNS 后缀。 将整个列表也将添加到 SuffixSearchList。

**$DomainName**。 用于指示该策略应用到该命名空间。 当发出名称查询后时，DNS 客户端会比较查询所有下 DomainNameInformationList 以查找匹配的命名空间中的名称。 此参数可以是以下类型之一：

- FQDN-完全限定的域名
- 后缀的域后缀将追加到的短名称查询的 DNS 解析。 若要指定后缀，添加前缀一段内 \ （.） 为 DNS 后缀。

**$DNSServers**。 列表的以逗号分隔的 DNS 服务器 IP 地址用于命名空间。

**$TrustedNetwork**。 用于标识受信任的网络的以逗号分隔的字符串。 不会不自动连接 VPN 时用户位于受保护的资源的设备可直接访问公司无线网络。

以下是以下命令中使用的参数的示例值。 确保为你的环境更改这些值。

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### 准备和创建配置文件 XML

以下示例命令获取模板配置文件中 EAP 设置：


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### 创建配置文件 XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

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


### Intune 输出 VPN_Profile.xml

你可以使用下面的示例命令保存配置文件 XML 文件：

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### 桌面和 System Center Configuration Manager 的输出 VPN_Profile.ps1

下面的代码示例通过使用 VPNv2 CSP 中的 ProfileXML 节点配置 AlwaysOn IKEv2 VPN 连接。

在 Windows 10 桌面版或 System Center Configuration Manager 中，你可以使用此脚本。

### 定义键 VPN 配置文件参数

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## 定义 VPN ProfileXML

    $ProfileXML = ''' + $ProfileXML + '''
    
### 配置文件中转义特殊字符

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### 定义 WMI 到 CSP 桥属性

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### 确定用户 SID VPN 配置文件：

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
    

### 定义 WMI 会话：

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### 检测和删除以前的 VPN 配置文件：

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
    

### 创建 VPN 配置文件：

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

### 保存配置文件 XML 文件

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>MakeProfile.ps1 完整脚本

大多数示例使用 Set-wmiinstance Windows PowerShell cmdlet ProfileXML 插入 MDM_VPNv2_01 WMI 类的新实例。 

但是，这不会不工作 System Center Configuration Manager 中，因为不能在最终用户的上下文中运行程序包。 因此，此脚本使用通用的信息模型在用户的上下文中，创建 WMI 会话，然后在该会话中创建新的 MDM_VPNv2_01 WMI 类的实例。 此 WMI 类使用 WMI 到 CSP 桥配置 VPNv2 CSP。 因此，通过添加类实例，你配置云解决方案提供商。 

>[!IMPORTANT]
>WMI 到 CSP 桥设计使然需要本地管理员权限。 若要每用户 VPN 配置文件部署你应使用 SCCM 或 mdm。

>[!NOTE]
>脚本 VPN_Profile.ps1 使用当前用户的 SID 来标识用户的上下文。 由于没有 SID 是远程桌面会话中可用，该脚本将不工作远程桌面会话中。 同样，它并非适用 HYPER-V 增强会话中。 如果你要在虚拟机中测试远程访问始终启用 VPN，禁用你的客户端虚拟机上运行此脚本之前的增强的会话。

以下示例脚本包括所有从上一节中的代码示例。 请确保将示例值更改为适合你的环境的值。
    
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

## 通过使用 Windows PowerShell 配置 VPN 客户端

若要在 Windows 10 客户端计算机上配置 VPNv2 CSP，运行你在[创建配置文件 XML](#create-the-profile-xml)部分中创建的 VPN_Profile.ps1 Windows PowerShell 脚本。 打开 Windows PowerShell 以管理员身份;否则，你将收到错误消息指出，_访问被拒绝_。

完成后运行 VPN_Profile.ps1 配置 VPN 配置文件，你可以验证在任何时间已成功通过在 Windows PowerShell ISE 中运行以下命令：

    Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01

**成功从 Get-wmiobject cmdlet 的结果**


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

ProfileXML 配置必须正确在结构、 拼写检查、 配置和有时字母大小写。 如果你看到中结构列表 1 到不同的地方，可能会在 ProfileXML 标记包含一个错误。

如果你需要进行故障排除标记，很容易将其放在比以解决在 Windows PowerShell ISE 中 XML 编辑器中。 在任一情况下，开始菜单最简单版本的配置文件，并添加的组件返回该问题直到一次一个地再次发生。

## <a name="vpn-deploy-client-sccm"></a>通过使用 System Center Configuration Manager 配置 VPN 客户端

在 System Center Configuration Manager 中，你可以通过使用 ProfileXML 云解决方案提供商节点中，就像 Windows PowerShell 中未部署 VPN 配置文件。 在这里，你使用你在[创建 ProfileXML 配置文件](#bkmk_ProfileXML)部分中创建的 VPN_Profile.ps1 Windows PowerShell 脚本。

若要使用 System Center Configuration Manager 将远程访问始终启用 VPN 配置文件部署到 Windows 10 客户端计算机，你必须首先创建一组计算机或用户向其部署配置文件。 在此方案中，创建部署配置脚本的用户组。

### 创建用户组

1.  在配置管理器控制台中，打开资源和 Compliance\\User 集合。

2.  在**家庭版**功能区的**创建**组中，单击**创建用户集合**。

3.  在常规页面，请完成以下步骤：

    a. 在**名称**中，键入**VPN 用户**。

    b. 单击**浏览**，单击**所有用户**，都单击**确定**。

    c. 单击**下一步**。

4.  在成员身份规则页面上完成以下步骤：

    a.  在**成员身份规则**，单击**添加规则**并单击**直接规则**。 在此示例中，你要添加到用户集合的单个用户。 但是，你可以使用查询规则将用户添加到动态对于大规模部署此集合。

    b.  在**欢迎**页上，单击**下一步**。

    c.  在搜索资源页上，在**值**中，键入你想要添加的用户的名称。 资源名称中包括用户的域。 若要包含基于部分匹配的结果，插入**%** 在您的搜索条件的任一端字符。 例如，若要查找包含字符串"lori"的所有用户，键入 **%lori%**。 单击**下一步**。

    d.  在选择资源页中，选择你想要将添加到组的用户，然后单击**下一步**。

    e.  在摘要页上，单击**下一步**。

    f.  在完成页上，单击**关闭**。

6.  返回在创建用户集合向导的成员身份规则页上，单击**下一步**。

7.  在摘要页上，单击**下一步**。

8.  在完成页上，单击**关闭**。

创建用户组以接收 VPN 配置文件后，你可以创建程序包和程序部署在[创建 ProfileXML 配置文件](#bkmk_ProfileXML)部分中创建的 Windows PowerShell 配置脚本。

### 创建包含 ProfileXML 配置脚本的包

1.  托管站点服务器计算机帐户可访问的网络共享上的脚本 VPN_Profile.ps1。

2.  在配置管理器控制台中，打开**软件 Library\\Application Management\\Packages**。

3.  在**家庭版**功能区的**创建**组中，单击**创建程序包**开始创建程序包和程序向导。

4.  在程序包页面中，请完成以下步骤：

    a. 在**名称**中，键入**Windows 10 始终在 VPN 配置文件**。

    b. 选择**此包包含源文件**复选框，然后单击**浏览**。

    c. 在设置源文件夹对话框中，单击**浏览**、 选择包含 VPN_Profile.ps1 的文件共享和单击**确定**。<p>请确保你选择的网络路径，而不是本地路径。 换句话说，路径应该*\\fileserver\\vpnscript*，不*c:\\vpnscript*类似。

1.  单击**下一步**。

2.  在计划类型页上单击**下一步**。

3.  在标准程序页面上，请完成以下步骤：

    a.  在**名称**中，键入**VPN 配置文件脚本**。

    b.  在**命令行**中，键入**PowerShell.exe ExecutionPolicy 绕过的文件"VPN_Profile.ps1"**。

    c.  在**运行模式**，请单击**使用管理员权限运行**。

    d.  单击**下一步**。

4.  在要求页上，请完成以下步骤：

    a.  选择**此计划可以仅在指定的平台上运行**。

    b.  选择**所有 Windows 10 （32 位）** 和**所有 Windows 10 （64 位）** 复选框。

    c.  在**估计磁盘空间**，键入**1**。

    d.  在**最大允许运行的时 （分钟）**，键入**15**。

    e.  单击**下一步**。

5.  在摘要页上，单击**下一步**。

6.  在完成页上，单击**关闭**。

程序包和程序创建后，你需要将其部署到**VPN 用户**组。

### 部署 ProfileXML 配置脚本

1.  在配置管理器控制台中，打开软件 Library\\Application Management\\Packages。

2.  在**程序包**中单击**Windows 10 始终在 VPN 配置文件**。

3.  在**程序**选项卡上，底部的详细信息窗格中，右键单击**VPN 配置文件脚本**、 单击**属性**并完成以下步骤：

    a.  在**高级**选项卡中，**此程序分配到计算机时**，单击**一次用于登录的每个用户**。

    b.  单击**确定**。

4.  右键单击**VPN 配置文件脚本**，然后单击**部署**以开始部署软件向导。

5.  在常规页面，请完成以下步骤：

    a.  旁边**集合**中，单击**浏览**。

    b.  在**集合类型**列表 （左上） 中，单击**用户集合**。

    c.  单击**VPN 用户**，然后单击**确定**。

    d.  单击**下一步**。

6.  在内容页面上，请完成以下步骤：

    a.  单击**添加**，然后单击**分发点**。

    b.  在**可用的分发点**，选择你想要分发 ProfileXML 配置脚本，然后单击**确定**分发点。

    c.  单击**下一步**。

7.  在部署设置页上单击**下一步**。

8.  在计划页中，请完成以下步骤：

    a.  单击**新建**以打开分配计划对话框。

    b.  单击**此事件后立即分配**，然后单击**确定**。

    c.  单击**下一步**。

9.  在用户体验页面上，请完成以下步骤：

    1.  选择**软件安装**复选框。

    2.  单击**摘要**。

10. 在摘要页上，单击**下一步**。

11. 在完成页上，单击**关闭**。

使用 ProfileXML 配置脚本部署，登录到 Windows 10 客户端计算机时生成的用户集合所选的用户帐户。 验证 VPN 客户端的配置。

>[!NOTE]
>脚本 VPN_Profile.ps1 中远程桌面会话不起作用。 同样，它并非适用 HYPER-V 增强会话中。 如果在虚拟机中测试远程访问始终启用 VPN，禁用增强的会话在你的客户端虚拟机上再继续操作。

### 验证 VPN 客户端的配置

1.  在控制面板下**System\\Security**，单击**配置管理器**。 

2.  在配置管理器属性对话框中的在**操作**选项卡上，请完成以下步骤：

    a.  单击**计算机策略检索 & 评估周期**，单击**立即运行**，单击**确定**。

    b.  单击**用户策略检索 & 评估周期**，单击**立即运行**，单击**确定**。

    c.  单击**确定**。

3.  关闭控制面板。

你应该不久看到新的 VPN 配置文件。

## 通过使用 Intune 配置 VPN 客户端

若要使用 Intune 部署 Windows 10 远程访问始终启用 VPN 配置文件，你可以通过使用在[创建 ProfileXML 配置文件](#bkmk_ProfileXML)，部分中创建的 VPN 配置文件配置 ProfileXML 云解决方案提供商节点，或者你可以使用提供的基本 EAP XML 示例下面。

>[!NOTE]
>现在，Intune 使用 Azure AD 组。 如果 Azure AD Connect 同步到 Azure AD，VPN 用户组中的本地，并且用户分配给 VPN 用户组，你就可以继续。

创建 VPN 设备配置策略来配置 Windows 10 客户端计算机的所有用户添加到组。 由于 Intune 模板提供了 VPN 参数，仅复制 VPN_ProfileXML 文件的 \<EapHostConfig> \</EapHostConfig> 部分。 


### 创建始终启用 VPN 配置策略

1.  登录到[Azure 门户](https://portal.azure.com/)。

2.  转到**Intune** > **设备配置** > **配置文件**。

3.  单击**创建配置文件**以开始创建配置文件向导。

4.  用于 VPN 配置文件和描述 （可选） 输入的**名称**。

5.   在**平台**选择**Windows 10 或更高版本**，并从配置文件类型下拉列表中选择**VPN** 。

     >[!TIP]
     >如果你要创建自定义 VPN profileXML，请参阅[应用 ProfileXML 使用 Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune)说明。

6. 在**基本 VPN**选项卡上，验证或设置以下设置：

    - **连接名称：** 出现在客户端计算机中**VPN**选项卡下的**设置**，例如， _Contoso AutoVPN_输入 VPN 连接的名称。  
    
    - **服务器：** 通过单击**添加**添加一个或多个 VPN 服务器
    
    - **描述**和**IP 地址或 FQDN:** 输入的描述和 IP 地址或 VPN 服务器的 FQDN。 这些值必须与 VPN 服务器身份验证证书中的使用者名称保持一致。 
    
    - **默认服务器：** 如果这是默认的 VPN 服务器，则设置为**True**。 执行此操作支持此服务器作为默认的服务器的设备使用建立连接。 
    
    - **连接类型：** 设置为**IKEv2**。  
    
    - **始终启用：** 设置为**启用**以连接到 VPN，在登录自动并保持连接状态直到用户手动断开连接。
    
    - **在每次登录记住凭据**： 用于缓存的凭据的布尔值 （true 或 false）。 如果设置为 true，凭据将缓存尽可能。

7.  将以下 XML 字符串复制到文本编辑器中：<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  替换**\<TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 是 4b<\/TrustedRootCA>** 在该示例中使用这两个位置中你的本地根证书颁发机构的证书指纹。
  
    >[!Important]
    >在下面的 \<TrustedRootCA>\</TrustedRootCA> 部分不使用示例指纹。  TrustedRootCA 必须在本地根证书颁发机构颁发 RRAS 和 NPS 服务器的服务器身份验证证书的证书指纹。 **这不能云根证书，也不中间的颁发 CA 证书指纹**。

10. 在该示例 XML **\<ServerNames>NPS.contoso.com\</ServerNames>** 替换为已加入域的 NPS 位置进行身份验证的 FQDN。 

11. 将修改后的 XML 字符串复制和粘贴到**EAP Xml**中的基本 VPN 选项卡下单击**确定**。<p>在 Intune 中创建使用 EAP 始终在 VPN 设备配置策略。


### 同步使用 Intune 始终启用 VPN 配置策略

若要测试配置策略，以添加到**始终在 VPN 用户**组中，用户身份登录到 windows 10 客户端计算机，然后使用 Intune 同步。

1.  在开始菜单上，单击**设置**。

2.  在设置中，单击**帐户**，然后单击**访问工作单位或学校**。

3.  单击 MDM 配置文件，然后单击**信息**。

4.  单击**同步**强制 Intune 策略评估和检索。

5.  关闭设置。 同步后，你可以在计算机上看到可用的 VPN 配置文件。

## 下一步
你已完成部署始终启用 VPN。  你可以配置其他功能，请参阅下表：

|如果你想要...  |请参阅...  |
|---------|---------|
|Vpn 配置条件访问    |[步骤 7。（可选）配置条件访问使用 Azure AD 的 VPN 连接](../../ad-ca-vpn-connectivity-windows10.md)： 在此步骤中，你可以微调如何授权的 VPN 用户访问使用[Azure Active Directory (Azure AD) 条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)的资源。 通过 Azure AD 的条件访问虚拟专用网络 (VPN) 连接，可帮助保护 VPN 连接。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。         |
|了解有关高级 VPN 功能的详细信息  |[高级 VPN 功能](always-on-vpn-adv-options.md#advanced-vpn-features)： 此页提供有关如何启用 VPN 流量筛选器、 如何配置自动 VPN 连接使用应用的触发器，以及如何将 NPS 配置为仅使用 Azure 颁发的证书从客户端允许 VPN 连接的指南广告。        |


---
