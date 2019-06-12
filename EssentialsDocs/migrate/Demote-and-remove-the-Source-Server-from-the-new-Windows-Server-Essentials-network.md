---
title: 降级和删除来自新的 Windows Server Essentials network1 源服务器
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e5bcdd58f4d88f7a555151d755bf427ecc9b5108
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433000"
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>降级和删除来自新的 Windows Server Essentials network1 源服务器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

完成安装 Windows Server Essentials 并完成迁移向导中的任务后，必须执行以下任务：  
  

1.  [卸载 Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003)。  
  
2.  [断开直接连接到源服务器的打印机的连接](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)。  
  
3.  [将源服务器降级](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)。  
  
4.  [将 DHCP 服务器角色从源服务器移到路由器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole)。  
  
5.  [删除源服务器并重新调整其用途](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

1.  [卸载 Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003)。  
  
2.  [断开直接连接到源服务器的打印机的连接](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)。  
  
3.  [将源服务器降级](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)。  
  
4.  [将 DHCP 服务器角色从源服务器移到路由器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole)。  
  
5.  [删除源服务器并重新调整其用途](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a> 卸载 Exchange Server 2003  
  
> [!IMPORTANT]
>  如果后将邮箱移到目标服务器和源服务器中卸载 Exchange Server 2003 之前添加的用户帐户，请在源服务器上添加邮箱。 这是设计使然。 你必须为所有在此期间添加的用户帐户将邮箱移动到目标服务器。 为 Windows Server Essentials 迁移重复移动 Exchange Server 邮箱和设置中的说明，然后再卸载 Exchange Server 2003。  
  
 降级之前，必须从源服务器卸载 Exchange Server 2003。 这为源服务器上的 Exchange Server Active Directory 域服务 (AD DS) 中删除所有引用。 您必须具有你要删除 Exchange Server 2003 的 Windows Small Business Server 2003 媒体。  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>若要从源服务器卸载 Exchange Server 2003  
  
1. 以管理员身份登录到源服务器  
  
2. 依次单击“开始”  、“控制面板”  ，然后单击“添加或删除程序”  。  
  
3. 在程序列表中，选择**Windows Small Business Server 2003**，然后单击**更改/删除**。  
  
4. 在设置向导中，单击“下一步”  ，直到“组件选择”  页面出现。  
  
5. 在组件选择页面上，展开“Exchange Server”  ，然后选择“删除”  。  
  
   > [!NOTE]
   > 
   >  Exchange Server 将进行检查以确保服务器上没有邮箱或公共文件夹。 如果保留了任何数据，将在你单击“删除”  时显示错误消息。 若要避免此问题，请确保你已完成所有主题中的过程[移动 SBS 2003 设置和数据到目标服务器](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  
   > 
   >  Exchange Server 将进行检查以确保服务器上没有邮箱或公共文件夹。 如果保留了任何数据，将在你单击“删除”  时显示错误消息。 若要避免此问题，请确保你已完成所有主题中的过程[移动 SBS 2003 设置和数据到目标服务器](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  

  
6. 单击“下一步”  。  
  
7. 出现提示时，插入 Windows Small Business Server 2003 CD #3 中，并按照屏幕上的说明。  
  
###  <a name="BKMK_PhysicallyDisconnect"></a> 断开直接连接到源服务器的打印机连接  
 在降级源服务器之前，请以物理方式断开直接连接到源服务器并通过源服务器共享的所有打印机的连接。 确保不会为已直接连接到源服务器的打印机保留任何 Active Directory 对象。 打印机可直接连接到目标服务器和从 Windows Server Essentials 中共享。  
  
###  <a name="BKMK_DemoteTheSourceServer"></a> 将源服务器降级  
 在将源服务器从 AD DS 域控制器的角色降级为域成员服务器的角色之前，请确保将组策略设置应用于所有的客户端计算机，如以下过程中所述。  
  
> [!IMPORTANT]
>  更新客户端计算机上的组策略更改时，源服务器和目标服务器必须连接到网络。  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>强制执行客户端计算机上的组策略更新  
  
1.  以管理员身份登录到客户端计算机。  
  
2.  以管理员身份打开“命令提示符”窗口。  
  
3.  在命令提示符下，键入 **gpupdate /force**，然后按 Enter。  
  
4.  该过程可能需要注销并重新登录才能完成。 单击“是”  以确认。  
  
##### <a name="to-demote-the-source-server"></a>对源服务器进行降级  
  
1. 在源服务器上，依次单击“开始”  、“运行”  ，键入 **dcpromo**，然后单击“确定”  。  
  
2. 单击“下一步”  两次。  
  
   > [!NOTE]
   >  请不要选择“此服务器是域中的最后一个域控制器”  。  
  
3. 在服务器上，键入新的管理员帐户的密码，然后单击“下一步”  。  
  
4. 在中**摘要**对话框中，系统将通知你，将从计算机中删除 AD DS，并且服务器将成为域的成员。 单击“下一步”  。  
  
5. 单击 **“完成”** 。 源服务器将重新启动。  
  
6. 在源服务器重新启动之后，请先将源服务器添加为工作组的成员，然后再断开它到网络的连接。  
  
   在将源服务器添加为工作组的成员并断开它到网络的连接之后，必须将它从目标服务器上的 AD DS 中删除。  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>从 Active Directory 中删除源服务器  
  
1.  在目标服务器上，打开“Active Directory 用户和计算机”  。  
  
2.  在“Active Directory 用户和计算机”  导航窗格中，展开域名，然后再展开“计算机”  。  
  
3.  右键单击源服务器名称（如果它仍存在于服务器列表中），单击“删除”  ，然后单击“是”  。  
  
4.  验证源服务器是否未列出，然后关闭“Active Directory 用户和计算机”  。  
  
###  <a name="BKMK_MoveTheDHCPRole"></a> 将 DHCP 服务器角色从源服务器移到路由器  
  
> [!NOTE]
> 
>  如果你在开始迁移过程前已执行此任务，请继续进行[删除源服务器并重新调整其用途](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)部分。  
> 
>  如果你在开始迁移过程前已执行此任务，请继续进行[删除源服务器并重新调整其用途](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)部分。  

  
 如果源服务器正在运行 DHCP 角色，则请执行下列步骤以将 DHCP 角色移到路由器。  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>将 DHCP 角色从源服务器移到路由器  
  
1.  关闭源服务器上的 DHCP 服务，如下所示：  
  
    1.  在源服务器上，依次单击“开始”  、“管理工具”  ，然后单击“服务”  。  
  
    2.  在当前运行的服务列表中，右键单击“Windows 服务器”  ，然后单击“属性”  。  
  
    3.  对于“启动类型”  ，选择“禁用”  。  
  
    4.  停止服务。  
  
2.  启用路由器上的 DHCP 角色  
  
    1.  按照路由器文档中的说明，启用路由器上的 DHCP 角色。  
  
    2.  若要确保源服务器颁发的 IP 地址保持不变，请按照路由器文档中的说明，将路由器上的 DHCP 范围配置为与源服务器上的 DHCP 范围相同。  
  
    > [!IMPORTANT]
    >  如果你尚未为目标服务器在路由器上设置静态 IP 或 DHCP 保留，并且 DHCP 范围与源服务器不同，则可能路由器将为目标服务器颁发一个新的 IP 地址。 如果发生这种情况，则请重置路由器的端口转发规则，以转发到目标服务器的新 IP 地址。  
  
###  <a name="BKMK_RemoveTheSourceServer"></a> 删除并重新使用源服务器  
 关闭源服务器并断开它到网络的连接。 我们建议你至少在一周的时间内不要重新格式化源服务器，以确保所有必要的数据都迁移到目标服务器。 在验证所有数据已迁移之后，如有必要，你可以在网络上将此服务器作为辅助服务器进行重新安装，以用于其他任务。  
  
> [!NOTE]
>  降级和删除源服务器后，请重新启动目标服务器。  
  
 降级源服务器后，它将不会处于正常运行状态。 如果你想要调整源服务器的用途，最简单的方法是将其重新格式化、安装服务器操作系统，然后对其进行设置以用作另一台服务器。
