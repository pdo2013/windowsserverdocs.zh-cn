---
title: 网络控制器安全
description: 可以使用本主题，了解如何配置网络控制器和其他软件和设备之间的所有通信的安全性。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: fccd6ee1ba734d72629264bd5b3bb7d93ddcdb70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884758"
---
# <a name="secure-the-network-controller"></a>保护网络控制器

在本主题中，了解如何配置之间的所有通信的安全性[网络控制器](../technologies/network-controller/network-controller.md)和其他软件和设备。 

可以保护这些通信路径中包含了管理平面，网络控制器虚拟机之间的群集通信南向通信\(Vm\)的群集，和对数据进行北向通信平面。

1. **南向通信**。 网络控制器进行通信，在使用 SDN 管理平面\-支持的管理软件，如 Windows PowerShell 和 System Center Virtual Machine Manager \(SCVMM\)。 这些管理工具为您提供的功能来定义网络策略并创建的网络，可以根据其比较实际的网络配置，以使实际配置为与目标状态的奇偶校验的目标状态。

2. **网络控制器群集通信**。 在三个或多个 Vm 配置为网络控制器的群集节点时，这些节点相互通信。 此通信可能与同步和复制数据的节点，或特定网络控制器服务之间的通信。

3.  **进行北向通信**。 网络控制器在 SDN 基础结构和软件负载均衡器、 网关和主机计算机之类的其他设备的数据平面上通信。 可以使用网络控制器配置和管理这些 southbound 设备，以便它们维护的网络配置的目标状态。


## <a name="northbound-communication"></a>南向通信

网络控制器 Northbound 通信支持身份验证、 授权和加密。 以下各节提供有关如何配置这些安全设置的信息。

### <a name="authentication"></a>身份验证

在配置网络控制器 Northbound 通信的身份验证时，您允许群集节点网络控制器和管理客户端验证的设备与之通信的身份。

网络控制器支持以下三种管理客户端和网络控制器节点之间的身份验证模式。

>[!Note]
>如果您要部署网络控制器使用 System Center Virtual Machine Manager 中，仅**Kerberos**支持模式。

1. **Kerberos**。 管理客户端和网络控制器的所有群集节点加入 Active Directory 域时，请使用 Kerberos 身份验证。 Active Directory 域必须具有域帐户用于进行身份验证。

2. **X509**。 使用 X509 证书\-基于管理客户端未加入到 Active Directory 域的身份验证。 必须注册到所有群集节点网络控制器和管理客户端证书。 此外，所有节点和管理客户端必须都信任对方的证书。

3. **无**。 在生产环境中使用的使用都不用于测试目的在测试环境中，因此，不推荐使用。 当你选择此模式下时，没有节点管理客户端之间进行身份验证。

