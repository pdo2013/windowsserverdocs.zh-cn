---
title: 使用扩展端口访问控制列表创建安全策略
description: 本主题提供有关扩展端口访问控制列表 (Acl) Windows Server 2016 中的信息。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-hv-switch
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e61c3-f7d4-4e42-8575-79d75d05a218
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d847213f0332b57ae38ada444d7a6cd98ab325ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848978"
---
# <a name="create-security-policies-with-extended-port-access-control-lists"></a>使用扩展端口访问控制列表创建安全策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题提供有关扩展端口访问控制列表 (Acl) Windows Server 2016 中的信息。 你可以在 Hyper-V 虚拟交换机上配置扩展 ACL，以允许和阻止传往及传自通过虚拟网络适配器连接到交换机的虚拟机 (VM) 的网络流量。  
  
本主题包含以下部分。  
  
-   [详细的 ACL 规则](#bkmk_detailed)  
  
-   [有状态 ACL 规则](#bkmk_stateful)  
  
## <a name="bkmk_detailed"></a>详细的 ACL 规则  
HYPER-V 虚拟交换机扩展 Acl 允许你创建可应用于单个 VM 网络适配器连接到 HYPER-V 虚拟交换机的详细的规则。 创建详细的规则的功能允许企业和云服务提供商 (Csp) 地址在多租户共享的服务器环境中的基于网络的安全威胁。  
  
现在，借助扩展 ACL，你无需创建各种规则来阻止或允许各协议与某个 VM 之间相互传递的所有流量，就可以阻止或允许 VM 上运行的单个协议的网络流量。 可以在 Windows Server 2016 中包含下列 5 元组参数集的创建扩展的 ACL 规则： 源 IP 地址、 目标 IP 地址、 协议、 源端口和目标端口。 此外，每个规则都可以指定网络流量方向（入站或出站），以及该规则支持的操作（阻止或允许流量）。  
  
例如，你可以针对某个 VM 配置端口 ACL，以允许端口 80 上所有传入和传出的 HTTP 与 HTTPS 流量，同时，阻止所有端口上所有其他协议的网络流量。  
  
此功能用于指定租户 VM 可以或不可以接收的协议流量，当你配置安全策略时，它能够提供灵活性。  
  
### <a name="configuring-acl-rules-with-windows-powershell"></a>使用 Windows PowerShell 配置 ACL 规则  
若要配置扩展 ACL，必须使用 Windows PowerShell 命令 **Add-VMNetworkAdapterExtendedAcl**。 此命令具有四种不同的语法，每种语法的用途都各不相同：  
  
1.  将扩展的 ACL 添加到的所有命名的 VM-由第一个参数-VMName 指定网络适配器。 语法：  
  
    > [!NOTE]  
    > 如果你想要将扩展的 ACL 添加到一个网络适配器，而不是所有，可以使用参数指定的网络适配器-VMNetworkAdapterName。  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMName] <string[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny}  
        [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
2.  将扩展 ACL 添加到特定 VM 上的特定虚拟网络适配器。 语法：  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMNetworkAdapter] <VMNetworkAdapterBase[]> [-Action]  
        <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound |  
        Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort]  
        <string>] [[-Protocol] <string>] [-Weight] <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID  
        <int>] [-Passthru] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
