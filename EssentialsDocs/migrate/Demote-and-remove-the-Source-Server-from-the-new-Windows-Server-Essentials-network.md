---
title: "降级和从新的 Windows Server Essentials 网络 1 删除源服务器"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 1545189732194ad5c0aba401f834b0102799e016
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>降级和从新的 Windows Server Essentials 网络 1 删除源服务器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

安装 Windows Server Essentials 完并完成迁移向导中的任务后，你必须执行以下任务：  
  

1.  [卸载 Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003)。  
  
2.  [断开连接打印机直接连接到源代码服务器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)。  
  
3.  [降级源服务器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)。  
  
4.  [从源服务器 DHCP 服务器角色转为路由器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole)。  
  
5.  [删除和重新源服务器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

1.  [卸载 Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003)。  
  
2.  [断开连接打印机直接连接到源代码服务器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)。  
  
3.  [降级源服务器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)。  
  
4.  [从源服务器 DHCP 服务器角色转为路由器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole)。  
  
5.  [删除和重新源服务器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a>卸载 Exchange Server 2003  
  
> [!IMPORTANT]
>  移动到目标服务器和您从源服务器卸载 Exchange Server 2003 之前邮箱之后，你可以添加用户帐户，如果邮箱添加源代码服务器上。 这是设计。 你必须邮箱移到目标针对所有用户帐户将添加在此期间，服务器。 卸载 Exchange Server 2003 前，Windows Server Essentials 迁移重复移动 Exchange Server 邮箱和设置中的说明进行操作。  
  
 你必须从源服务器卸载 Exchange Server 2003 之前将其降级。 这将所有引用都删除 Active Directory 域服务 (广告 DS) 的源代码服务器上的 Exchange 服务器在。 你必须要删除 Exchange Server 2003 Windows 小型企业 Server 2003 媒体。  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>若要从源服务器卸载 Exchange Server 2003  
  
1.  以管理员身份登录到源代码服务器  
  
2.  单击**开始**，单击**“控制面板”**，然后单击**添加或删除程序**。  
  
3.  在程序的列表中，选择**Windows 小型企业 Server 2003**，然后单击**更改/删除**。  
  
4.  在设置向导中，单击**下一步**直到**组件选择**页面显示时。  
  
5.  在组件选择页面上，展开**Exchange Server**，然后选择**删除**。  
  
    > [!NOTE]

    >  Exchange Server 将检查以确保没有邮箱或服务器上的公共文件夹。 如果将保留的任何数据，请单击时出现错误消息**删除**。 若要避免此问题，请确保你已完成所有主题中的步骤[移动 SBS 2003 设置和数据到目标服务器](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  

    >  Exchange Server 将检查以确保没有邮箱或服务器上的公共文件夹。 如果将保留的任何数据，请单击时出现错误消息**删除**。 若要避免此问题，请确保你已完成所有主题中的步骤[移动 SBS 2003 设置和数据到目标服务器](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  

  
6.  单击**下一步**。  
  
7.  看到提示后，插入 Windows 小型企业服务器 2003 CD # 3 平板电脑，并按照屏幕说明操作。  
  
###  <a name="BKMK_PhysicallyDisconnect"></a>断开连接直接连接到源代码服务器的打印机  
 降级源服务器之前，断开物理连接任何直接连接到源代码服务器和共享通过源服务器的打印机。 请确保没有 Active Directory 对象保持直接连接到源代码服务器的打印机。 打印机可以然后直接连接到目标 Server 和 Windows Server Essentials 从共享。  
  
###  <a name="BKMK_DemoteTheSourceServer"></a>降级源服务器  
 降级源服务器的广告 DS 域控制器角色域成员服务器的作用之前，请确保组策略设置将应用于所有客户端计算机，如下面的过程中所述。  
  
> [!IMPORTANT]
>  组策略更改的更新的客户端计算机上时，源服务器和目的地服务器必须连接到该网络。  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>若要在客户端计算机上强制更新组策略  
  
1.  以管理员身份登录到客户端计算机上。  
  
2.  以 administrator 身份打开一个 Command Prompt 窗口。  
  
3.  在命令提示符下，键入**gpupdate /force**，然后按 ENTER。  
  
4.  该过程可能需要注销并重新登录才能完成。 单击**是**以确认。  
  
##### <a name="to-demote-the-source-server"></a>若要将降源服务器  
  
1.  在源服务器上，单击**开始**，单击**运行**，类型**dcpromo**，然后单击**确定**。  
  
2.  单击**下一步**两次。  
  
    > [!NOTE]
    >  不要选择**此服务器是在域中的最后一个域控制器**。  
  
3.  在服务器上，键入新管理员帐户的密码，然后单击**下一步**。  
  
4.  在**摘要**对话框中，通知您，将从计算机中删除广告 DS 和的服务器将成为域中的成员。 单击**下一步**。  
  
5.  单击**完成**。 在源服务器重启。  
  
6.  源服务器重启后，在工作组中的成员添加源代码服务器之前断开网络。  
  
 添加源代码服务器以在工作组中的成员，并断开网络后，必须从广告 DS 目标服务器上将其删除。  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>若要删除 Active Directory 源服务器  
  
1.  在目标服务器上，打开**Active Directory 用户和计算机**。  
  
2.  在**Active Directory 用户和计算机**导航窗格中，展开域名，，然后展开**计算机**。  
  
3.  如果在列表中服务器，并且仍然存在源服务器名称单击右键单击**删除**，然后单击**是**。  
  
4.  验证源服务器是否未列出，然后再关闭**Active Directory 用户和计算机**。  
  
###  <a name="BKMK_MoveTheDHCPRole"></a>从源服务器 DHCP 服务器角色转为路由器  
  
> [!NOTE]

>  如果你已执行此任务中，启动的迁移进程之前，继续一部分[删除和重新源服务器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

>  如果你已执行此任务中，启动的迁移进程之前，继续一部分[删除和重新源服务器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

  
 如果你的源服务器运行的 DHCP 角色，执行以下步骤来进入路由器 DHCP 角色。  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>若要将从源服务器 DHCP 角色移动到路由器  
  
1.  关闭 DHCP 服务源在服务器上，如下所示：  
  
    1.  在源服务器上，单击**开始**，单击**管理工具**，然后单击**服务**。  
  
    2.  在当前运行的服务的列表中，右键单击**Windows Server**，然后单击**属性**。  
  
    3.  对于**开始键入**、选择**禁用**。  
  
    4.  停止的服务。  
  
2.  启用 DHCP 角色路由器上  
  
    1.  按照你的路由器相关文档中的说明，若要启用 DHCP 角色在路由器上。  
  
    2.  若要确保 IP 地址发出的源服务器保持不变，按照你的路由器相关文档中的说明配置为源服务器上的 DHCP 范围相同在路由器上 DHCP 范围。  
  
    > [!IMPORTANT]
    >  如果你尚未设置在路由器上的静态 IP 或 DHCP 预订目标服务器，并 DHCP 范围不源服务器相同，则可能路由器将新的 IP 地址发出的目标服务器。 如果发生这种情况，重置端口转发规则路由器转发给目标服务器的新 IP 地址的。  
  
###  <a name="BKMK_RemoveTheSourceServer"></a>删除和重新源服务器  
 关闭源服务器，并断开网络。 我们建议在未重新格式化至少一周源服务器，以确保所有必需的数据迁移到目的地的服务器。 验证具有迁移的所有数据后，你可以重装此服务器网络上为辅助服务器执行其他任务，如果需要。  
  
> [!NOTE]
>  你将降级并删除源服务器后，重新启动目标服务器。  
  
 降级源服务器后，它不在正常运行的状态。 如果你想要调整用途源服务器，最简单方法是将其重新格式化安装在服务器操作系统，然后将其设置为其他服务器的使用。