可以使用 Windows PowerShell 命令来配置南向通信的身份验证模式**[安装 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** 与_ClientAuthentication_参数。 


### <a name="authorization"></a>授权

在配置网络控制器 Northbound 通信的授权时，允许群集节点网络控制器和管理客户端验证与之通信的设备是受信任且有权参与通信。

为每个由网络控制器支持的身份验证模式使用以下授权方法。

1.  **Kerberos**。 当使用 Kerberos 身份验证方法时，您定义的用户和计算机有权通过在 Active Directory 中创建安全组，然后将经过授权的用户和计算机添加到组与网络控制器进行通信。 可以配置要使用的安全组用于授权的网络控制器_ClientSecurityGroup_的参数**[安装 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Windows PowerShell 命令。 在安装后网络控制器，你可以安全组使用**[集 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令和参数 _-ClientSecurityGroup_. 如果使用 SCVMM，则必须在部署期间作为参数提供的安全组。

2.  **X509**。 使用 X509 时身份验证方法，网络控制器只接受来自其证书指纹已知到网络控制器管理客户端请求。 可以通过配置这些指纹_ClientCertificateThumbprint_的参数**[安装 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)**  Windows PowerShell 命令。 可以通过使用在任何时候添加其他客户端指纹**[集 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令。

3.  **无**。 当你选择此模式下时，没有节点管理客户端之间进行身份验证。 在生产环境中使用的使用都不用于测试目的在测试环境中，因此，不推荐使用。 


### <a name="encryption"></a>加密

南向通信使用安全套接字层\(SSL\)创建管理客户端和网络控制器节点之间的加密的通道。 南向通信的 SSL 加密包括以下要求：

- 网络控制器的所有节点必须都具有的相同证书，包括增强型密钥用法中的服务器身份验证和客户端身份验证目的\(EKU\)扩展。 

- 管理客户端用来与网络控制器进行通信的 URI 必须是证书使用者名称。 证书使用者名称必须包含完全限定域名 (FQDN) 或网络控制器 REST 终结点的 IP 地址。

- 如果网络控制器节点位于不同子网，其证书的使用者名称必须与使用的值相同_RestName_中的参数**安装 NetworkController** WindowsPowerShell 命令。 

- 所有管理客户端必须信任的 SSL 证书。


### <a name="ssl-certificate-enrollment-and-configuration"></a>SSL 证书注册和配置

你必须手动注册网络控制器节点上的 SSL 证书。

注册证书后，可以配置要使用的证书的网络控制器 **-ServerCertificate**的参数**安装 NetworkController** Windows PowerShell 命令. 如果你安装了网络控制器，可以通过使用更新的配置在任何时间**集 NetworkController**命令。

>[!NOTE]
>如果使用的 SCVMM，必须将证书添加为库资源。 有关详细信息，请参阅[SDN 网络控制器在 VMM 构造中设置](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

## <a name="network-controller-cluster-communication"></a>网络控制器的群集通信

网络控制器支持网络控制器节点间通信的身份验证、 授权和加密。 通信是通过[Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\)和 TCP。

你可以配置与此模式下**ClusterAuthentication**的参数**安装 NetworkControllerCluster** Windows PowerShell 命令。 

有关详细信息，请参阅[安装 NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster)。

### <a name="authentication"></a>身份验证

在配置网络控制器群集通信的身份验证时，允许网络控制器群集节点，以验证与之通信的其他节点的标识。

网络控制器支持以下三种网络控制器节点之间的身份验证模式。

>[!NOTE]
>仅使用 SCVMM，如果部署网络控制器**Kerberos**支持模式。

1. **Kerberos**。 网络控制器的所有群集节点都加入到 Active Directory 域，使用域帐户用于进行身份验证时，可以使用 Kerberos 身份验证。

2. **X509**。 X509 是证书\-基于身份验证。 可以使用的 X509 身份验证时网络控制器群集节点未加入到 Active Directory 域。 若要使用 X509，必须注册到所有网络控制器群集节点的证书，并且所有节点必须都信任证书。 此外，每个节点注册证书的使用者名称必须与该节点的 DNS 名称相同。

3. **无**。 当你选择此模式下时，没有网络控制器节点之间执行身份验证。 此模式仅用于测试目的，提供，不建议在生产环境中使用。

### <a name="authorization"></a>授权

在配置网络控制器群集通信的授权时，允许网络控制器群集节点，以验证与之通信的节点是受信任，并有权参与通信。

对于每个由网络控制器支持的身份验证模式，使用以下授权方法。

1. **Kerberos**。 网络控制器节点接受仅来自其他网络控制器计算机帐户的通信请求。 你可以配置这些帐户时使用部署网络控制器**名称**的参数[新建 NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

2. **X509**。 网络控制器节点接受仅来自其他网络控制器计算机帐户的通信请求。 你可以配置这些帐户时使用部署网络控制器**名称**的参数[新建 NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

3. **无**。 当你选择此模式下时，没有网络控制器节点之间执行未授权。 此模式仅用于测试目的，提供，不建议在生产环境中使用。

### <a name="encryption"></a>加密

网络控制器节点之间的通信使用 WCF 传输级加密进行加密。 这种形式的加密在身份验证和授权方法使用 Kerberos 或 X509 证书。 有关详细信息，请参阅以下主题。

- [如何：使用 Windows 凭据保护服务](https://msdn.microsoft.com/library/ms734673.aspx)
- [如何：使用 X.509 证书保护服务](https://msdn.microsoft.com/library/ms788968.aspx)。

## <a name="southbound-communication"></a>进行北向通信

网络控制器与不同类型的进行北向通信的设备进行交互。 这些交互使用不同的协议。 因此，有不同的要求身份验证、 授权和加密，具体取决于设备和网络控制器用来与设备通信协议的类型。

下表提供了有关不同 southbound 设备与网络控制器交互的信息。

| Southbound 设备/服务 | 协议              | 使用身份验证    |
|---------------------------|-----------------------|------------------------|
| 软件负载平衡器    | WCF (MUX) TCP （主机） | 证书           |
| 防火墙                  | OVSDB                 | 证书           |
| 网关                   | WinRM                 | Kerberos 证书 |
| 虚拟网络        | OVSDB WCF            | 证书           |
| 用户定义的路由      | OVSDB                 | 证书           |

对于每个这些协议，以下部分中描述的通信机制。

### <a name="authentication"></a>身份验证

对于进行北向通信，使用以下协议和身份验证方法。

1. **WCF/TCP/OVSDB**。 对这些协议执行身份验证通过使用 X509 证书。 网络控制器和软件负载平衡的对等方\(SLB\)复用器\(MUX \) /主机提供他们彼此相互身份验证的证书。 每个证书必须受远程对等方。

    对于 southbound 身份验证，可以使用相同的配置的 SSL 证书，与 Northbound 客户端的通信进行加密。 此外必须在 SLB MUX 和主机设备上配置证书。 证书使用者名称必须与设备的 DNS 名称相同。

2. **WinRM**。 通过使用 Kerberos 对此协议执行身份验证\(对于加入域的计算机\)并通过使用证书\(非域已加入计算机\)。

### <a name="authorization"></a>授权

对于进行北向通信，使用以下协议和授权方法。

1. **WCF/TCP**。 对于这些协议授权基于对等实体的使用者名称。 网络控制器存储的对等设备 DNS 名称，并使用进行授权。 此 DNS 名称必须与证书中设备的使用者名称匹配。 同样，网络控制器证书必须存储在对等设备上的网络控制器 DNS 名称匹配。

2. **WinRM**。 如果正在使用 Kerberos，WinRM 客户端帐户必须位于 Active Directory 中或在服务器上的本地管理员组中的预定义组。 如果要使用的证书，客户端提供到服务器的服务器授权使用使用者名称/颁发者，且服务器使用映射的用户帐户来执行身份验证的证书。

3.  **OVSDB**。 不没有为此协议提供任何授权。

### <a name="encryption"></a>加密

为进行北向通信的以下加密方法用于协议。

1. **WCF/TCP/OVSDB**。 对于这些协议，使用的证书来注册客户端或服务器上执行加密。

2. **WinRM**。 使用 Kerberos 安全支持提供程序的默认情况下加密 WinRM 通信\(SSP\)。 WinRM 服务器上的 SSL，窗体中，可以配置其他加密。
