---
title: 管理 Windows Server Essentials 中的设备
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5fe1088-ebe7-4799-a47d-075b0048dea1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 288d62a3fe4d9073ba2c0e3fdff385d8317f20d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815108"
---
# <a name="manage-devices-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的设备

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 以下部分讨论了服务器的设备管理功能，还介绍了如何在网络上设置和使用设备：  
  
-   [通过使用仪表板管理设备](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [将用户帐户分配登录特定网络计算机的权限](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [从服务器中删除计算机](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [配置文件夹重定向和安全性的组策略设置](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [使用远程桌面会话连接到网络计算机](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [查看计算机属性](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a> 通过使用仪表板管理设备  
 通过 Windows Server Essentials，可以使用 Windows Server Essentials 仪表板执行常见管理任务。 仪表板的“设备”页面提供了以下内容：  
  
-   显示以下内容的网络计算机列表：  
  
    -   计算机名称  
  
    -   计算机状态：“联机”或“脱机”  
  
    -   计算机说明  
  
    -   计算机的备份状态  
  
    -   计算机的更新状态  
  
    -   计算机的安全状态  
  
    -   计算机的警报状态  
  
    -   计算机的组策略信息  
  
-   带有有关选定计算机的附加信息的详细信息窗格  
  
-   一个任务窗格，它包含一组设备管理任务，例如查看计算机属性和警报、设置计算机备份以及从备份中还原文件和文件夹  
  
#### <a name="to-view-the-status-of-network-computers"></a>查看网络计算机的状态  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“设备”。  
  
3.  在列表窗格中查看网络中所有计算机的状态。  
  
 下表介绍了 Windows Server Essentials 仪表板中可用的各种计算机和备份任务。 某些任务特定于计算机，它们仅在选择列表中的某个计算机时才可见。  
  
### <a name="computer-tasks-in-the-dashboard"></a>仪表板中的计算机任务  
  
|任务名称|描述|  
|---------------|-----------------|  
|查看计算机属性|显示选定计算机的常规信息，并允许你查看计算机备份的详细信息。|  
|设置该计算机备份|运行“设置备份”向导。|  
|自定义计算机备份|打开备份属性，你可以通过它们更改选定计算机的备份设置。|  
|启动计算机备份|为选定的计算机启动备份。|  
|停止计算机备份|为选定的计算机停止备份。|  
|还原计算机的文件或文件夹|运行“还原文件和文件夹”向导，该向导使你能够还原特定文件、文件夹或驱动器。|  
|查看计算机警报|显示关键警报和其他信息性警报，并允许你采取更正措施（如果可能）。|  
|计算机的远程桌面|打开到选定计算机的远程桌面连接。|  
|删除计算机|运行“删除计算机”向导，该向导可以将计算机从 Windows Server Essentials 仪表板中分离。|  
|自定义计算机备份和文件历史记录设置|打开备份设置页面，你可以从中更改客户端计算机的备份计划和文件历史记录设置。|  
|如何将计算机连接到服务器？|打开帮助主题，该主题介绍了将计算机加入网络时需执行的步骤。|  
|实现组策略|将策略设置应用于已加入域的 Windows 8 和 Windows 7 计算机。|  
  
##  <a name="BKMK_2"></a> 将用户帐户分配登录特定网络计算机的权限  
 你可以为用户帐户分配权限，以使用户从远程位置访问 Windows Server Essentials 网络时只能登录到特定网络计算机。  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>更改用户帐户对计算机的访问权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“用户”。  
  
3.  在用户帐户列表中，选择要更改的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**查看帐户属性**。 这将显示用户帐户的“属性”  页面。  
  
5.  在“计算机访问”选项卡上，选择该用户可以远程访问的计算机，然后单击“确定”。  
  
##  <a name="BKMK_3"></a> 从服务器中删除计算机  
 当你使用仪表板从运行 Windows Server Essentials 的服务器中删除计算机时，该服务器将不再管理该计算机。 因此，在从网络中删除计算机后，服务器将停止创建计算机备份或监控其运行状况。  
  
> [!NOTE]
>  从服务器中删除计算机将不会从网络断开与该计算的连接。 该计算机仍可通过在连接到服务器之前采用的相同方式来访问网络上的资源。 若要阻止计算机访问服务器资源，并且将其从服务器断开连接，则必须从域中删除该计算机。 此外，从服务器中删除计算机时，不会自动从删除的计算机中卸载连接器软件或快速启动板。 必须手动从该计算机中删除连接器软件。 有关详细信息，请参阅卸载连接器软件中的[获取连接](../use/Get-Connected-in-Windows-Server-Essentials.md)。  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>使用仪表板从网络中删除计算机  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“设备”  选项卡。  
  
3.  在计算机列表中，右键单击要从网络中删除的计算机，然后单击“删除该计算机”。  
  
##  <a name="BKMK_5"></a> 配置文件夹重定向和安全性的组策略设置  
 通过使用 Windows Server Essentials 仪表板，可以配置组策略并将其部署到 Windows Server Essentials 网络中的计算机。 Windows Server Essentials 中的组策略包括影响 Windows 更新、Windows Defender 以及网络防火墙的文件夹重定向和安全性设置。  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>配置 Windows Server Essentials 中的组策略  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏上，单击“设备” 。  
  
3.  适用于 Windows Server Essentials:在全局“用户任务”窗格中，单击“实现组策略”。  
  
     适用于 Windows Server Essentials:在全局“设备任务”窗格中，单击“实现组策略”。  
  
4.  此时将打开“实现组策略”向导。  
  
5.  在该向导的“启用文件夹重定向组策略”页面上，你可以选择要重定向的用户文件夹。  
  
6.  在该向导的“启用安全策略设置”  页面上，你可以选择启用适用于“Windows 更新” 、“Windows Defender” 以及“网络防火墙” 的组策略设置。  
  
7.  单击“完成”  以实现组策略设置。  
  
##  <a name="BKMK_7"></a> 使用远程桌面会话连接到网络计算机  
 若要在离开办公室后，远程访问 Windows Server Essentials 网络计算机，使用 Web 浏览器登录到你的组织的远程 Web 访问网站，然后在**计算机**选项卡上，单击的名称计算机。  
  
 “状态”列将显示是否能连接到网络上的计算机，以及是否可以包含下列值：  
  
-   **已推出**  
  
     计算机已打开，并且可用于远程连接。 如果第三方防火墙阻止该连接，则即使你看到此状态，仍可能无法连接到此计算机。  
  
-   **脱机或处于睡眠状态**  
  
     计算机处于关闭状态，或处于睡眠或休眠模式。 若计算机已脱机或处于睡眠状态，则将实时更新其状态，以便你知道该计算何时可用。  
  
-   **不支持的操作系统**  
  
     计算机上的操作系统不支持远程桌面。 如果有更改，则在服务器上更新此状态可能需要多达 6 个小时。  
  
-   **连接已禁用**  
  
     计算机连接受到防火墙阻止，或者计算机上的远程桌面已由组策略禁用。 如果有更改，则在服务器上更新此状态可能需要多达 6 个小时。  
  
##  <a name="BKMK_8"></a> 查看计算机属性  
 Windows Server Essentials 仪表板的“设备”  部分将显示网络计算机列表。 该列表还提供有关每台计算机的其他信息。  
  
#### <a name="to-view-a-list-of-computers"></a>查看计算机列表  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在主导航栏上，单击“设备”。  
  
3.  仪表板将显示计算机的当前列表。  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>查看或更改计算机属性  
  
1.  在计算机列表中，选择要查看或更改其属性的帐户。  
  
2.  在中 **< 计算机名\>任务**窗格中，单击**查看计算机属性**。 这将显示计算机的“属性”  页面。  
  
3.  单击选项卡以显示该计算机的属性。  
  
4.  若要保存对计算机属性所做的任何更改，请单击“应用” 。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理远程 Web 访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [使用仪表板的用户帐户管理](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
