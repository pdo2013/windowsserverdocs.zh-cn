---
title: 在主计算机或 VM 上创建新的 NIC 组
description: 在本主题中，你将在主计算机或运行 Windows Server 2016 的 Hyper-v 虚拟机（VM）中创建新的 NIC 组。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 1785b34741ce525a5bdd27b77a0e52fc2ca6c1b6
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591104"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>在主计算机或 VM 上创建新的 NIC 组

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，你将在主计算机或运行 Windows Server 2016 的 Hyper-v 虚拟机（VM）中创建新的 NIC 组。  

## <a name="network-configuration-requirements"></a>网络配置要求  
在创建新的 NIC 组之前，必须部署具有两个连接到不同物理交换机的网络适配器的 Hyper-v 主机。 还必须将网络适配器配置为具有来自相同 IP 地址范围的 IP 地址。  

在 VM 中创建 NIC 组的物理交换机、Hyper-v 虚拟交换机、局域网（LAN）和 NIC 组合要求如下：  

-   运行 Hyper-v 的计算机必须具有两个或多个网络适配器。  

-   如果将网络适配器连接到多个物理交换机，则物理交换机必须位于同一第2层子网中。  

-   必须使用 Hyper-v 管理器或 Windows PowerShell 来创建两个外部 Hyper-v 虚拟交换机，每个交换机都连接到不同的物理网络适配器。  

-   VM 必须连接到创建的外部虚拟交换机。  

-   Windows Server 2016 中的 NIC 组合支持 Vm 中包含两个成员的团队。 你可以创建更大的团队，但不支持。

-   如果在 VM 中配置 NIC 组，则必须选择 "_独立交换机_"和 "_地址哈希_的**负载平衡" 模式**。 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>步骤 1： 配置物理网络和虚拟网络  
在此过程中，你将创建两个外部 Hyper-v 虚拟交换机，将 VM 连接到交换机，然后配置与交换机的 VM 连接。  

### <a name="prerequisites"></a>必备条件

您必须拥有**管理员**成员身份或同等身份。  

### <a name="procedure"></a>过程