3.  将扩展 ACL 添加到保留给 Hyper-V 主机管理操作系统使用的所有虚拟网络适配器。  
  
    > [!NOTE]  
    > 如果你想要将扩展的 ACL 添加到一个网络适配器，而不是所有，可以使用参数指定的网络适配器-VMNetworkAdapterName。  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction]  
        <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress]  
        <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight] <int> -ManagementOS  
        [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName <string>]  
        [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
4.  将扩展的 ACL 添加到已在 Windows PowerShell，例如创建一个 VM 对象 **$vm = 获取 vm"my_vm"**。 在下一个代码行中，你可以使用以下语法运行此命令，以创建扩展 ACL：  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VM] <VirtualMachine[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow |  
        Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
### <a name="detailed-acl-rule-examples"></a>详细 ACL 规则示例  
以下几个示例说明了如何使用 **Add-VMNetworkAdapterExtendedAcl** 命令来配置扩展端口 ACL 及创建 VM 的安全策略。  
  
-   [强制实施应用程序级安全](#bkmk_enforce)  
  
-   [强制实施用户级和应用程序级别的安全性](#bkmk_both)  
  
-   [提供到 TCP/UDP 应用程序的安全支持](#bkmk_tcp)  
  
> [!NOTE]  
> 下列表格中规则参数“方向”的值基于传往或传自你正在为其创建规则的 VM 的流量。 如果该 VM 正在接收流量，则流量为入站流量；如果该 VM 正在发送流量，则流量为出站流量。 例如，如果向 VM 应用一个阻止入站流量的规则，则入站流量的方向是从外部资源到 VM。 如果应用一个阻止出站流量的规则，则出站流量的方向是从本地 VM 到外部资源。  
  
### <a name="bkmk_enforce"></a>强制实施应用程序级安全  
由于许多应用程序服务器使用标准化 TCP/UDP 端口来与客户端计算机通信，因此，可以通过筛选传往和传自指定给应用程序的端口的流量来方便地创建规则，用于阻止或允许对应用程序服务器的访问。  
  
例如，你可能想要允许用户使用远程桌面连接 (RDP) 登录数据中心内的应用程序服务器。 由于 RDP 使用 TCP 端口 3389，因此，你可以快速设置以下规则：  
  
|源 IP|目标 IP|协议|源端口|目标端口|Direction|操作|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|TCP|*|3389|放大|允许|  
  
下面两个示例说明了如何使用 Windows PowerShell 命令创建规则。 第一个示例规则将阻止所有流量引导至 VM 名为"ApplicationServer"。 第二个示例规则，此功能应用于名为"ApplicationServer"的 VM 的网络适配器，只允许入站的 RDP 流量到 VM。  
  
> [!NOTE]  
> 在创建规则时，可以使用 **-权重**参数，以确定在其中的 HYPER-V 虚拟交换机处理规则的顺序。 值为 **-权重**表示为整数; 整数较小的规则之前处理整数较大的规则。 例如，如果你向 VM 网络适配器应用了两个规则，其中一个规则的权重为 1，另一个规则的权重为 10，则先应用权重为 10 的规则。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Inbound" -LocalPort 3389 -Protocol "TCP" -Weight 10  
```  
  
### <a name="bkmk_both"></a>强制实施用户级和应用程序级别的安全性  
由于规则能够与 5 元组 IP 数据包（源 IP、目标 IP、协议、源端口和目标端口）匹配，因此，与端口 ACL 相比，该规则可以强制更详细的安全策略。  
  
例如，如果你想要使用一组特定的 DHCP 服务器的计算机提供有限数量的客户端的 DHCP 服务，你可以运行 HYPER-V，托管用户 Vm 的 Windows Server 2016 计算机上配置以下规则：  
  
|源 IP|目标 IP|协议|源端口|目标端口|Direction|操作|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|255.255.255.255|UDP|*|67|缩小|允许|  
|*|10.175.124.0/25|UDP|*|67|缩小|允许|  
|10.175.124.0/25|*|UDP|*|68|放大|允许|  
  
以下示例说明了如何使用 Windows PowerShell 命令创建这些规则。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 255.255.255.255 -RemotePort 67 -Protocol "UDP"-Weight 10  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 67 -Protocol "UDP"-Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 68 -Protocol "UDP"-Weight 20  
```  
  
### <a name="bkmk_tcp"></a>提供到 TCP/UDP 应用程序的安全支持  
尽管数据中心内的大多数网络流量都使用 TCP 和 UDP，但仍有一些流量使用其他协议。 例如，如果你想要允许一组服务器运行依赖于 Internet 组管理协议 (IGMP) 的 IP 多播应用程序，则可以创建以下规则。  
  
> [!NOTE]  
> IGMP 具有一个指定的 IP 协议编号 0x02。  
  
|源 IP|目标 IP|协议|源端口|目标端口|Direction|操作|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|0x02|*|*|放大|允许|  
|*|*|0x02|*|*|缩小|允许|  
  
以下示例说明了如何使用 Windows PowerShell 命令创建这些规则。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -Protocol 2 -Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -Protocol 2 -Weight 20  
```  
  
## <a name="bkmk_stateful"></a>有状态 ACL 规则  
使用扩展 ACL 的另一个新功能可以配置有状态规则。 有状态规则筛选器基于五个属性中数据包的源 IP、 目标 IP、 协议、 源端口和目标端口的数据包。  
  
有状态规则提供以下功能：  
  
-   它们始终允许流量，而不可用于阻止流量。  
  
-   如果你指定参数“方向”的值为入站，并且流量与该规则匹配，那么，Hyper-V 虚拟交换机将动态创建一个匹配规则，用于允许 VM 发送出站流量以响应外部资源。  
  
-   如果你指定参数“方向”的值为出站，并且流量与该规则匹配，那么，Hyper-V 虚拟交换机将动态创建一个匹配规则，用于允许 VM 接收外部资源入站流量。  
  
-   这些规则包含一个以秒为计量单位的超时属性。 如果网络数据包到达交换机，并且该数据包与某个有状态规则匹配，那么，Hyper-V 虚拟交换机将创建一个状态，以便允许同一流量的、以两个方向传递的所有后续数据包。 如果在超时值指定的时间段内任一方向都没有出现流量，该状态将会过期。  
  
以下示例说明了如何使用有状态规则。  
  
### <a name="allow-inbound-remote-server-traffic-only-after-it-is-contacted-by-the-local-server"></a>只有在本地服务器联系了远程服务器之后才允许远程服务器的入站流量  
在某些情况下必须采用有状态规则，因为只有有状态规则才能跟踪已知的现成连接，以及将该连接与其他连接区分开来。  
  
例如，如果你要让 VM 应用程序服务器使用端口 80 发起与 Internet 上的 Web 服务器的连接，并且希望远程 Web 服务器能够响应 VM 流量，则可以配置一个有状态规则，以允许 VM 向 Web 服务传送初始出站流量；由于该规则是有状态的，因此也允许 Web 服务器向 VM 传回流量。 出于安全方面的原因，你可以阻止传往 VM 的所有其他入站网络流量。  
  
若要实现这种规则配置，你可以使用下表中的设置。  
  
> [!NOTE]  
> 由于格式方面的限制以及提供的信息量不同，下表中显示的信息与本文档前面表格中的信息有所不同。  
  
|参数|规则 1|规则 2|规则 3|  
|-------------|----------|----------|----------|  
|源 IP|*|*|*|  
|目标 IP|*|*|*|  
|协议|*|*|TCP|  
|源端口|*|*|*|  
|目标端口|*|*|80|  
|Direction|放大|缩小|缩小|  
|操作|拒绝|拒绝|允许|  
|有状态|否|否|是|  
|超时（以秒为单位）|不可用|不可用|3600|  
  
该有状态规则允许 VM 应用程序服务器连接到远程 Web 服务器。 发出第一个数据包时，Hyper-V 虚拟交换机将动态创建两个流量状态，以允许发送到远程 Web 服务器的所有数据包，并允许远程 Web 服务器传回的所有数据包。 当服务器之间的数据包流停止时，流量状态将在指定的超时值 3600 秒（一小时）内超时。  
  
以下示例说明了如何使用 Windows PowerShell 命令创建这些规则。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1   
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Outbound" 80 "TCP" -Weight 100 -Stateful -Timeout 3600  
```  
  


