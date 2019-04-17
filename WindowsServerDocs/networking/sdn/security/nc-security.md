---
title: 网络控制器安全
description: 你可以使用本主题以了解如何配置所有之间进行通信网络控制器和其他软件和设备安全。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab04d3f528beb037988e6390fe85ad9af219266b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-security"></a>网络控制器安全

你可以使用本主题以了解如何配置所有之间进行通信网络控制器和其他软件和设备安全。 

>[!NOTE]
>网络控制器的概述，请参阅[网络控制器](../technologies/network-controller/network-controller.md)。

你可以在安全的通信路径包括上管理平面、网络控制器虚拟机 \(VMs\) 群集、中的和 Southbound 通信的数据板上之间的群集通信的 Northbound 通信。

1. **Northbound 通信**。 网络控制器上使用 Windows PowerShell 和 System Center 虚拟机 Manager \(SCVMM\) 等 SDN\ 支持管理软件管理平面进行通信。 这些管理工具为您提供的功能定义网络策略和以创建该网络，你可以对其进行比较实际网络配置为与目标状态奇偶校验到实际的配置目标状态。

2. **网络控制器群集通信**。 作为网络控制器群集节点配置三个或多个虚拟机的功能时，这些节点相互通信。 这种通信可能相关同步，并类数据的复制到，在多个节点或特定网络控制器服务之间的通信。

3.  **Southbound 通信**。 网络控制器数据平面 SDN 基础结构和等软件负载平衡、网关和主计算机的其他设备上进行通信。 你可以使用网络控制器配置和管理这些 southbound 设备，以使其保持的网络配置目标状态。

## <a name="northbound-communication"></a>Northbound 通信

网络控制器支持 Northbound 通信身份验证、授权和加密。 以下部分提供有关如何配置这些安全设置的信息。

**身份验证**

网络控制器 Northbound 通信的身份验证配置时，您可以允许网络控制器群集节点和管理客户，验证与他们进行通信的设备的身份。

网络控制器支持之间管理客户端和网络控制器节点身份验证以下三种模式。

>[!Note]
>如果你正在仅部署网络控制器的 System Center 虚拟机 Manager，**Kerberos**模式受支持。

1. **Kerberos**. 这两个管理客户，例如运行 SCVMM，并且所有网络控制器群集节点的计算机连接到 Active Directory 域中，使用用于身份验证的域帐户时，你可以使用 Kerberos 身份验证。

2. **X509**。 X509 是 certificate\ 基于身份验证。 你可以使用 X509 管理客户端无法连接到域的 Active Directory 时的身份验证。 若要使用 X509，必须注册证书为所有网络控制器群集节点和管理客户端和所有节点和管理客户必须都信任彼此的 ' 证书。

3. **无**。 当选择此模式时，没有管理客户端和网络控制器之间进行身份验证。 此模式仅用于测试目的，并建议不要生产环境中使用。 

你可以通过使用 Windows PowerShell 命令配置 Northbound 通信的身份验证模式**安装 NetworkController**与**ClientAuthentication**参数。 

有关详细信息，请参阅以下主题。 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**授权**

网络控制器 Northbound 通信的授权配置时，您可以允许网络控制器群集节点并验证它们进行通信的设备是受信任的并且有权参与通信管理客户端。

对于每个受网络控制器的身份验证模式中，使用以下授权方法。

1.  **Kerberos**. 当你使用的 Kerberos 身份验证方法时、定义的用户和有权通过 Active Directory 中, 创建安全组，然后将获得授权的用户和计算机添加到组与网络控制器通信的计算机。 您可以配置了网络控制器用于安全组授权使用**ClientSecurityGroup**参数**安装 NetworkController** Windows PowerShell 命令。 安装网络控制器后，你可以通过更改安全组[组 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)与参数命令**-ClientSecurityGroup**。 如果你使用的 SCVMM，必须期间部署作为参数提供安全的组。

2.  **X509**。 当您使用 X509 身份验证方法、网络控制器仅接受管理其证书指纹已知网络控制器的客户端的请求。 你可以通过使用配置这些指纹**ClientCertificateThumbprint**参数**安装 NetworkController** Windows PowerShell 命令。 你可以随时通过添加其他客户端指纹**组 NetworkController**命令。

3.  **无**。 当选择此模式时，可为管理客户端和网络控制器节点之间进行通信尝试执行的未授权。 此模式仅用于测试目的，并建议不要生产环境中使用。 

有关详细信息，请参阅以下主题。 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**加密**

Northbound 通信使用安全套接字层 \(SSL\) 创建加密的通道之间管理客户端和网络控制器节点。 对于 Northbound 通信 SSL 加密包括以下要求。

- 所有网络控制器节点都必须增强键使用量 \(EKU\) 扩展包含服务器身份验证和客户端身份验证的用途的同一个证书。 

- 管理客户端用于与网络控制器通信 URI 必须证书主题名称。 完全合格域 (FQDN) 或网络控制器其余端点的 IP 地址，则必须包含证书主题名称。

- 如果在不同的子网网络控制器节点，它们的证书的对象名称必须与你使用的值相同**RestName**中的参数**安装 NetworkController** Windows PowerShell 命令。 

- 必须所有管理客户端信任 SSL 证书。

### <a name="ssl-certificate-enrollment-and-configuration"></a>SSL 证书的注册和配置

你必须手动注册网络控制器节点上的 SSL 证书。

注册证书后，您可以配置网络控制器使用与证书**-服务器证书**参数**安装 NetworkController** Windows PowerShell 命令。 如果你已经安装了网络控制器，你可以使用更新随时配置**组 NetworkController**命令。

