---
title: 为 Hyper-v 虚拟机创建虚拟交换机
description: 提供有关使用 Hyper-v 管理器或 Windows PowerShell 创建虚拟交换机的说明
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f1a814060e763545411b5c4345367638a5161ac2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392922"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>为 Hyper-v 虚拟机创建虚拟交换机

>适用于：Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019
  
虚拟交换机允许在 Hyper-v 主机上创建的虚拟机与其他计算机进行通信。 在 Windows Server 上首次安装 Hyper-v 角色时，可以创建虚拟交换机。 若要创建其他虚拟交换机，请使用 Hyper-v 管理器或 Windows PowerShell。 若要详细了解虚拟交换机，请参阅[Hyper-v 虚拟交换机](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
虚拟机网络可能是一个复杂的主题。 你可能想要使用多个新的虚拟交换机功能，如[交换机嵌入组合（SET）](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set)。 但基本网络比较简单。 本主题介绍了足够的空间，以便可以在 Hyper-v 中创建联网的虚拟机。 若要详细了解如何设置网络基础结构，请查看[网络](../../../networking/Networking.md)文档。   
  
## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>使用 Hyper-v 管理器创建虚拟交换机  
  
1.  打开 "Hyper-v 管理器"，选择 Hyper-v 主机计算机名称。  
  
2.  选择**操作** > **虚拟交换机管理器**。  
  
    ![显示菜单选项操作 > 虚拟交换机管理器的屏幕截图](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  选择所需的虚拟交换机类型。  
  
    |连接类型|描述|  
    |-------------------|---------------|  
    |外部|为虚拟机提供对物理网络的访问权限，以与外部网络上的服务器和客户端进行通信。 允许相同 Hyper-v 服务器上的虚拟机相互通信。|  
    |内部|允许相同 Hyper-v 服务器上的虚拟机之间的通信，以及虚拟机与管理主机操作系统之间的通信。|  
    |Private|仅允许同一 Hyper-v 服务器上的虚拟机之间的通信。 专用网络与 Hyper-v 服务器上的所有外部网络流量隔离。 当必须创建独立的网络环境（如隔离的测试域）时，此类型的网络很有用。|  
  
4.  选择 "**创建虚拟交换机**"。  
  
5.  添加虚拟交换机的名称。  
  
6.  如果选择 "外部"，请选择要使用的网络适配器（NIC）以及下表中描述的任何其他选项。  
  
    ![显示外部网络选项的屏幕截图](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |设置名称|描述|  
    |----------------|---------------|  
    |允许管理操作系统共享此网络适配器|如果要允许 Hyper-v 主机与虚拟机共享虚拟交换机和 NIC 或 NIC 组的使用情况，请选择此选项。 启用此功能后，主机可以使用你为虚拟交换机配置的任何设置，如服务质量（QoS）设置、安全设置或 Hyper-v 虚拟交换机的其他功能。|  
    |启用单根 i/o 虚拟化（SR-IOV）|仅当你想要允许虚拟机通信绕过虚拟机交换机，并直接转到物理 NIC 时，才选择此选项。 有关详细信息，请参阅海报附属参考中的[单根 I/o 虚拟化](https://technet.microsoft.com/library/dn641211.aspx#Sec4)：Hyper-v 网络。|  
  
7.  如果要将管理 Hyper-v 主机操作系统或其他共享同一虚拟交换机的虚拟机中的网络流量隔离开来，请选择 "**为管理操作系统启用虚拟 LAN 标识**"。 可以将 VLAN ID 更改为任意数字，或保留默认值。 这是管理操作系统将用于通过此虚拟交换机进行的所有网络通信的虚拟 LAN 标识号。  
  
    ![显示 VLAN ID 选项的屏幕截图](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  单击 **“确定”** 。  
  
9. 单击 **“是”** 。  
  
    ![显示 "挂起的更改可能会中断网络连接" 消息的屏幕截图](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>使用 Windows PowerShell 创建虚拟交换机  
  
1.  在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。  
  
2.  右键单击 "Windows PowerShell" 并选择 "以**管理员身份运行**"。  
  
3.  通过运行[get-netadapter](https://technet.microsoft.com/library/jj130867.aspx) cmdlet 查找现有的网络适配器。 记下要用于虚拟交换机的网络适配器名称。  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  使用[纽约](https://technet.microsoft.com/library/hh848455.aspx)cmdlet 创建虚拟交换机。 例如，若要创建名为 ExternalSwitch 的外部虚拟交换机，使用以太网网络适配器，并启用了 "**允许管理操作系统共享此网络适配器**"，请运行以下命令。  
  
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
  
若要深入了解 Windows Server 2016 中的改进或全新虚拟交换机功能的更高级 Windows PowerShell 脚本，请参阅[远程直接内存访问和交换机嵌入式组合](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  

  
## <a name="next-step"></a>下一步  
[在 Hyper-V 中创建虚拟机](Create-a-virtual-machine-in-Hyper-V.md)  
  


