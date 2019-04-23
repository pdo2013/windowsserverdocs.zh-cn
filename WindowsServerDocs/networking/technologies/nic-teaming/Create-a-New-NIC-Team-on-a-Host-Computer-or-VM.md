---
title: 在主计算机或 VM 上创建新的 NIC 组
description: 本主题中的主计算机上或在运行 Windows Server 2016 的 HYPER-V 虚拟机 (VM) 中创建新的 NIC 组。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 3518436f08a0e7fe0ea8fed06724b11df6acccea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864628"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>在主计算机或 VM 上创建新的 NIC 组

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题中的主计算机上或在运行 Windows Server 2016 的 HYPER-V 虚拟机 (VM) 中创建新的 NIC 组。  

## <a name="network-configuration-requirements"></a>网络配置要求  
创建新的 NIC 组之前，必须部署具有两个连接到不同的物理交换机的网络适配器的 HYPER-V 主机。 您还必须使用来自同一 IP 地址范围的 IP 地址配置网络适配器。  

物理交换机、 HYPER-V 虚拟交换机、 局域网 (LAN) 和 NIC 组合在 VM 中创建 NIC 组的要求是：  
  
-   运行 HYPER-V 的计算机必须具有两个或多个网络适配器。  
  
-   如果连接到多个物理交换机的网络适配器，物理交换机必须位于相同的第 2 层子网。  
  
-   必须使用 HYPER-V 管理器或 Windows PowerShell 来创建两个外部 HYPER-V 虚拟交换机，每个连接到不同的物理网络适配器。  
  
-   VM 必须连接到你创建这两个外部虚拟交换机。  
  
-   NIC 组合，在 Windows Server 2016 中，支持与在 Vm 中的两个成员的团队。 可以创建更大的团队，但不支持。
  
-   如果要在 VM 中配置 NIC 组，则必须选择**组合模式**的_独立于交换机_和一个**负载均衡模式**的_地址哈希_. 



## <a name="step-1-configure-the-physical-and-virtual-network"></a>步骤 1： 配置物理和虚拟网络  
在此过程中，您创建两个外部 HYPER-V 虚拟交换机、 将虚拟机连接到的交换机，然后配置的 VM 连接到的交换机。  
 

### <a name="prerequisites"></a>先决条件
  
您必须拥有成员身份**管理员**，或等效。  
  
### <a name="procedure"></a>过程
  