>[!NOTE]
>如果你使用的 SCVMM，你必须为库资源添加证书。 有关详细信息，请参阅[设置 VMM 结构中 SDN 网络控制器](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

## <a name="network-controller-cluster-communication"></a>网络控制器群集通信

网络控制器支持节点网络控制器之间进行通信的身份验证、授权和加密。 通信是通过[Windows 通信基础](https://msdn.microsoft.com/library/ms731082.aspx)\(WCF\) 和 TCP。

你还可以将使用此模式配置为**ClusterAuthentication**参数**安装 NetworkControllerCluster** Windows PowerShell 命令。 

有关详细信息，请参阅[安装 NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster)。

**身份验证**

网络控制器群集通信的身份验证配置时，您可以允许网络控制器群集节点以验证身份的其他节点与其它们进行通信。

网络控制器支持以下三种模式的网络控制器节点之间的身份验证。

>[!NOTE]
>如果你仅使用 SCVMM，部署网络控制器**Kerberos**模式受支持。

1. **Kerberos**. 所有网络控制器群集节点都连接到域的 Active Directory 中，使用用于身份验证的域帐户时，你可以使用 Kerberos 身份验证。

2. **X509**。 X509 是 certificate\ 基于身份验证。 你可以使用的 X509 身份验证来网络控制器群集节点未 Active Directory 域加入。 若要使用 X509，必须注册证书为所有网络控制器群集节点，并且所有节点必须都信任的证书。 此外，注册的每个节点证书的主题名称必须 DNS 名称节点的相同。

3. **无**。 当选择此模式时，没有节点网络控制器之间进行身份验证。 此模式仅用于测试目的，并建议不要生产环境中使用。

**授权**

网络控制器群集通信的授权配置时，您可以允许网络控制器群集节点验证与其它们进行通信的节点信任并有权参与通信。

对于每个受网络控制器的身份验证模式中，使用以下授权方法。

1. **Kerberos**. 网络控制器节点接受通信请求仅从网络控制器计算机的其他帐户。 使用部署网络控制器时，您可以配置这些帐户**名称**参数[新建 NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

2. **X509**。 网络控制器节点接受通信请求仅从网络控制器计算机的其他帐户。 使用部署网络控制器时，您可以配置这些帐户**名称**参数[新建 NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

3. **无**。 当选择此模式时，可执行网络控制器节点之间的未授权。 此模式仅用于测试目的，并建议不要生产环境中使用。

**加密**

网络控制器节点间通信进行加密使用 WCF 传输级别加密。 身份验证和授权的方法是 Kerberos 或 X509 时使用这种形式的加密证书。 有关详细信息，请参阅以下主题。

- [操作方法：安全 Windows 凭据与服务](https://msdn.microsoft.com/library/ms734673.aspx)
- [操作方法：安全证书 X.509 与服务](https://msdn.microsoft.com/library/ms788968.aspx)。

## <a name="southbound-communication"></a>Southbound 通信

网络控制器与不同类型的 Southbound 通信的设备进行交互。 这些交互使用不同的协议。 出于此原因，有不同的要求身份验证、授权和加密，具体取决于设备和网络控制器用于与设备通信的协议的类型。

下表提供了与不同 southbound 设备网络控制器互动交流的信息。

| 每个服务 southbound 设备 | 协议              | 使用的身份验证    |
|---------------------------|-----------------------|------------------------|
| 软件负载平衡    | WCF（输入混合），TCP（主机） | 证书           |
| 防火墙                  | OVSDB                 | 证书           |
| 网关                   | WinRM                 | Kerberos 证书 |
| 虚拟网络        | OVSDB WCF            | 证书           |
| 用户定义路由      | OVSDB                 | 证书           |

对于每个这些协议，通信机制是以下部分中所述的。

**身份验证**

对于 Southbound 通信，使用以下协议和身份验证方法。

1. **WCF/TCP/OVSDB**。 对于以下协议中，通过使用 X509 执行身份验证证书。 网络控制器和等软件负载平衡 \(SLB\) 复用器 \ (MUX\) / 主计算机展示了他们彼此的相互身份验证的证书。 每个证书必须受远程等。

    对于 southbound 身份验证，可用于配置相同 SSL 证书加密与 Northbound 客户通信。 您还必须 SLB 输入混合和主机设备配置证书。 证书主题名称必须与设备的 DNS 名称相同。

2. **WinRM**。 对于本协议，通过使用 Kerberos 执行身份验证 \（对于已加入域 machines\) 和由使用证书 \（适用于非域连接 machines\)。

**授权**

对于 Southbound 通信，使用以下协议和授权的方法。

1. **WCF/TCP**。 对于这些协议，授权基于等实体的对象名称。 网络控制器存储对等设备 DNS 名称，并使用它进行授权。 此 DNS 名称必须与主题证书中的设备的名称匹配。 同样，网络控制器证书必须匹配存储对等设备上的网络控制器 DNS 名称。

2. **WinRM**。 如果正在使用 Kerberos，WinRM 客户端帐户必须存在预定义的组 Active Directory 中或在服务器上的本地管理员组中。 如果正在使用证书，的客户端将提供服务器授权使用主题名称/发行商，，并服务器使用映射的用户帐户进行身份验证的证书。

3.  **OVSDB**。 很没有提供用于此协议的授权问题。

**加密**

对于 Southbound 通信，以下加密方法用于协议。

1. **WCF/TCP/OVSDB**。 对于这些协议的客户端或服务器上使用注册的证书执行加密。

2. **WinRM**。 WinRM 交通加密默认情况下使用 Kerberos 安全支持的提供商 \(SSP\)。 您可以配置 SSL、的形式 WinRM 服务器上的额外的加密。
