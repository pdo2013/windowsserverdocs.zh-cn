---
title: 使用 Windows PowerShell 部署网络控制器
description: 本主题说明了使用 Windows PowerShell 在一个或多个计算机或运行 Windows Server 2016 的虚拟机 (Vm) 上部署网络控制器。
manager: dougkim
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
ms.date: 08/23/2018
ms.openlocfilehash: d671d044896ae9e71edad8302f06f2a21fe50772
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034555"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>使用 Windows PowerShell 部署网络控制器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题说明了使用 Windows PowerShell 将一个或多个虚拟机 (Vm) 上运行 Windows Server 2016 中部署网络控制器。

>[!IMPORTANT]
>不要部署物理主机上的网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色上的 HYPER-V 虚拟机\(VM\)安装在 HYPER-V 主机上。 在三个不同超上的 Vm 上安装网络控制器后\-V 主机，必须启用超\-V 软件定义的网络的主机\(SDN\)通过添加到网络控制器使用的主机Windows PowerShell 命令**新建 NetworkControllerServer**。 这样，要启用 SDN 软件负载均衡器函数。 有关详细信息，请参阅[新建 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

本主题包含以下部分。

- [安装网络控制器服务器角色](#install-the-network-controller-server-role)

- [配置网络控制器群集](#configure-the-network-controller-cluster)

- [配置网络控制器应用程序](#configure-the-network-controller-application)

- [网络控制器部署验证](#network-controller-deployment-validation)

- [用于网络控制器的其他 Windows PowerShell 命令](#additional-windows-powershell-commands-for-network-controller)

- [示例网络控制器配置脚本](#sample-network-controller-configuration-script)

- [对于非 Kerberos 的部署的部署后步骤](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>安装网络控制器服务器角色

可以使用此过程的虚拟机上安装网络控制器服务器角色\(VM\)。

>[!IMPORTANT]
>不要部署物理主机上的网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色上的 HYPER-V 虚拟机\(VM\)安装在 HYPER-V 主机上。 在三个不同超上的 Vm 上安装网络控制器后\-V 主机，必须启用超\-V 软件定义的网络的主机\(SDN\)通过添加到网络控制器主机。 这样，要启用 SDN 软件负载均衡器函数。

Administrators  组成员或同等身份是执行此过程的最低要求。  

>[!NOTE]
>如果你想要使用服务器管理器而不是 Windows PowerShell 来安装网络控制器，请参阅[安装网络控制器服务器角色使用服务器管理器](https://technet.microsoft.com/library/mt403348.aspx)

若要使用 Windows PowerShell 安装网络控制器，请在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

网络控制器安装需要重新启动计算机。 为此，请键入以下命令，然后按 ENTER。

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>配置网络控制器群集

网络控制器群集提供高可用性和网络控制器应用程序，你可以创建群集后，配置和基础群集上托管的可伸缩性。

>[!NOTE]
>您可以在以下各节中或者直接在其中安装网络控制器，也可以使用远程服务器管理工具为 Windows Server 2016 要执行的过程从远程计算机正在运行的 VM 上执行的过程Windows Server 2016 或 Windows 10。 此外中的成员身份**管理员**，或等效身份是执行此过程所需的最小。 如果计算机或在其安装网络控制器 VM 已加入域，您的用户帐户必须隶属**域用户**。

创建节点对象，然后配置群集，可以创建网络控制器群集。

### <a name="create-a-node-object"></a>创建节点对象

您需要为每个 VM 网络控制器群集的成员创建节点对象。

若要创建的节点对象，在 Windows PowerShell 命令提示符下键入以下命令，然后按 ENTER。 请确保添加适合你的部署的每个参数的值。  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

下表提供的每个参数的说明**新建 NetworkControllerNodeObject**命令。

|参数|描述|
|-------------|---------------|
|名称|**名称**参数指定你想要添加到群集的服务器的友好名称|
|Server|**Server**参数指定的主机名、 完全限定域 (FQDN) 或你想要添加到群集的服务器的 IP 地址。 对于已加入域的计算机，FQDN 是必需的。|
|FaultDomain|**FaultDomain**参数指定要添加到群集的服务器的故障域。 此参数定义要添加到群集的服务器时可能会遇到故障的服务器。 此失败可能是由于共享的物理依赖项，如电源和网络源。 容错域通常代表这些共享依赖项，更多的服务器可能从容错域树中的较高点一起失败相关的层次结构。 在运行时，网络控制器会考虑群集中的容错域，并尝试分散的网络控制器服务，以便它们位于单独的容错域。 此过程有助于确保，如果失败的任何一个容错域，该服务和其状态的可用性不遭到破坏。 该层次结构格式指定容错域。 例如："Fd: / DC1/Rack1/Host1"，其中 DC1 是数据中心名称、 Rack1 是机架名称，Host1 是放置在节点的主机的名称。|
|RestInterface|**RestInterface**参数指定的终止的具象状态传输 (REST) 通信的节点上接口的名称。 此网络控制器接口接收来自网络的管理层 Northbound API 请求。|
|NodeCertificate|**NodeCertificate**参数指定网络控制器使用进行计算机身份验证的证书。 如果为群集内部的通信; 使用基于证书的身份验证，则需要证书该证书还用于网络控制器服务之间的流量进行加密。 证书使用者名称必须与该节点的 DNS 名称相同。|

### <a name="configure-the-cluster"></a>配置群集

若要配置群集，在 Windows PowerShell 命令提示符下键入以下命令，然后按 ENTER。 请确保添加适合你的部署的每个参数的值。

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

下表提供的每个参数的说明**安装 NetworkControllerCluster**命令。
  
|参数|描述|
|-------------|---------------|
|ClusterAuthentication|**ClusterAuthentication**参数指定用于保护节点之间的通信，还用于网络控制器服务之间的流量进行加密的身份验证类型。 支持的值为**Kerberos**， **X509**并**None**。 Kerberos 身份验证使用域帐户，以便仅用于网络控制器节点是否已加入域。 如果您指定基于 X509 的身份验证，则必须提供 NetworkControllerNode 对象中的证书。 此外，你必须手动设置证书，在运行此命令之前。|
|ManagementSecurityGroup|**ManagementSecurityGroup**参数指定包含允许从远程计算机运行的管理 cmdlet 的用户的安全组的名称。 这是仅适用于 ClusterAuthentication 是 Kerberos。 必须在本地计算机上指定的域安全组而不是安全组。|
|节点|**节点**参数指定的使用创建的网络控制器节点的列表**新建 NetworkControllerNodeObject**命令。|
|DiagnosticLogLocation|**DiagnosticLogLocation**参数指定的诊断日志定期上传的共享位置。 如果不指定此参数的值，日志每个节点上本地存储。 本地日志存储在文件夹 %systemdrive%\windows\tracing\sdndiagnostics。 群集日志本地存储在文件夹 %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces。|
|LogLocationCredential|**LogLocationCredential**参数指定所需的访问日志的存储位置的共享位置的凭据。|
|CredentialEncryptionCertificate|**CredentialEncryptionCertificate**参数指定网络控制器用来加密凭据用于访问网络控制器的二进制文件的证书和**LogLocationCredential**，如果指定。 该证书必须预配在所有网络控制器节点在运行此命令之前，必须在所有群集节点上都注册同一个证书。 在生产环境中建议使用此参数以保护网络控制器的二进制文件和日志。 如果没有此参数，凭据以明文形式存储，并且可能会被误用的任何未经授权的用户。|
|凭据|此参数是必需的仅当从远程计算机运行此命令。 **凭据**参数指定有权在目标计算机上运行此命令的用户帐户。|
|CertificateThumbprint|此参数是必需的仅当从远程计算机运行此命令。 **CertificateThumbprint**参数指定的数字公钥证书 (X509) 有权在目标计算机上运行此命令的用户帐户。|
|UseSSL|此参数是必需的仅当从远程计算机运行此命令。 **UseSSL**参数指定用于建立与远程计算机的连接的安全套接字层 (SSL) 协议。 默认情况下，不使用 SSL。|
|ComputerName|**ComputerName**参数指定此命令是运行在其的网络控制器节点。 如果不指定此参数的值，默认情况下使用本地计算机。|
|LogSizeLimitInMBs|此参数指定的最大日志大小，单位为 MB，可存储网络控制器。 日志存储在循环方式。 如果提供 DiagnosticLogLocation，则此参数的默认值为 40 GB。 如果未提供 DiagnosticLogLocation，日志存储在网络控制器节点上，此参数的默认值为 15 GB。|
|LogTimeLimitInDays|此参数指定的持续时间限制，以天为单位，这些日志存储。 日志存储在循环方式。 此参数的默认值为 3 天。|

## <a name="configure-the-network-controller-application"></a>配置网络控制器应用程序
若要配置网络控制器应用程序，在 Windows PowerShell 命令提示符下键入以下命令，然后按 ENTER。 请确保添加适合你的部署的每个参数的值。

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

下表提供的每个参数的说明**安装 NetworkController**命令。

|参数|描述|
|-------------|---------------|
|ClientAuthentication|**ClientAuthentication**参数指定用于保护 REST 与网络控制器之间的通信的身份验证类型。 支持的值为**Kerberos**， **X509**并**None**。 Kerberos 身份验证使用域帐户，以便仅用于网络控制器节点是否已加入域。 如果您指定基于 X509 的身份验证，则必须提供 NetworkControllerNode 对象中的证书。 此外，你必须手动设置证书，在运行此命令之前。|
|节点|**节点**参数指定的使用创建的网络控制器节点的列表**新建 NetworkControllerNodeObject**命令。|
|ClientCertificateThumbprint|仅当你使用基于证书的身份验证网络控制器客户端时，此参数是必需的。 **ClientCertificateThumbprint**参数指定注册到 Northbound 层上的客户端的证书的指纹。|
|ServerCertificate|**ServerCertificate**参数指定网络控制器用来向客户端证明其身份的证书。 服务器证书必须包含服务器身份验证目的在增强型密钥用法扩展中，并且必须由客户端信任的 CA 颁发给网络控制器。|
|RESTIPAddress|不需要指定的值**RESTIPAddress**与网络控制器的单节点部署。 对于多节点部署， **RESTIPAddress**参数指定的 REST 终结点的 IP 地址以 CIDR 表示法。 例如，192.168.1.10/24。 使用者名称值**ServerCertificate**必须解析为的值**RESTIPAddress**参数。 位于同一子网中的所有节点时，此参数必须指定所有多节点网络控制器部署。 如果节点位于不同子网，则必须使用**RestName**而不是使用参数**RESTIPAddress**。|
|RestName|不需要指定的值**RestName**与网络控制器的单节点部署。 时，才必须指定的值**RestName**时多节点部署有位于不同子网上的节点。 对于多节点部署， **RestName**参数指定网络控制器群集的 FQDN。|
|ClientSecurityGroup|**ClientSecurityGroup**参数指定的成员是网络控制器客户端的 Active Directory 安全组的名称。 此参数是必需的仅当使用 Kerberos 身份验证**ClientAuthentication**。 安全组必须包含的帐户从其访问的 REST Api，并且必须创建安全组，并运行此命令之前添加成员。|
|凭据|此参数是必需的仅当从远程计算机运行此命令。 **凭据**参数指定有权在目标计算机上运行此命令的用户帐户。|
|CertificateThumbprint|此参数是必需的仅当从远程计算机运行此命令。 **CertificateThumbprint**参数指定的数字公钥证书 (X509) 有权在目标计算机上运行此命令的用户帐户。|
|UseSSL|此参数是必需的仅当从远程计算机运行此命令。 **UseSSL**参数指定用于建立与远程计算机的连接的安全套接字层 (SSL) 协议。 默认情况下，不使用 SSL。|

完成网络控制器应用程序的配置后，你的网络控制器的部署已完成。

## <a name="network-controller-deployment-validation"></a>网络控制器部署验证

若要验证网络控制器部署，您可以将凭据添加到网络控制器，然后检索凭据。

如果使用 Kerberos 作为 ClientAuthentication 机制中的成员身份**ClientSecurityGroup**是执行此过程所需的最低要求你创建。

**过程：**

1.  在客户端计算机上，如果您使用 Kerberos 作为 ClientAuthentication 机制，使用登录的成员的用户帐户在**ClientSecurityGroup**。

2. 打开 Windows PowerShell，键入以下命令以将凭据添加到网络控制器，然后按 ENTER。 请确保添加适合你的部署的每个参数的值。

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. 若要检索添加到网络控制器的凭据，请键入以下命令，然后按 ENTER。 请确保添加适合你的部署的每个参数的值。

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. 查看命令输出中，这应类似于以下示例输出。

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
    > 在运行时**Get NetworkControllerCredential**命令时，您可以将命令的输出分配给变量中使用点运算符以列出所提供的凭据属性。 例如，$cred。属性。

## <a name="additional-windows-powershell-commands-for-network-controller"></a>用于网络控制器的其他 Windows PowerShell 命令

部署网络控制器后，可以使用 Windows PowerShell 命令来管理和修改你的部署。 以下是一些你可以对你的部署进行的更改。

- 修改网络控制器节点、 群集和应用程序设置

- 删除网络控制器群集和应用程序

- 管理网络控制器群集节点，包括添加、 删除、 启用和禁用节点。

下表提供了适用于 Windows PowerShell 语法的命令可用于完成这些任务。

|任务|Command|语法|
|--------|-------|----------|
|修改网络控制器群集设置|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改网络控制器应用程序设置|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改网络控制器节点设置|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改网络控制器诊断设置|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|删除网络控制器应用程序|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|删除网络控制器群集|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|将节点添加到网络控制器群集|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|禁用网络控制器群集节点|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|启用网络控制器群集节点|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|从群集中删除网络控制器节点|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>为网络控制器的 Windows PowerShell 命令会在 TechNet 库中[网络控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。

## <a name="sample-network-controller-configuration-script"></a>示例网络控制器配置脚本

下面的示例配置脚本演示如何创建多节点网络控制器群集并安装网络控制器应用程序。 此外，$cert 变量相匹配的使用者名称字符串"networkController.contoso.com"的本地计算机证书存储中选择一个证书。

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>对于非 Kerberos 的部署的部署后步骤

如果未与网络控制器部署使用 Kerberos，则必须部署证书。

有关详细信息，请参阅[的网络控制器部署后步骤](../technologies/network-controller/post-deploy-steps-nc.md)。


