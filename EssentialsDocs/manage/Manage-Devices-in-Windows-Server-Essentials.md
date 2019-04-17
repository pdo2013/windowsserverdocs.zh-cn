---
title: "在 Windows Server Essentials 管理设备"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-devices-in-windows-server-essentials"></a>在 Windows Server Essentials 管理设备

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 以下部分讨论服务器，设备管理功能，并介绍了如何设置并使用你的网络上的设备：  
  
-   [通过仪表板中管理设备](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [指定用户帐户登录到特定网络计算机上的权限](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [从服务器中删除一台计算机](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [配置组策略设置文件夹重定向和安全](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [使用远程桌面会话连接到网络计算机](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [查看计算机属性](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a>通过仪表板中管理设备  
 Windows Server Essentials 便可以通过使用 Windows Server Essentials 仪表板中执行常见的管理任务。 **设备**仪表板中的页面将提供以下：  
  
-   网络计算机，其中显示列表：  
  
    -   计算机的名称  
  
    -   计算机的状态任一**联机**或**离线**  
  
    -   计算机说明  
  
    -   备份的计算机的状态  
  
    -   更新的计算机的状态  
  
    -   计算机的安全状态  
  
    -   计算机的通知状态  
  
    -   组策略计算机信息  
  
-   的详细信息窗格中与所选的计算机的其他信息  
  
-   包含一组设备管理任务，如查看电脑属性和通知、设置计算机备份并从备份中还原的文件和文件夹的任务窗格  
  
#### <a name="to-view-the-status-of-network-computers"></a>若要查看网络计算机的状态  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**设备**。  
  
3.  在网络列表窗格中查看所有计算机的状态。  
  
 下表介绍了各种计算机和 Windows Server Essentials 仪表板中提供的备份任务。 一些任务计算机特定以及这些仅显示在列表中选择一台计算机时。  
  
### <a name="computer-tasks-in-the-dashboard"></a>仪表板中的计算机任务  
  
|任务名称|描述|  
|---------------|-----------------|  
|查看电脑属性|显示所选的计算机的一般信息并使你查看计算机备份的详细信息。|  
|设置此计算机的备份|运行设置向导备份。|  
|自定义计算机的备份|打开备份属性，从中可以向所选的计算机的备份设置进行更改。|  
|启动计算机备份|启动选计算机备份。|  
|停止计算机的备份|停止选计算机备份。|  
|还原文件或文件夹的计算机|运行还原文件和文件夹向导中，以便您可以还原特定文件、文件夹或驱动器。|  
|查看计算机的通知|显示关键和其他信息警报并使你采取措施，尽可能。|  
|计算机到远程桌面|打开到所选的计算机的远程桌面连接。|  
|删除计算机|一个分离 Windows Server Essentials 仪表板从计算机的计算机向导将运行删除。|  
|自定义计算机备份和文件历史记录设置|打开备份设置页面，从中可以更改的计划备份和客户端设置文件历史记录的计算机。|  
|如何将计算机连接到服务器？|打开帮助主题介绍的步骤执行才能将计算机连接到该网络。|  
|实现组策略|适用于 Windows 8 和 Windows 7 的计算机连接到域的策略设置。|  
  
##  <a name="BKMK_2"></a>指定用户帐户登录到特定网络计算机上的权限  
 以便用户可以登录到特定网络计算机远程访问 Windows Server Essentials 网络时，你可以分配给用户帐户权限。  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>若要更改计算机的访问权限的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要更改用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**查看帐户属性**。 **属性**页面的显示的用户帐户。  
  
5.  在**计算机的访问权限**选项卡上，选择电脑的此用户可以访问远程，然后单击**确定**。  
  
##  <a name="BKMK_3"></a>从服务器中删除一台计算机  
 从的服务器上运行的 Windows Server Essentials 通过仪表板中删除一台计算机时，它不会再由服务器管理。 因此，服务器将停止创建计算机备份或监视器其健康后从网络其删除。  
  
> [!NOTE]
>  删除服务器的计算机不会从网络断开计算机。 计算机仍然可以访问网络资源，它可能之前未连接到服务器的方式相同。 若要阻止访问服务器资源计算机并断开服务器，必须从域中删除的计算机。 此外，从服务器的计算机中删除不会自动卸载接头软件或启动栏从已被的计算机。 必须手动从计算机中删除接头软件。 有关详细信息，查看部分卸载上的接头软件[连接](../use/Get-Connected-in-Windows-Server-Essentials.md)。  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>若要从网络通过仪表板中删除一台计算机  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**设备**选项卡。  
  
3.  在计算机列表中，右键单击你想要删除该网络，然后单击计算机**计算机中删除**。  
  
##  <a name="BKMK_5"></a>配置组策略设置文件夹重定向和安全  
 您可以配置组策略，并将其部署到 Windows Server Essentials 网络中的计算机中，通过使用 Windows Server Essentials 仪表板。 Windows Server Essentials 中的组策略包括文件夹重定向和安全设置，这种影响 Windows 更新、Windows Defender 和网络防火墙。  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>若要配置 Windows Server Essentials 中的组策略  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**设备**。  
  
3.  Windows Server essentials：在全球**用户任务**窗格中，单击**实现组策略**。  
  
     Windows Server essentials：在全球**设备任务**窗格中，单击**实现组策略**。  
  
4.  打开实现组策略向导。  
  
5.  在**启用文件夹重定向组策略**页面向导中，你可以选择你想要将重定向的用户文件夹。  
  
6.  在**启用安全策略设置**页面向导中，你可以选择要启用组策略设置**Windows 更新**，**Windows Defender**，并**网络防火墙**。  
  
7.  单击**完成**实现组策略设置。  
  
##  <a name="BKMK_7"></a>使用远程桌面会话连接到网络计算机  
 若要远程访问你的 Windows Server Essentials 网络计算机，当你离开办公室，使用你的 Web 浏览器登录到你的组织 s 远程网站访问的网站，并在**计算机**选项卡上，单击计算机的名称。  
  
 **状态**列向你显示是否你可以在你的网络连接到计算机，并且可以包含以下值：  
  
-   **可用**  
  
     计算机处于打开状态，并且可以为远程连接。 即使你看到此状态时，你仍然可能无法如果第三方防火墙阻止了该连接连接到此计算机。  
  
-   **离线状态或休眠状态**  
  
     计算机处于关闭状态，或处于睡眠或休眠模式。 如果离线状态或休眠状态的计算机，以便你可以知道计算机可用实时更新的状态。  
  
-   **不受支持的操作系统**  
  
     在计算机上的操作系统不支持远程桌面。 它可能需要最多 6 小时这种状态要更新服务器上，如果进行了更改。  
  
-   **连接已禁用**  
  
     计算机连接已被阻止的防火墙，或在计算机上，或通过组策略禁用远程桌面。 它可能需要最多 6 小时这种状态要更新服务器上，如果进行了更改。  
  
##  <a name="BKMK_8"></a>查看计算机属性  
 **设备**Windows Server Essentials 仪表板部分显示的网络计算机列表。 列表中还提供了有关每个计算机的其他信息。  
  
#### <a name="to-view-a-list-of-computers"></a>若要查看的计算机列表  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在主要导航栏中，单击**设备**。  
  
3.  仪表板中显示计算机的当前的列表。  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>若要查看或更改计算机的属性  
  
1.  在计算机的列表中，选择你想要查看或更改属性的帐户。  
  
2.  在**< Computername\ > 任务**窗格中，单击**查看电脑属性**。 **属性**出现这两台计算机的页面。  
  
3.  单击选项卡显示有关该计算机属性。  
  
4.  若要保存你对电脑属性进行任何更改，请单击**应用**。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理 Web 远程访问](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理用户帐户使用仪表板](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
