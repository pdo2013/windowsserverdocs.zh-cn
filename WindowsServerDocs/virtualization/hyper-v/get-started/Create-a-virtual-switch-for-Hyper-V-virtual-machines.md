---
title: 创建的虚拟交换机的 HYPER-V 虚拟机。
description: 提供创建虚拟交换机使用的 HYPER-V 管理器或 Windows PowerShell 的说明
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 2668f9fa21c8efbad455d82c7e110ff89b729187
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222868"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>创建的虚拟交换机的 HYPER-V 虚拟机。

>适用于：Windows 10、 Windows Server 2016、 Microsoft HYPER-V Server 2016，Windows Server 2019，Microsoft HYPER-V Server 2019
  
虚拟交换机允许在与其他计算机进行通信的 HYPER-V 主机上创建的虚拟机。 首先在 Windows Server 上安装 HYPER-V 角色时，你可以创建虚拟交换机。 若要创建其他虚拟交换机，请使用 HYPER-V 管理器或 Windows PowerShell。 若要了解有关虚拟交换机的详细信息，请参阅[HYPER-V 虚拟交换机](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
虚拟机网络可以是一个复杂的主题。 并且有可能想要的方式使用的几个新的虚拟交换机功能[交换嵌入组合 (SET)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set)。 但基本网络是相当轻松地完成。 本主题介绍刚好足够，以便可以在 HYPER-V 中创建为联网的虚拟机。 若要了解有关如何设置网络基础结构的详细信息，请查看[网络](../../../networking/Networking.md)文档。   
  
## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>通过使用 Hyper-v 管理器创建虚拟交换机  
  
1.  打开 HYPER-V 管理器中，选择 HYPER-V 主机计算机名称。  
  
2.  选择**操作** > **虚拟交换机管理器**。  
  
    ![显示操作菜单选项的屏幕截图 > 虚拟交换机管理器](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  选择所需的虚拟交换机的类型。  
  
    |连接类型|描述|  
    |-------------------|---------------|  
    |外部|允许与服务器和客户端上的外部网络进行通信的物理网络的虚拟机访问。 允许在同一台 HYPER-V 服务器与彼此进行通信的虚拟机。|  
    |内部|允许虚拟机和管理主机操作系统之间以及同一 HYPER-V 服务器上的虚拟机之间进行通信。|  
    |Private|仅允许在同一台 HYPER-V 服务器上的虚拟机之间的通信。 专用网络与 HYPER-V 服务器上的所有外部网络流量隔离。 此类型的网络时，必须创建隔离的网络环境，如隔离的测试域。|  
  
4.  选择**创建虚拟交换机**。  
  
5.  添加虚拟交换机的名称。  
  
6.  如果选择外部，选择你想要使用的网络适配器 (NIC) 和下表中所述的任何其他选项。  
  
    ![显示外部网络选项的屏幕截图](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |设置名称|描述|  
    |----------------|---------------|  
    |允许管理操作系统共享此网络适配器|如果你想要允许 HYPER-V 主机共享的虚拟交换机的使用和 NIC 或 NIC 团队与虚拟机，请选择此选项。 启用此功能，主机可以使用任何你配置的设置用于服务质量 (QoS) 设置、 安全设置或 HYPER-V 虚拟交换机的其他功能等的虚拟交换机。|  
    |启用单根 I/O 虚拟化 (SR-IOV)|选择此选项，仅当你想要允许虚拟机流量绕过虚拟机切换器，可以直接访问物理 nic。 有关详细信息，请参阅[单根 I/O 虚拟化](https://technet.microsoft.com/library/dn641211.aspx#Sec4)海报配套引用中：HYPER-V 联网。|  
  
7.  如果想要隔离网络流量从管理的 HYPER-V 主机操作系统或其他共享同一个虚拟交换机的虚拟机，请选择**启用管理操作系统的虚拟 LAN 标识**。 可以更改为任意数量的 VLAN ID 或保留默认值。 这是在管理操作系统将使用的所有网络通信通过此虚拟交换机的虚拟 LAN 标识号。  
  
    ![显示的 VLAN ID 选项的屏幕截图](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  单击 **“确定”** 。  
  
9. 单击 **“是”** 。  
  
    ![显示"挂起的更改可能会中断网络连接"消息的屏幕截图](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>通过使用 Windows PowerShell 创建虚拟交换机  
  
1.  在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。  
  
2.  右键单击 Windows PowerShell，然后选择**以管理员身份运行**。  
  
3.  通过运行查找现有的网络适配器[Get-netadapter](https://technet.microsoft.com/library/jj130867.aspx) cmdlet。 记下想要用于虚拟交换机的网络适配器名称。  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  通过使用创建虚拟交换机[New-vmswitch](https://technet.microsoft.com/library/hh848455.aspx) cmdlet。 例如，若要创建外部虚拟交换机名为 ExternalSwitch，使用以太网网络适配器，且**允许管理操作系统共享此网络适配器**开启，运行以下命令。  
  
    ```  
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true  
    ```  
  
    若要创建内部交换机，请运行以下命令。  
  
    ```  
    New-VMSwitch -name InternalSwitch -SwitchType Internal  
    ```  
  
    若要创建专用交换机，请运行以下命令。  
  
    ```  
    New-VMSwitch -name PrivateSwitch -SwitchType Private  
    ```  
  
介绍 Windows Server 2016 中的改进或新的虚拟交换机功能的更高级 Windows PowerShell 脚本，请参阅[远程直接内存访问和切换嵌入组合](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  

  
## <a name="next-step"></a>下一步  
[在 Hyper-V 中创建虚拟机](Create-a-virtual-machine-in-Hyper-V.md)  
  


