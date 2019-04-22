---
title: 配置和查看 Hyper-V 虚拟交换机端口上的 VLAN 设置
description: 可以使用本主题以了解有关配置和查看 Windows Server 2016 中的 HYPER-V 虚拟交换机端口上的虚拟局域网 (VLAN) 设置的最佳实践。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e4843b0ffee86d728736ae212b953bb7c8552c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820548"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>配置和查看 Hyper-V 虚拟交换机端口上的 VLAN 设置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解有关配置和查看 HYPER-V 虚拟交换机端口上的虚拟局域网 (VLAN) 设置最佳实践。

当你想要的 HYPER-V 虚拟交换机端口上配置的 VLAN 设置时，可以使用任一 Windows&reg; Server 2016 HYPER-V Manager 或 System Center Virtual Machine Manager (VMM)。

如果正在使用 VMM，VMM 将使用以下 Windows PowerShell 命令来配置交换机端口。

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
如果未使用 VMM，并在 Windows Server 中配置的交换机端口，可以使用 Hyper-v 管理器控制台或以下 Windows PowerShell 命令。
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

由于 HYPER-V 虚拟交换机端口上配置的 VLAN 设置这两种方法，就可以，您尝试查看的交换机端口设置，该证书会显示给你，VLAN 设置未配置-即使在配置时。

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>使用相同的方法来配置和查看交换机端口的 VLAN 设置

若要确保不会遇到这些问题，必须使用相同的方法来查看您的交换机端口 VLAN 设置用来配置交换机端口 VLAN 设置。

若要配置和查看 VLAN 交换机端口设置，必须执行以下操作：

- 如果使用 VMM 或网络控制器来设置和管理您的网络，并且已部署软件定义网络 (SDN)，必须使用**VMNetworkAdapterIsolation** cmdlet。 
- 如果正在使用 Windows Server 2016 HYPER-V 管理器或 Windows PowerShell cmdlet，并且尚未部署软件定义网络 (SDN)，则必须使用**VMNetworkAdapterVlan** cmdlet。

### <a name="possible-issues"></a>可能的问题

如果不遵循这些准则可能会遇到以下问题。

- 在其中部署了 SDN 并使用 VMM 中，网络控制器的情况下或**VMNetworkAdapterIsolation** cmdlet 来配置的 HYPER-V 虚拟交换机端口上的 VLAN 设置：如果你使用 Hyper-v 管理器或**获取 VMNetworkAdapterVlan**若要查看配置设置，命令输出中将不显示你的 VLAN 设置。 您必须改用**Get VMNetworkIsolation** cmdlet 查看 VLAN 设置。
- 在您未部署 SDN 的信息，并改为使用 Hyper-v 管理器的情况下或**VMNetworkAdapterVlan** cmdlet 来配置的 HYPER-V 虚拟交换机端口上的 VLAN 设置：如果您使用**Get VMNetworkIsolation** cmdlet 来查看配置设置，命令输出不会显示你的 VLAN 设置。 您必须改用**获取 VMNetworkAdapterVlan** cmdlet 查看 VLAN 设置。

还有一点不会尝试使用这两种配置方法配置相同的交换机端口 VLAN 设置。 如果这样做，交换机端口配置不正确，并且结果可能是网络的通信失败。

### <a name="resources"></a>资源

本主题中提到的 Windows PowerShell 命令的详细信息，请参阅：

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)





