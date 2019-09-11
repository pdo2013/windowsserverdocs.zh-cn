---
title: 网络控制器安全
description: 你可以使用本主题来了解如何配置网络控制器与其他软件和设备之间所有通信的安全性。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: cea660eb28645fb814d718ac04d0c9acea7b2e34
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867135"
---
# <a name="secure-the-network-controller"></a>保护网络控制器

本主题介绍如何为[网络控制器](../technologies/network-controller/network-controller.md)与其他软件与设备之间的所有通信配置安全性。 

您可以保护的通信路径包括：管理平面上的 Northbound 通信、群集中网络控制器虚拟机\(vm\)之间的群集通信以及数据上的 Southbound 通信平面.

1. **Northbound 通信**。 网络控制器在具有 SDN\-功能的管理软件（如 Windows PowerShell 和 System Center Virtual Machine Manager \(SCVMM\)）上进行通信。 这些管理工具为你提供了定义网络策略并为网络创建目标状态的功能，你可以通过这些管理工具比较实际的网络配置，使实际配置与目标状态保持一致。

2. **网络控制器群集通信**。 如果将三个或更多 Vm 配置为网络控制器群集节点，则这些节点彼此通信。 这种通信可能与跨节点同步和复制数据或网络控制器服务之间的特定通信有关。

3.  **Southbound 通信**。 网络控制器通过 SDN 基础结构和其他设备（如软件负载平衡器、网关和主机计算机）与数据平面通信。 你可以使用网络控制器来配置和管理这些 southbound 设备，使其保持你为网络配置的目标状态。


## <a name="northbound-communication"></a>Northbound 通信

网络控制器支持对 Northbound 通信进行身份验证、授权和加密。 以下各节提供了有关如何配置这些安全设置的信息。

### <a name="authentication"></a>身份验证

当你为网络控制器 Northbound 通信配置身份验证时，你允许网络控制器群集节点和管理客户端验证与其通信的设备的标识。

网络控制器在管理客户端和网络控制器节点之间支持以下三种身份验证模式。

>[!Note]
>如果要部署 System Center Virtual Machine Manager 的网络控制器，则仅支持**Kerberos**模式。

1. **Kerberos**。 将管理客户端和所有网络控制器群集节点加入到 Active Directory 域时，请使用 Kerberos 身份验证。 Active Directory 域必须具有用于身份验证的域帐户。

2. **X509**。 对于未加入到\-Active Directory 域的管理客户端，使用 X509 进行基于证书的身份验证。 必须将证书注册到所有网络控制器群集节点和管理客户端。 此外，所有节点和管理客户端必须信任彼此的证书。

3. **无**。 在测试环境中使用 "无" 进行测试，因此不建议在生产环境中使用。 选择此模式时，不会在节点和管理客户端之间执行身份验证。

