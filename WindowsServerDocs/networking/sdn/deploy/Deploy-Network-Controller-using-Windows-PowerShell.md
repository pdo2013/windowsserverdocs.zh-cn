---
title: 部署使用 Windows PowerShell 网络控制器
description: 本主题介绍了如何使用 Windows PowerShell 部署网络控制器一个或多个计算机运行的 Windows Server 2016 的虚拟机 (Vm) 上的说明进行操作。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cfd06662f317381fb77bf31db5ed60c2489ff871
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-controller-using-windows-powershell"></a>部署使用 Windows PowerShell 网络控制器

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍了如何使用 Windows PowerShell 部署网络控制器上一个或多个虚拟机 (Vm) 运行 Windows Server 2016 的说明进行操作。

>[!IMPORTANT]
>不要部署物理主机上的了网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色 Hyper-V 虚拟机上 \(VM\) Hyper-V 主机上安装。 通过添加到网络控制器使用 Windows PowerShell 命令主机上安装了网络控制器在虚拟机上三个不同的 Hyper\ V 主机后，必须启用软件定义网络 \(SDN\) Hyper\ V 主机**新建 NetworkControllerServer**。 通过执行此操作，你启用 SDN 软件负载平衡函数。 有关详细信息，请参阅[新建 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

本主题包含以下部分。

- [安装网络控制器服务器角色](#bkmk_role)

- [配置了网络控制器群集](#bkmk_configure)

- [配置了网络控制器应用程序](#bkmk_app)

- [网络控制器部署验证](#bkmk_validation)

- [其他 Windows PowerShell 命令网络控制器](#bkmk_ps)

- [示例网络控制器配置脚本](#bkmk_script)

- [后 Post-Deployment 非 Kerberos 部署的步骤](#bkmk_nonkerb)

## <a name="bkmk_role"></a>安装网络控制器服务器角色

你可以使用此过程虚拟机上安装了网络控制器服务器角色 \(VM\)。

>[!IMPORTANT]
>不要部署物理主机上的了网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色 Hyper-V 虚拟机上 \(VM\) Hyper-V 主机上安装。 有三种不同的 Hyper\ V 主机上在虚拟机上安装网络控制器后，你必须启用软件定义网络 \(SDN\) Hyper\ V 主机，通过添加到网络控制器主机。 通过执行此操作，你启用 SDN 软件负载平衡函数。

在会员**管理员**，或等效的最低要求执行此过程。  

>[!NOTE]
>如果你想要使用而不是 Windows PowerShell 服务器管理器安装网络控制器、查看[安装网络控制器服务器角色使用服务器管理器](https://technet.microsoft.com/library/mt403348.aspx)

若要使用的 Windows PowerShell 安装网络控制器，在 Windows PowerShell 提示符下，键入以下命令，然后按 ENTER。

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

安装网络控制器要求你重启计算机。 为此，请键入以下命令，，然后按 ENTER。

`Restart-Computer`

## <a name="bkmk_configure"></a>配置了网络控制器群集

高可用性和到该网络控制器应用程序，您可以配置后创建群集，并在群集顶部中托管其可扩展性，提供了网络控制器群集。

>[!NOTE]
>你可以执行过程以下部分中可以直接在 VM 安装网络控制器、或可用于 Windows Server 2016 远程服务器管理工具，请执行过程远程计算机的运行的 Windows Server 2016 或 Windows 10 中的位置。 此外，在会员**管理员**，或等效的最低要求执行此过程。 如果计算机或在其你安装了网络控制器 VM 已加入域，你的用户帐户必须成员**域用户**。

你可以通过创建节点对象，然后配置群集创建网络控制器群集。

### <a name="create-a-node-object"></a>创建节点对象

你需要为每个成员网络控制器群集的 VM 创建节点对象。

若要创建节点对象，在 Windows PowerShell 命令提示符下，键入以下命令，然后按 ENTER。 确保你添加适合你的部署的值为每个参数。  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

下表介绍的每个参数**新建 NetworkControllerNodeObject**命令。

|参数|描述|
|-------------|---------------|
|名称|**名称**参数指定你希望将添加到群集服务器的友好名称|
|服务器|**服务器**参数指定的主名称、完全合格域 (FQDN) 或你想要添加到群集服务器 IP 地址。 对于加入域的计算机，FQDN 是必需的。|
|FaultDomain|**FaultDomain**参数指定你添加到群集该服务器失败域。 此参数定义你添加到群集的服务器时，可能会遇到失败的服务器。 这种失败可能是由于共享物理依赖关系，如电源和网络的来源。 故障域通常表示分层与这些共享依赖关系，可能无法在一起从故障域树中的高点的更多服务器。 在运行时，网络控制器视为在群集故障域，并且尝试以分散网络控制器服务，以使它们在单独的故障域。 此过程有助于确保出现故障的任何一个错误域，该服务和及其状态的可用性未受到威胁后。 故障域中分层格式指定。 例如:"Fd: / DC1/机架 1 月 Host1"、在 DC1 是 datacenter 名称、机架 1 是机架名称和 Host1 是节点位于的主机的名称。|
|RestInterface|**RestInterface**参数了终止具象传输状态 (REST) 通信节点上指定的接口名称。 该网络控制器接口从网络管理层接收 Northbound API 请求。|
|NodeCertificate|**NodeCertificate**参数指定网络控制器用于计算机身份验证的证书。 如果你使用用于群集; 内部通信的证书基于身份验证证书是必需证书还用于加密的服务网络控制器之间进行通信。 证书主题名称必须节点 DNS 名称相同。|

### <a name="configure-the-cluster"></a>配置群集

若要配置群集，在 Windows PowerShell 命令提示符下，键入以下命令，然后按 ENTER。 确保你添加适合你的部署的值为每个参数。

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

下表介绍的每个参数**安装 NetworkControllerCluster**命令。
  
|参数|描述|
|-------------|---------------|
|ClusterAuthentication|**ClusterAuthentication**参数指定可用于保护节点间通信，还可用于加密的服务网络控制器之间进行通信的身份验证类型。 受支持的值为**Kerberos**，**X509**和**无**。 Kerberos 身份验证使用域帐户，并且仅可网络控制器节点是否能加入域。 如果指定 X509 基于身份验证，你必须提供 NetworkControllerNode 对象中的证书。 此外，你必须手动配置证书，运行此命令之前。|
|ManagementSecurityGroup|**ManagementSecurityGroup**参数指定安全组包含允许从远程计算机上运行管理 cmdlet 的用户的名称。 此功能仅适用 ClusterAuthentication 是否 Kerberos。 在本地计算机上，你必须指定域安全组并不安全组。|
|节点|**节点**参数指定列表中，通过使用你创建网络控制器节点**新建 NetworkControllerNodeObject**命令。|
|DiagnosticLogLocation|**DiagnosticLogLocation**参数指定共享位置定期上载诊断日志。 如果你没有指定的值为此参数，日志本地存储在以下位置每个节点。 在文件夹 %systemdrive%\windows\tracing\sdndiagnostics 本地保存日志。 在文件夹 %systemdrive%\ProgramData\ Microsoft \Service Fabric\log\Traces 本地保存群集日志。|
|LogLocationCredential|**LogLocationCredential**参数指定访问存储日志共享位置的所需的凭据。|
|CredentialEncryptionCertificate|**CredentialEncryptionCertificate**参数指定的证书，使用加密用于访问网络控制器二进制文件的凭据网络控制器和**LogLocationCredential**、如果指定。 必须上的所有网络控制器在运行此命令之前节点中预都配证书，必须对所有群集节点都登记同一个证书。 使用此参数保护网络控制器二进制文件和日志建议生产环境中。 如果不此参数，凭据存入清晰的文本和任何未经授权的用户可能会滥用。|
|凭据|在远程计算机运行此命令，才需要此参数。 **凭据**参数指定的用户帐户有权目标计算机上运行该命令。|
|CertificateThumbprint|在远程计算机运行此命令，才需要此参数。 **CertificateThumbprint**参数指定数字公钥证书 (X509) 的用户帐户有权目标计算机上运行该命令。|
|UseSSL|在远程计算机运行此命令，才需要此参数。 **UseSSL**参数指定用于建立连接到远程计算机的安全套接字层 (SSL) 协议。 默认情况下，无法使用 SSL。|
|计算机名称|**ComputerName**参数指定在其，将是运行此命令网络控制器节点。 如果你没有指定的值为此参数，默认情况下使用本地计算机。|
|LogSizeLimitInMBs|此参数 mb，程序可以存储网络控制器指定日志最大大小。 日志会存储在循环方式。 提供 DiagnosticLogLocation，此参数的默认值为 40 GB。 如果未提供 DiagnosticLogLocation，存储在网络控制器节点日志和此参数的默认值 15 GB。|
|LogTimeLimitInDays|此参数天，用于存储日志中指定的持续时间限制。 日志会存储在循环方式。 此参数的默认值为 3 天。|

## <a name="bkmk_app"></a>配置了网络控制器应用程序
若要配置网络控制器应用程序，在 Windows PowerShell 命令提示符下，键入以下命令，然后按 ENTER。 确保你添加适合你的部署的值为每个参数。

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

下表介绍的每个参数**安装 NetworkController**命令。

|参数|描述|
|-------------|---------------|
|ClientAuthentication|**ClientAuthentication**参数指定用于保护其余和网络控制器之间的通信的身份验证类型。 受支持的值为**Kerberos**，**X509**和**无**。 Kerberos 身份验证使用域帐户，并且仅可网络控制器节点是否能加入域。 如果指定 X509 基于身份验证，你必须提供 NetworkControllerNode 对象中的证书。 此外，你必须手动配置证书，运行此命令之前。|
|节点|**节点**参数指定列表中，通过使用你创建网络控制器节点**新建 NetworkControllerNodeObject**命令。|
|ClientCertificateThumbprint|仅当你使用的证书基于身份验证网络控制器客户端此参数是必需的。 **ClientCertificateThumbprint**参数指定的指纹注册上 Northbound 层的客户端的证书。|
|服务器证书|**服务器证书**参数指定网络控制器用于证明向客户端其身份的证书。 服务器证书必须在增强键使用扩展包含服务器身份验证目的，并且必须颁发给网络控制器 ca 受信任的客户端。|
|RESTIPAddress|不需要指定的值**RESTIPAddress**与网络控制器的单个节点部署。 多个节点部署**RESTIPAddress**参数指定 CIDR 法中的其余部分端点的 IP 地址。 例如，192.168.1.10 月 24。 对象名称值**服务器证书**必须解析的值为**RESTIPAddress**参数。 所有节点相同子网时，此参数必须都指定多个节点网络控制器的所有部署。 如果在不同的子网上节点，必须使用**RestName**而不是使用参数**RESTIPAddress**。|
|RestName|不需要指定的值**RestName**与网络控制器的单个节点部署。 仅时间必须指定的值**RestName**当多个节点部署具有节点，在不同的个子网。 多个节点部署**RestName**参数指定网络控制器群集 FQDN。|
|ClientSecurityGroup|**ClientSecurityGroup**参数指定其成员均网络控制器客户端 Active Directory 安全组的名称。 仅当你使用的 Kerberos 身份验证，这参数是必需**ClientAuthentication**。 安全组必须包含从中访问 REST Api、客户，你必须创建安全组并运行此命令之前添加成员。|
|凭据|在远程计算机运行此命令，才需要此参数。 **凭据**参数指定的用户帐户有权目标计算机上运行该命令。|
|CertificateThumbprint|在远程计算机运行此命令，才需要此参数。 **CertificateThumbprint**参数指定数字公钥证书 (X509) 的用户帐户有权目标计算机上运行该命令。|
|UseSSL|在远程计算机运行此命令，才需要此参数。 **UseSSL**参数指定用于建立连接到远程计算机的安全套接字层 (SSL) 协议。 默认情况下，无法使用 SSL。|

网络控制器应用程序的配置完成后，你的网络控制器部署已完成。

## <a name="bkmk_validation"></a>网络控制器部署验证

若要验证你的网络控制器部署，可以添加到网络控制器的凭据，然后检索凭据。

如果你要用作 Kerberos ClientAuthentication 机制，在会员**ClientSecurityGroup**创建了是最低要求执行此过程。

#### <a name="to-validate-deployment-of-network-controller"></a>若要验证部署网络控制器

1.  在客户端计算机上，如果你正在使用 Kerberos 作为 ClientAuthentication 机制，登录成员的用户帐户你**ClientSecurityGroup**。

2. 打开 Windows PowerShell 键入以下命令以添加到网络控制器的凭据，然后按 ENTER。 确保你添加适合你的部署的值为每个参数。

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. 检索你添加到网络控制器的凭据，键入以下命令，然后再次按 ENTER。 确保你添加适合你的部署的值为每个参数。

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. 查看命令输出，则应类似于以下示例输出。

    ```
    Tags                   :
    ResourceRef     : /credentials/cred1
    CreatedTime    : 1/1/0001 12:00:00 AM
    InstanceId        : e16ffe62-a701-4d31-915e-7234d4bc5a18
    Etag                  : W/"1ec59631-607f-4d3e-ac78-94b0822f3a9d"
    ResourceMetadata :
    ResourceId       : cred1
    Properties       : Microsoft.Windows.NetworkController.CredentialProperties
    ```

    > [!NOTE]
    > 运行时**获取 NetworkControllerCredential**命令时，你可以通过使用点运营商列表中的凭据属性将给变量命令输出。 例如，$cred。属性。

## <a name="bkmk_ps"></a>其他 Windows PowerShell 命令网络控制器

部署网络控制器后，你可以使用 Windows PowerShell 命令管理和修改部署。 以下是一些可使你的部署的更改。

- 修改了网络控制器节点、群集、并应用程序设置

- 删除网络控制器群集和应用程序

- 管理网络控制器群集节点，包括添加、删除、启用和禁用节点。

下表提供语法可用来完成这些任务的 Windows PowerShell 命令。

|任务|命令|语法|
|--------|-------|----------|
|修改了网络控制器群集设置|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改了网络控制器应用程序设置|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改了网络控制器节点设置|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改了网络控制器诊断设置|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|删除该网络控制器应用程序|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|删除网络控制器群集|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|添加到网络控制器群集节点|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|禁用网络控制器群集节点|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|启用网络控制器群集节点|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|删除从群集节点网络控制器|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>在 TechNet 库中的 Windows PowerShell 命令网络控制器是[网络控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。

## <a name="bkmk_script"></a>示例网络控制器配置脚本

下面的示例配置脚本显示创建多个节点网络控制器群集并安装网络控制器应用程序的方法。 此外，$cert 变量相匹配的对象名称字符串"networkController.contoso.com"本地计算机证书官方商城选择证书。

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="bkmk_nonkerb"></a>部署后步骤的非-Kerberos 部署

如果你未使用 Kerberos 与网络控制器部署，你必须部署证书。

有关详细信息，请参阅[网络控制器部署后步骤](../technologies/network-controller/post-deploy-steps-nc.md)。