1.  在 Hyper-v 主机上，打开 "Hyper-v 管理器"，然后在 "操作" 下单击 "**虚拟交换机管理器**"。  

   ![虚拟交换机管理器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  在 "虚拟交换机管理器" 中，确保已选择 "**外部**"，然后单击 "**创建虚拟交换机**"。  

   ![创建虚拟交换机](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  在 "虚拟交换机属性" 中，键入虚拟交换机的**名称**，并根据需要添加**注释**。  

4.  在 "**连接类型**" 的 "**外部网络**" 中，选择要向其附加虚拟交换机的物理网络适配器。  

5.  为你的部署配置其他交换机属性，然后单击 **"确定"** 。  

6.  通过重复前面的步骤创建第二个外部虚拟交换机。 将第二个外部交换机连接到其他网络适配器。  

7.  在 "Hyper-v 管理器" 中的 "**虚拟机**" 下，右键单击要配置的 VM，然后单击 "**设置**"。  

   此时将打开 "VM**设置**" 对话框。

8.  确保未启动 VM。 如果已启动，请在配置 VM 之前执行关闭。  

8.  在 "**硬件**" 中，单击 "**网络适配器**"。  

   ![网络适配器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. 在 "**网络适配器**属性" 中，选择在前面的步骤中创建的某个虚拟交换机，然后单击 "**应用**"。  

    ![选择虚拟交换机](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. 在 "**硬件**" 中，单击 "**网络适配器**" 旁边的加号（+）。 

11. 单击 "**高级功能**"，使用图形用户界面启用 NIC 组合。 

    >[!TIP]
    >还可以通过 Windows PowerShell 命令启用 NIC 组合： 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. 选择 "**动态**" 作为 MAC 地址。 

    b. 单击选择 "**受保护的网络**"。 

    c. 单击选择 **"启用此网络适配器作为来宾操作系统中的组的一部分"** 。 

    d. 单击**确定**。  

    ![将网络适配器添加到团队](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. 若要添加另一个网络适配器，请在 Hyper-v 管理器的 "**虚拟机**" 中，右键单击同一个 VM，然后单击 "**设置**"。  

    此时将打开 "VM**设置**" 对话框。

14. 在 "**添加硬件**" 中，单击 "**网络适配器**"，然后单击 "**添加**"。  

   ![添加网络适配器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. 在 "**网络适配器**属性" 中，选择在前面的步骤中创建的第二个虚拟交换机，然后单击 "**应用**"。  

   ![应用虚拟交换机](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. 在 "**硬件**" 中，单击 "**网络适配器**" 旁边的加号（+）。 

17. 单击 "**高级功能**"，向下滚动到 " **NIC 组合**"，然后单击选择 **"启用此网络适配器作为来宾操作系统中的组的一部分"** 。 

18. 单击**确定**。  

_**恭喜!**_  你已经配置了物理网络和虚拟网络。  现在，你可以继续创建新的 NIC 组。  

## <a name="step-2-create-a-new-nic-team"></a>步骤 2： 创建新的 NIC 组

创建新的 NIC 组时，必须配置 NIC 组属性：  

-   团队名称  

-   成员适配器  

-   组合模式  

-   负载均衡模式  

-   备用适配器  

你还可以选择配置主要团队界面并配置虚拟 LAN （VLAN）号码。  

有关这些设置的详细信息，请参阅[NIC 组合设置](nic-teaming-settings.md)。

### <a name="prerequisites"></a>必备条件

您必须拥有**管理员**成员身份或同等身份。  

### <a name="procedure"></a>过程

1. 在服务器管理器中，单击 **“本地服务器”** 。  

2. 在 "**属性**" 窗格的第一列中，找到 " **NIC 组合**"，然后单击**禁用**的链接。  

   此时将打开 " **NIC 组合**" 对话框。

   ![“NIC 组合”对话框](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. 在 "**适配器和接口**" 中，选择要添加到 NIC 组中的一个或多个网络适配器。  

4. 单击 "**任务**"，然后单击 "**添加到新团队**"。  

   此时将打开 "**新建团队**" 对话框并显示网络适配器和团队成员。

5. 在 "**团队名称**" 中，键入新 NIC 组的名称，然后单击 "**其他属性**"。  

6. 在 "**其他属性**" 中，选择以下项的值：

   - **组合模式**。 分组模式选项是**独立交换**和**交换机依赖**的选项。 交换机相关模式包括**静态组合**和**链接聚合控制协议（LACP）** 。 

     - **独立切换。** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **依赖于交换机。** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **静态组合**              |                                                                                                                                              要求您手动配置交换机和主机来识别构成团队的链接。 由于这是一个静态配置的解决方案，因此没有其他协议可帮助交换机和主机标识错误插入的电缆或可能导致团队无法执行的其他错误。 服务器级交换机通常支持这种模式。                                                                                                                                              |
       | **链接聚合控制协议（LACP）** | 与静态组合不同，LACP 组合模式动态标识在主机和交换机之间连接的链接。 这种动态连接使团队能够自动创建，但在理论上，只是通过传输或接收来自对等实体的 LACP 数据包来扩展和减少团队。 所有服务器类交换机都支持 LACP，所有这些交换机都需要网络运营商在交换机端口上以管理方式启用 LACP。 配置 LACP 的分组模式时，NIC 组合始终在 LACP 的活动模式下运行。  默认情况下，NIC 组合使用短计时器（3秒），但你可以使用 `Set-NetLbfoTeam` 配置长计时器（90秒）。 |

       ---

   - **负载平衡模式**。 负载均衡分布模式的选项包括：**地址哈希**、 **Hyper-v 端口**和**动态**。

     - **地址哈希。** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Hyper-v 端口。** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **动态.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **备用适配器**。 备用适配器的选项为 "**无" （所有适配器处于活动状态）** ，或选择作为备用适配器的 NIC 组中的特定网络适配器。

   > [!TIP]  
   > 如果在虚拟机（VM）中配置 NIC 组，则必须选择 "_独立交换机_"和 "_地址哈希_的**负载平衡" 模式**。  

7. 如果要配置主要团队界面名称或向 NIC 组分配 VLAN 编号，请单击 "**主要团队界面**" 右侧的链接。  

    此时将打开 "**新建团队界面**" 对话框。

    ![新团队界面](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. 根据你的要求，请执行以下操作之一：  

   -   提供 tNIC 接口名称。  

   -   配置 VLAN 成员身份：单击 "**特定 vlan** "，然后键入 VLAN 信息。 例如，如果要将此 NIC 组添加到记帐 VLAN 编号44，请键入 "记账 44-VLAN"。   

9. 单击**确定**。  

_**恭喜!**_  已在主计算机或 VM 上创建了新的 NIC 组。

## <a name="related-topics"></a>相关主题

- [NIC 组合](NIC-Teaming.md)：在本主题中，我们将概述 Windows Server 2016 中的网络接口卡（NIC）组合。 NIC 组合允许在一个或多个基于软件的虚拟网络适配器之间分组到32物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。   

- [NIC 组合 MAC 地址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md)：在使用 "交换机独立" 模式配置 nic 组，并使用 "地址哈希" 或 "动态负载分配" 时，团队将在出站上使用主 NIC 团队成员的媒体访问控制（MAC）地址交易. 主 NIC 组成员是操作系统从一组初始团队成员中选择的网络适配器。

- [Nic 组合设置](nic-teaming-settings.md)：在本主题中，我们将为你概述 NIC 组属性，例如组合和负载平衡模式。 此外，我们还会向你介绍备用适配器设置和主团队接口属性的详细信息。 如果 NIC 组中至少有两个网络适配器，则无需指定备用适配器来实现容错。

- [Nic 组合故障排除](Troubleshooting-NIC-Teaming.md)：在本主题中，我们将讨论使用 Windows POWERSHELL 对 nic 组合（如硬件、物理交换机证券和禁用或启用网络适配器）进行故障排除的方法。 

---
