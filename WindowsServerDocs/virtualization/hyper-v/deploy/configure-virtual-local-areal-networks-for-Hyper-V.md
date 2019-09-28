---
title: 配置 Hyper-v 的虚拟局域网
description: 提供有关配置虚拟局域网（VLAN）以供 Hyper-v 主机上的虚拟机使用的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: f2c240a3ad9f9783e509efb288cc6c6410339685
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364276"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>配置 Hyper-v 的虚拟局域网
虚拟局域网 \(VLANs @ no__t 提供一种隔离网络流量的方式。 Vlan 在支持 802.1 q 的交换机和路由器中进行配置。 如果你配置了多个 Vlan 并且需要在它们之间进行通信，则需要将网络设备配置为允许。 

你将需要以下内容来配置 Vlan：  
  
-   支持 802.1 q VLAN 标记的物理网络适配器和驱动程序。  
-   支持 802.1 q VLAN 标记的物理网络交换机。  
  
在主机上，将虚拟交换机配置为允许物理交换机端口上的网络流量。 这适用于要在内部与虚拟机一起使用的 VLAN Id。 接下来，配置虚拟机以指定虚拟机将用于所有网络通信的 VLAN。  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>允许虚拟交换机使用 VLAN  
  
1.  打开 "超级 @ no__t-0V Manager"。  
  
2.  在 "操作" 菜单中，单击 "**虚拟交换机管理器**"。  
  
3.  在 "**虚拟交换机**" 下，选择连接到支持 vlan 的物理网络适配器的虚拟交换机。 

4. 在右窗格中的 "VLAN ID" 下，选择 "**启用虚拟 LAN 标识**"，然后键入 VLAN ID 的数字。  
  
    所有通过物理网络适配器连接到虚拟交换机的流量都将使用您设置的 VLAN ID 进行标记。  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>允许虚拟机使用 VLAN  
  
1.  打开 "超级 @ no__t-0V Manager"。  
  
2.  在结果窗格中的 "**虚拟机**" 下，选择相应的虚拟机，然后右键单击 "**设置**"。  

3.  在 "**硬件**" 下，选择设置有 VLAN 的虚拟交换机。
  
4.  在右侧窗格中，选择 "**启用虚拟 LAN 标识**"，然后键入与为虚拟交换机指定的相同的 VLAN ID。 

如果虚拟机需要使用更多 Vlan，请执行以下操作之一：  
  
-   将更多虚拟网络适配器连接到相应的虚拟交换机，并分配 VLAN Id。 请确保正确配置 IP 地址，并且要通过 VLAN 路由的流量也使用正确的 IP 地址。  
  
-   使用[Set @ no__t-1VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) cmdlt 配置干线模式下的虚拟网络字适配器。
  
## <a name="see-also"></a>请参阅  
 
[超级 @ no__t-1V 虚拟交换机](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)