可以通过使用带有_ClientAuthentication_参数的 Windows PowerShell 命令 **[NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** 来配置 Northbound 通信的身份验证模式。 


### <a name="authorization"></a>Authorization

当你为网络控制器 Northbound 通信配置授权时，你允许网络控制器群集节点和管理客户端验证与其通信的设备是否受信任，以及是否有权参与通讯.

对于网络控制器支持的每种身份验证模式，请使用以下授权方法。

1.  **Kerberos**。 使用 Kerberos 身份验证方法时，可以通过在 Active Directory 中创建一个安全组，然后将授权的用户和计算机添加到该组，来定义有权与网络控制器进行通信的用户和计算机。 你可以通过使用 **[NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Windows PowerShell 命令的_ClientSecurityGroup_参数将网络控制器配置为使用用于授权的安全组。 安装网络控制器后，可以通过将 **[NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令与参数 _-ClientSecurityGroup_一起使用来更改安全组。 如果使用 SCVMM，则必须在部署过程中将安全组作为参数提供。

2.  **X509**。 当你使用 X509 身份验证方法时，网络控制器仅接受来自客户端的证书指纹对网络控制器的请求的请求。 可以通过使用 **[NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Windows PowerShell 命令的_ClientCertificateThumbprint_参数来配置这些指纹。 可以通过使用 **[NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令随时添加其他客户端指纹。

3.  **无**。 选择此模式时，不会在节点和管理客户端之间执行身份验证。 在测试环境中使用 "无" 进行测试，因此不建议在生产环境中使用。 


### <a name="encryption"></a>加密

Northbound 通信使用安全套接字层\(SSL\)在管理客户端和网络控制器节点之间创建加密通道。 Northbound 通信的 SSL 加密包括以下要求：

- 所有网络控制器节点都必须具有相同的证书，该证书包括服务器身份验证和客户端身份验证\(目的\) 。 

- 管理客户端用于与网络控制器通信的 URI 必须是证书使用者名称。 证书使用者名称必须包含完全限定的域名（FQDN）或网络控制器 REST 终结点的 IP 地址。

- 如果网络控制器节点在不同的子网上，则其证书的使用者名称必须与**NetworkController** Windows PowerShell 命令中用于_RestName_参数的值相同。 

- 所有管理客户端必须信任 SSL 证书。


### <a name="ssl-certificate-enrollment-and-configuration"></a>SSL 证书注册和配置

你必须在网络控制器节点上手动注册 SSL 证书。

注册证书后，可以将网络控制器配置为使用带有**NetworkController** Windows PowerShell 命令的 **-ServerCertificate**参数的证书。 如果你已经安装了网络控制器，则可以使用**NetworkController**命令随时更新配置。

>[!NOTE]
>如果你使用的是 SCVMM，则必须将该证书添加为库资源。 有关详细信息，请参阅[在 VMM 构造中设置 SDN 网络控制器](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

## <a name="network-controller-cluster-communication"></a>网络控制器群集通信

网络控制器支持对网络控制器节点之间的通信进行身份验证、授权和加密。 通信[Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\)和 TCP 之间的通信。

可以通过**NetworkControllerCluster** Windows PowerShell 命令的**ClusterAuthentication**参数配置此模式。 

有关详细信息，请参阅[NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster)。

### <a name="authentication"></a>身份验证

配置网络控制器群集通信的身份验证时，允许网络控制器群集节点验证与其通信的其他节点的标识。

网络控制器在网络控制器节点之间支持以下三种身份验证模式。

>[!NOTE]
>如果使用 SCVMM 部署网络控制器，则仅支持**Kerberos**模式。

1. **Kerberos**。 如果所有网络控制器群集节点都加入到 Active Directory 域中，并且域帐户用于身份验证，则可以使用 Kerberos 身份验证。

2. **X509**。 X509 是基于\-证书的身份验证。 如果网络控制器群集节点未加入 Active Directory 域，则可以使用 X509 身份验证。 若要使用 X509，你必须将证书注册到所有网络控制器群集节点，并且所有节点必须信任证书。 此外，在每个节点上注册的证书的使用者名称必须与节点的 DNS 名称相同。

3. **无**。 选择此模式时，不会在网络控制器节点之间执行身份验证。 此模式仅用于测试目的，不建议在生产环境中使用。

### <a name="authorization"></a>Authorization

为网络控制器群集通信配置授权时，允许网络控制器群集节点验证与其通信的节点是否受信任，以及是否有权参与通信。

对于网络控制器支持的每种身份验证模式，使用以下授权方法。

1. **Kerberos**。 网络控制器节点只接受来自其他网络控制器计算机帐户的通信请求。 使用[NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令的**Name**参数部署网络控制器时，可以配置这些帐户。

2. **X509**。 网络控制器节点只接受来自其他网络控制器计算机帐户的通信请求。 使用[NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令的**Name**参数部署网络控制器时，可以配置这些帐户。

3. **无**。 选择此模式时，不会在网络控制器节点之间执行任何授权。 此模式仅用于测试目的，不建议在生产环境中使用。

### <a name="encryption"></a>加密

网络控制器节点之间的通信使用 WCF 传输级别加密进行加密。 当身份验证和授权方法为 Kerberos 或 X509 证书时，将使用这种加密形式。 有关详细信息，请参阅以下主题。

- [如何：使用 Windows 凭据保护服务](https://msdn.microsoft.com/library/ms734673.aspx)
- [如何：使用 x.509 证书](https://msdn.microsoft.com/library/ms788968.aspx)保护服务。

## <a name="southbound-communication"></a>Southbound 通信

网络控制器与不同类型的设备交互，实现 Southbound 通信。 这些交互使用不同的协议。 因此，身份验证、授权和加密要求不同，具体取决于网络控制器与设备通信时所使用的设备和协议类型。

下表提供有关与不同 southbound 设备的网络控制器交互的信息。

| Southbound 设备/服务 | Protocol              | 使用的身份验证    |
|---------------------------|-----------------------|------------------------|
| 软件负载平衡器    | WCF （MUX），TCP （主机） | 证书           |
| 防火墙                  | OVSDB                 | 证书           |
| 网关                   | WinRM                 | Kerberos，证书 |
| 虚拟网络        | OVSDB，WCF            | 证书           |
| 用户定义的路由      | OVSDB                 | 证书           |

对于上述每种协议，以下部分介绍了通信机制。

### <a name="authentication"></a>身份验证

对于 Southbound 通信，使用以下协议和身份验证方法。

1. **WCF/TCP/OVSDB**。 对于这些协议，将使用 X509 证书执行身份验证。 网络控制器和对等软件负载\(平衡 SLB\)多路复\(用\)器 MUX/host 计算机将各自的证书提供给相互身份验证。 每个证书都必须受远程对等方信任。

    对于 southbound 身份验证，你可以使用配置用于对与 Northbound 客户端的通信进行加密的相同 SSL 证书。 还必须在 SLB MUX 和主机设备上配置证书。 证书使用者名称必须与设备的 DNS 名称相同。

2. **WinRM**。 对于此协议， \(通过将 Kerberos 用于加入域的计算机\) ，并使用未加入域\(的计算机\)的证书来执行身份验证。

### <a name="authorization"></a>Authorization

对于 Southbound 通信，使用以下协议和授权方法。

1. **WCF/TCP**。 对于这些协议，授权基于对等实体的使用者名称。 网络控制器存储对等设备的 DNS 名称，并使用它进行授权。 此 DNS 名称必须与证书中设备的使用者名称匹配。 同样，网络控制器证书必须与对等设备上存储的网络控制器 DNS 名称匹配。

2. **WinRM**。 如果正在使用 Kerberos，则 WinRM 客户端帐户必须存在于 Active Directory 中的预定义组或服务器上的本地管理员组中。 如果正在使用证书，则客户端向服务器提供证书，服务器使用使用者名称/颁发者向服务器提供证书，而服务器使用映射的用户帐户执行身份验证。

3.  **OVSDB**。 没有为此协议提供授权。

### <a name="encryption"></a>加密

对于 Southbound 通信，会将以下加密方法用于协议。

1. **WCF/TCP/OVSDB**。 对于这些协议，使用在客户端或服务器上注册的证书执行加密。

2. **WinRM**。 默认情况下，使用 Kerberos 安全支持提供程序\(SSP\)对 WinRM 通信进行加密。 你可以在 WinRM 服务器上以 SSL 的形式配置其他加密。