1.  在 HYPER-V 主机上打开 HYPER-V 管理器和操作下单击**虚拟交换机管理器**。  
  
    ![虚拟交换机管理器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  
  
2.  在虚拟交换机管理器，请确保**外部**已选中，然后单击**创建虚拟交换机**。  
  
    ![创建虚拟交换机](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  
  
3.  在虚拟交换机属性，键入**名称**为虚拟交换机，然后添加**说明**根据需要。  
  
4.  在中**连接类型**，在**外部网络**，选择你想要附加的虚拟交换机的物理网络适配器。  
  
5.  配置为你的部署，其他交换机属性，然后单击**确定**。  
  
6.  通过重复上一步骤中创建第二个外部虚拟交换机。 第二个外部交换机连接到不同的网络适配器。  
  
7.  中的 HYPER-V 管理器下**虚拟机**，右键单击你想要配置，然后单击 VM**设置**。<p>VM**设置**对话框随即打开。  

8.  请确保 VM 未启动。 如果它已启动，请配置 VM 之前执行关闭。  
  
8.  在中**硬件**，单击**网络适配器**。  
  
    ![网络适配器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  
  
9. 在中**网络适配器**属性中，选择一个在上一步骤中创建的虚拟交换机，然后单击**应用**。  
  
    ![选择虚拟交换机](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  
  
10. 在中**硬件**，单击此项可展开旁边的加号 （+）**网络适配器**。 

11. 单击**高级功能**若要使用图形用户界面中启用 NIC 组合。 

    >[!TIP]
    >此外可以使用 Windows PowerShell 命令启用 NIC 组合： 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```
   
    a. 选择**动态**MAC 地址。 

    b. 单击此项可选择**受保护网络**。 

    c. 单击此项可选择**启用此网络适配器作为来宾操作系统中团队的**。 

    d. 单击 **“确定”**。  
  
    ![向团队添加网络适配器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  
  
13. 若要在中添加的 HYPER-V 管理器中的第二个的网络适配器**虚拟机**，右键单击同一个 VM，然后单击**设置**。<p>VM**设置**对话框随即打开。  
  
14. 在中**添加硬件**，单击**网络适配器**，然后单击**添加**。  
  
    ![添加网络适配器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  
  
15. 在中**网络适配器**属性中，选择在上一步骤中创建的第二个虚拟交换机，然后单击**应用**。  
  
    ![将虚拟交换机](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  
  
16. 在中**硬件**，单击此项可展开旁边的加号 （+）**网络适配器**。 

17. 单击**高级功能**，向下滚动到**NIC 组合**，并单击以选择**启用此网络适配器作为来宾操作系统中团队的**。 

18. 单击 **“确定”**。  
  
_**祝贺您 ！**_  物理和虚拟网络配置。  现在可以转到创建新的 NIC 组。  

## <a name="step-2-create-a-new-nic-team"></a>步骤 2： 创建新的 NIC 组
  
在创建新的 NIC 组时，必须配置的 NIC 组属性：  
  
-   团队名称  
  
-   成员适配器  
  
-   组合模式  
  
-   负载均衡模式  
  
-   备用适配器  
  
还可以配置主要组接口和配置虚拟 LAN (VLAN) 数。  
  
有关这些设置的更多详细信息，请参阅[NIC 组合设置](nic-teaming-settings.md)。

### <a name="prerequisites"></a>先决条件

您必须拥有成员身份**管理员**，或等效。  
  
### <a name="procedure"></a>过程
  
1.  在服务器管理器中，单击 **“本地服务器”**。  
  
2.  在中**属性**窗格中，在第一列中，找到**NIC 组合**，然后单击**禁用**链接。<p>**NIC 组合**对话框随即打开。<p>![NIC 组合对话框](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)  
  
3.  在中**适配器和接口**，选择你想要添加到 NIC 组的一个或多个网络适配器。  
  
4.  单击**任务**，然后单击**将添加到新的团队**。<p>**新的团队**对话框将打开并显示网络适配器和团队成员。  
  
5.  在中**团队名称**，为新的 NIC 组中，键入一个名称，然后单击**其他属性**。  
  
6.  在中**附加属性**，为选择的值：

    - **组合模式**。 组合模式的选项都**独立于交换机**并**交换机依赖**。 依赖交换机的模式包括**静态组合**并**链接聚合控制协议 (LACP)**。 
      
      - **独立于交换机。** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]
      
      - **切换相关。** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 
      
        | | |
        |---|---|
        |**静态组合** | 要求您手动配置了交换机和主机以确定哪个链接就会形成团队。 由于这是静态配置的解决方案，没有其他协议，以帮助交换机和主机来识别未正确插入的电缆或可能造成该组失败执行的其他错误。 服务器级交换机通常支持这种模式。 |
        |**链接聚合控制协议 (LACP)** |与静态组合不同 LACP 组合模式动态找出主机和交换机之间的链接。 此动态连接，只需通过传输和接收 LACP 数据包从对等实体可以自动创建团队的和，从理论上讲，但很少的做法、 扩展和减少的团队中。 所有服务器级交换机都支持 LACP，并且所有需要网络运营商，以管理方式启用 LACP 的交换机端口上。 在配置的 LACP 组合模式时，NIC 组合始终会在运行 LACP 的主动模式下使用短计时器。  目前可修改计时器或 LACP 模式更改任何选项不。 |
        ---
    
    - **负载均衡模式**。 负载均衡分发模式的选项都**地址哈希**， **HYPER-V 端口**，并**动态**。
    
       - **地址哈希值。** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
       
       - **HYPER-V 端口。** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]
       
       - **动态。** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
    
    - **备用适配器**。 备用适配器的选项都**None （所有适配器处于活动状态）** 或您选择的充当待机适配器 NIC 组中的特定网络适配器。
   
   > [!TIP]  
   > 如果要在虚拟机 (VM) 中配置 NIC 组，则必须选择**组合模式**的_独立于交换机_和一个**负载均衡模式**的_地址哈希_。  
  
7.  如果你想要配置主要组接口名称或将 VLAN 分配到 NIC 组，请单击右侧的链接**主要组接口**。<p>**新的团队界面**对话框随即打开。<p>![新组接口](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)  
  
8.  具体取决于你的要求，请执行以下操作：  
  
    -   提供 tNIC 接口名称。  
  
    -   配置 VLAN 成员身份： 单击**特定 VLAN**键入 VLAN 信息。 例如，如果你想要将此 NIC 组添加到记帐 VLAN 编号 44，类型记帐 44-VLAN。   
  
9. 单击 **“确定”**。  

_**祝贺您 ！**_  你已在主计算机或 VM 上创建新的 NIC 组。

## <a name="related-topics"></a>相关主题
- [NIC 组合](NIC-Teaming.md):在本主题中，我们提供您的网络接口卡 (NIC) 合作概述 Windows Server 2016 中。 NIC 组合允许你将介于 1 和 32 之间分组到一个或多个基于软件的虚拟网络适配器的物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。   

- [NIC 组合 MAC 地址的使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md):在 NIC 组配置为切换独立模式和地址哈希或动态负载分发时，该团队使用的媒体访问控制 (MAC) 地址的出站流量上的主 NIC 组成员。 主 NIC 团队成员是由操作系统来自团队成员的初始集选择的网络适配器。

- [NIC 组合设置](nic-teaming-settings.md):在本主题中，我们为您提供的 NIC 组属性，如组合概述和负载平衡模式。 我们还会为您提供的备用适配器设置和主要组接口属性的详细信息。 如果 NIC 组中具有至少两个网络适配器，您不需要指定的容错能力的备用适配器。

- [NIC 组合疑难解答](Troubleshooting-NIC-Teaming.md):在本主题中，我们将讨论如何解决 NIC 组合，如硬件、 物理交换机证券和使用 Windows PowerShell 的禁用或启用网络适配器。 

---