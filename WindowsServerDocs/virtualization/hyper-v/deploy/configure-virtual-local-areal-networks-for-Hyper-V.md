---
title: 为 HYPER-V 配置虚拟局域网
description: 提供用于 HYPER-V 主机上的虚拟机配置虚拟局域网 (VLAN) 的说明。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: 5b5eaf175e7c09124aaa3f7a33523e8b87a9ae84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848458"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>为 HYPER-V 配置虚拟局域网
虚拟局域网\(Vlan\)提供隔离的网络流量的一种方法。 Vlan 在交换机和路由器支持 802.1q 的配置。 如果你配置多个 Vlan，并且想以它们之间进行通信，你需要配置网络设备为允许的。 

您将需要以下操作来配置 Vlan:  
  
-   物理网络适配器和驱动程序，它支持 802.1q VLAN 标记。  
-   物理网络交换机支持 802.1q VLAN 标记。  
  
在主机上，你将配置的虚拟交换机允许物理交换机端口上的网络流量。 这是你想要与虚拟机在内部使用的 VLAN id。 接下来，您配置虚拟机，若要指定虚拟机将使用的所有网络通信的 VLAN。  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>若要允许使用 VLAN 的虚拟交换机  
  
1.  打开超\-管理器。  
  
2.  从操作菜单中，单击**虚拟交换机管理器**。  
  
3.  下**虚拟交换机**，选择虚拟交换机连接到支持 Vlan 的物理网络适配器。 

4. 在右窗格中，在 VLAN ID 下选择**启用虚拟 LAN 标识**然后键入一个数字的 VLAN id。  
  
    将使用您设置的 VLAN ID 标记穿过物理网络适配器连接到虚拟交换机的所有流量。  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>若要允许虚拟机以使用 VLAN  
  
1.  打开超\-管理器。  
  
2.  在结果窗格中下,**虚拟机**，选择相应的虚拟机，然后右键单击**设置**。  

3.  下**硬件**，选择使用 VLAN 设置的虚拟交换机。
  
4.  在右窗格中，选择**启用虚拟 LAN 标识**，然后键入与为虚拟交换机指定相同的 VLAN ID。 

如果虚拟机需要使用更多 Vlan，请执行以下操作：  
  
-   连接多个虚拟网络适配器，以相应的虚拟交换机并分配 VLAN Id。 请确保正确配置的 IP 地址，并要路由通过 VLAN 的流量使用正确的 IP 地址。  
  
-   在 trunk 模式下使用配置虚拟网络 word 适配器[设置\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) cmdlt。
  
## <a name="see-also"></a>请参阅  
 
[超\-V 虚拟交换机](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)