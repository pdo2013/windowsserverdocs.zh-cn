---
title: 在 Windows Server Essentials 中进行连接
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 05/07/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 04d09574046474da5bee4437628ade9646cf58ca
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866936"
---
# <a name="get-connected-in-windows-server-essentials"></a>在 Windows Server Essentials 中进行连接

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials，Windows Server 2012 Essentials

 可以通过使用连接器软件将计算机连接到 Windows Server Essentials 服务器。 在使用“将计算机连接到服务器”向导来将计算机连接到服务器时安装连接器软件。 可以通过键入**http：//< servername\>/connect**（其中 **\> < servername**是服务器的名称）来启动此向导。  

 本主题内容：  


-   [准备将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [使用连接器软件将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [使用快速启动板](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [准备将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [使用连接器软件将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [使用快速启动板](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  


##  <a name="BKMK_A"></a>准备将计算机连接到服务器  
 本部分讨论了连接器软件、受 Windows Server Essentials 支持的操作系统、必须在将计算机连接到服务器之前完成的先决条件任务，以及在运行连接器软件时服务器对计算机所做的更改。  


-   [连接器软件概述](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [将计算机连接到服务器的先决条件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [将 Mac 计算机连接到网络的先决条件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [客户端计算机支持的操作系统](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [服务器对客户端计算机所做的更改](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [网络用户名和密码信息](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [服务器管理员帐户](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [从 Windows 域中删除计算机](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a>连接器软件概述  
 Windows Server Essentials 操作系统的连接器软件可将网络中的计算机连接到 Windows Server Essentials 服务器。 当你将计算机连接到服务器时，连接器软件允许你自动备份计算机并监视其运行状况。 连接器软件还允许你配置并远程管理 Windows Server Essentials 服务器。 在将客户端计算机连接到服务器时安装连接器软件。 有关将客户端计算机连接到 Windows Server Essentials 服务器的详细说明，请参阅本主题后面的[将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

-   [连接器软件概述](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [将计算机连接到服务器的先决条件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [将 Mac 计算机连接到网络的先决条件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [客户端计算机支持的操作系统](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [服务器对客户端计算机所做的更改](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [网络用户名和密码信息](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [服务器管理员帐户](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [从 Windows 域中删除计算机](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a>连接器软件概述  
 Windows Server Essentials 操作系统的连接器软件可将网络中的计算机连接到 Windows Server Essentials 服务器。 当你将计算机连接到服务器时，连接器软件允许你自动备份计算机并监视其运行状况。 连接器软件还允许你配置并远程管理 Windows Server Essentials 服务器。 在将客户端计算机连接到服务器时安装连接器软件。 有关将客户端计算机连接到 Windows Server Essentials 服务器的详细说明，请参阅本主题后面的[将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  


###  <a name="BKMK_2"></a>将计算机连接到服务器的先决条件  
 在将计算机连接到网络之前，必须先满足以下要求：  

-   Windows Server Essentials 的安装已完成，并且该服务器处于运行状态。 如果连接器软件无法与服务器通信，则它将退出安装。  


-   客户端计算机运行受支持的操作系统。 有关详细信息，请参阅 [Supported operating systems for client computers](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)。


-   客户端计算机必须有效连接到 Internet。  

-   当客户端计算机与服务器位于同一网络上时，客户端计算机还与运行 Windows Server Essentials 的服务器位于同一 IP 子网上。  

-   在客户端计算机上安装了 .NET Framework 4.5。 连接器软件自动将它安装在计算机上。  

-   客户端计算机满足以下最低系统要求：  

    -   1.4 GHz 或更快的处理器  

    -   1 GB RAM 或更多  

    -   1 GB 的可用硬盘驱动器空间（安装后会释放此磁盘上的一个分区）  

-   使用 NTFS 文件系统格式化启动分区（即，安装 Windows 操作系统所在的磁盘分区）。  

-   计算机包含的字符不能超过 15 个。  

-   计算机名称不能包含下划线 (_)。  

-   计算机的日期和时间设置与服务器上的设置一致。  

-   在任何给定时间，都只能将客户端计算机连接到一台 Windows Server Essentials 服务器。  

-   已加入到另一个 Active Directory 域的客户端计算机无法加入 Windows Server Essentials 域。  

> [!NOTE]
> 
>  在 Windows Server Essentials 或 Windows Server Essentials 的本地客户端部署中，你可以将计算机连接到服务器，而无需将计算机添加到 Windows Server Essentials 域。 此方法并非适用于所有受支持的客户端操作系统，将计算机连接到域所需的功能（例如组策略和虚拟专用网络 (VPN)）也不可用。 有关要求和说明，请参阅 [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  

 有关将计算机连接到运行 Windows Server Essentials 的服务器的逐步说明，请参阅 [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

>  在 Windows Server Essentials 或 Windows Server Essentials 的本地客户端部署中，你可以将计算机连接到服务器，而无需将计算机添加到 Windows Server Essentials 域。 此方法并非适用于所有受支持的客户端操作系统，将计算机连接到域所需的功能（例如组策略和虚拟专用网络 (VPN)）也不可用。 有关要求和说明，请参阅 [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  

 有关将计算机连接到运行 Windows Server Essentials 的服务器的逐步说明，请参阅 [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  


###  <a name="BKMK_3"></a>将 Mac 计算机连接到网络的先决条件  
 在将 Mac 计算机连接到网络之前，必须先满足以下要求：  

-   服务器操作系统的安装已完成，并且该服务器处于运行状态。 如果连接器软件无法与服务器通信，则不会安装该连接器软件。  

-   运行 Mac OS X 10.5 (Leopard) 或更高版本的计算机。  

-   该计算机与服务器位于同一 IP 子网上。  

-   该计算机必须有效连接到 Internet。  

-   确保该计算机满足以下最低系统要求：  

    -   1.4 GHz 或更快的处理器  

    -   1 GB RAM 或更多  

    -   1 GB 的可用硬盘驱动器空间（安装后会释放此磁盘上的一个分区）  

-   在任何给定时间，都只能将客户端计算机连接到一台服务器。  

###  <a name="BKMK_4"></a>客户端计算机支持的操作系统  
 Windows Server Essentials 为所有受支持的客户端计算机提供了同一组功能。 这些功能包括域加入、快速启动板和客户端运行状况通知。  

> [!IMPORTANT]
>  Windows Server Essentials 不支持将运行家庭版、简易版或媒体中心版 Windows 的计算机加入到域中。 此外，不能使用远程 Web 访问连接到这些计算机。  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  本部分适用于运行 Windows Server Essentials 的服务器，或适用于运行 windows server 2012 R2 的 windows server R2 Standard 或 Windows Server 2012 R2 Datacenter （安装了 Windows Server Essentials Experience 角色）的服务器。 支持以下计算机操作系统：  

 **Windows 7 操作系统**  

- Windows 7 家庭普通版 SP1 （x86 和 x64）  

- Windows 7 家庭高级版 SP1 （x86 和 x64）  

- Windows 7 Professional SP1 （x86 和 x64）  

- Windows 7 旗舰版 SP1 （x86 和 x64）  

- Windows 7 Enterprise SP1 （x86 和 x64）  

- Windows 7 简易版 SP1 （x86）  

  **Windows 8 操作系统**  

- Windows 8  

- Windows 8 专业版  

- Windows 8 企业版  

  **Windows 8.1 操作系统**  

- Windows 8.1  

- Windows 8.1 专业版  

- Windows 8.1 企业版  

  **Windows 10 操作系统**  

- Windows 10  

- Windows 10 专业版  

- Windows 10 企业版  

- Windows 10 教育版  

  **Mac 客户端计算机**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

- Mac OS X v10.8 Mountain Lion  

> [!NOTE]
>  你可以从 Windows Server Essentials 仪表板查看 Mac 计算机的运行状况和备份状态。 但是，不能配置计算机备份，或从仪表板中开始备份。 此外，不能使用远程 Web 访问连接 Mac 计算机。  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  本节适用于运行 Windows Server Essentials 的服务器。 支持以下计算机操作系统：  

 **Windows 7 操作系统**  

- Windows 7 家庭普通版（x86 和 x64）  

- Windows 7 家庭高级版（x86 和 x64）  

- Windows 7 专业版（x86 和 x64）  

- Windows 7 旗舰版（x86 和 x64）  

- Windows 7 企业版（x86 和 x64）  

- Windows 7 简易版（x86）  

  **Windows 8 操作系统**  

- Windows 8  

- Windows 8 专业版  

- Windows 8 企业版  

  **Windows 10 操作系统**  

- Windows 10  

- Windows 10 专业版  

- Windows 10 企业版  

- Windows 10 教育版  

  **Mac 客户端计算机**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

> [!NOTE]
>  你可以从 Windows Server Essentials 仪表板查看 Mac 计算机的运行状况和备份状态。 但是，不能配置计算机备份，或从仪表板中开始备份。 此外，不能使用远程 Web 访问连接 Mac 计算机。  

###  <a name="BKMK_5"></a>服务器对客户端计算机所做的更改  
 当你将计算机连接到服务器时，Windows Server Essentials 软件会对计算机进行大量更改，以便计算机和服务器可以协同工作。  

 该软件将执行以下操作：  

-   在计算机上安装连接器软件  

-   在计算机上安装 Microsoft .NET Framework 4.5（如果尚未安装）  

-   在计算机的桌面上创建仪表板和快速启动板的快捷方式  

-   在计算机上配置 Windows 防火墙端口以使以下功能生效：  

    -   核心网络  

    -   远程桌面服务  

-   对计算机进行以下更改以改进备份：  

    -   创建计划任务以运行自动备份  

    -   安装使用服务器管理备份操作的服务  

    -   安装在文件和文件夹存储过程中使用的虚拟设备驱动程序  

-   安装运行状况代理以检测问题并创建相应的警报通知  

-   对于重复运行状况评估，在计算机上创建计划任务，并同步运行状况警报定义。  

-   将服务添加到计算机，使用该计算机与服务器以及其他 Windows Server Essentials 功能进行通信。  

-   在运行 Windows 客户端的客户端计算机中打开 TCP 端口 3389，以允许运行远程桌面服务  

-   如果在 Windows Server Essentials 上启用了 vpn 功能，则在客户端计算机上部署 VPN 并提供一次单击体验; 如果在 Windows Server Essentials 上启用了 VPN 功能，则提供自动连接体验  


 有关将计算机连接到服务器的信息，请参阅 [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

###  <a name="BKMK_6"></a>网络用户名和密码信息  
 你可以从管理服务器的人员处获取网络用户名和密码信息。 你可以使用这些凭据将计算机连接到服务器并访问服务器中的信息。  

###  <a name="BKMK_6"></a>网络用户名和密码信息  
 你可以从管理服务器的人员处获取网络用户名和密码信息。 你可以使用这些凭据将计算机连接到服务器并访问服务器中的信息。 


 如果你是服务器管理员，则可以通过从仪表板的“用户” 选项卡添加用户帐户来创建网络凭据。 有关用户帐户的详细信息，请参阅[使用仪表板管理用户帐户](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)。  

###  <a name="BKMK_7"></a>服务器管理员帐户  
 你必须能够提供网络管理员帐户名称和密码，才能安装连接器软件。 网络管理员帐户使用户能够管理组织的局域网并帮助管理和维护网络设备（例如交换机和路由器）。  

 可通过使用网络管理员帐户执行的任务包括：  

- 安装联网应用程序和其他软件  

- 管理服务器上的存储  

- 分发软件更新  

- 执行日常备份  

- 监视网络上的日常活动  

  在安装了 Windows Server Essentials Experience 角色的 Windows Server Essentials、Windows Server Essentials 和 Windows Server 2012 R2 中，你可以向任何用户帐户分配网络管理员访问级别。 这将授予执行网络管理员任务所需的权限。 当用户分配网络管理员访问级别时，将为需要管理员权限的任何任务打开“用户访问控制”提示。  

###  <a name="BKMK_8"></a>从 Windows 域中删除计算机  
 若要从其域中删除计算机，系统将提示你输入域帐户的用户名和密码。  

##### <a name="to-remove-a-computer-from-a-windows-domain"></a>从 Windows 域中删除计算机  

1.  单击“开始”、右键单击“计算机”，然后单击“属性”。  

2.  在“计算机名、域和工作组设置”下，单击“更改设置”。  

    > [!NOTE]
    >  如果系统提示你输入管理员密码或确认信息，请键入域密码或提供确认信息。  

3.  在“系统属性” 对话框中，单击“计算机名” 选项卡，然后单击“更改”。  

4.  在“计算机名/域更改”对话框中的“隶属于”下，单击“工作组”，然后执行以下操作之一：  

    1.  若要加入现有工作组，请键入要加入的工作组的名称，然后单击“确定”。  

    2.  若要创建工作组，请键入要创建的工作组的名称，然后单击“确定”。  

        > [!NOTE]
        >  将从域中删除你的计算机，并将禁用该域上的计算机帐户。  

##  <a name="BKMK_B"></a>使用连接器软件将计算机连接到服务器  
 本部分提供了对将帮助你执行以下操作的过程和信息的访问权限：安装连接器软件、将计算机连接到服务器，以及解答将计算机连接到服务器的问题。  


-   [将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [在不加入域的情况下将计算机连接到 Windows Server Essentials 服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [安装连接器软件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [手动移动计算机数据和设置](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [在计算机部署期间传输多个用户配置文件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [卸载连接器软件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [断开计算机与服务器的连接或将计算机重新连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [备份如何处理睡眠和休眠模式](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [在不加入域的情况下将计算机连接到 Windows Server Essentials 服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [安装连接器软件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [手动移动计算机数据和设置](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [在计算机部署期间传输多个用户配置文件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [卸载连接器软件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [断开计算机与服务器的连接或将计算机重新连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [备份如何处理睡眠和休眠模式](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  


###  <a name="BKMK_9"></a>将计算机连接到服务器  
 当你将计算机连接到运行 Windows server essentials 或 Windows Server 2012 R2 且安装了 Windows Server Essentials Experience 角色的服务器时，请确保客户端计算机具有与 Internet 的有效连接。  

 在所有客户端计算机上完成以下过程以将它们连接到服务器。  

 若要完成该过程，你将需要以下信息：  

-   将在新网络上使用该计算机的用户的用户名和密码  

-   计算机的本地管理员帐户的用户名和密码  

> [!NOTE]
> 
>  在 Windows Server Essentials 或 Windows Server Essentials 的本地客户端部署中，你可以将计算机连接到服务器，而无需将计算机添加到 Windows Server Essentials 域。 此方法并非适用于所有受支持的客户端操作系统，将计算机连接到域所需的功能（例如组策略和虚拟专用网络 (VPN)）也不可用。 有关要求和说明，请参阅 [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  
> 
>  在 Windows Server Essentials 或 Windows Server Essentials 的本地客户端部署中，你可以将计算机连接到服务器，而无需将计算机添加到 Windows Server Essentials 域。 此方法并非适用于所有受支持的客户端操作系统，将计算机连接到域所需的功能（例如组策略和虚拟专用网络 (VPN)）也不可用。 有关要求和说明，请参阅 [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  


##### <a name="to-connect-a-client-computer-to-the-server"></a>将客户端计算机连接到服务器的步骤  

1.  登录要连接到服务器的计算机。  

    > [!NOTE]
    >  如果此计算机上具有多个用户帐户，则使用包含你要在将计算机连接到服务器后继续保留的文档、图片和个人首选项的用户帐户进行登录。  

2.  打开 Internet 浏览器，如 Internet Explorer。  

3.  在地址栏中，键入**http：//<\>servername/Connect**，然后按 enter。  

    > [!NOTE]
    >  如果计算机位于 Windows Server Essentials 网络之外的远程位置，若要运行 "将计算机连接到服务器" 向导，请在 web 浏览器的地址栏中键入 http：/ **/< domainname\>/connect** （其中 < 域\>是组织的域名）。 你可以从网络管理员处获得你的域名信息。  

4.  此时将显示 **“将你的计算机连接到服务器”** 页面。 执行下列操作之一：  

    -   对于运行 Windows 操作系统的计算机，单击“下载适用于 Windows 的软件”。  

    -   对于运行 Mac OS X 或更高版本的计算机，单击“下载适用于 Mac 的软件”。  

5.  在文件下载安全警告消息中，单击 **“运行”** 。  

6.  如果显示“用户帐户控制”消息，请单击“是” ，或在出现提示后键入本地用户名和密码。  

7.  将显示连接计算机与服务器向导。 请执行以下操作以完成该向导：  

    1.  接受最终用户许可协议。  

    2.  在“查找服务器”页上，自动检测本地网络中的服务器，选择你想要连接到的服务器。 或者，如果有这些信息，可以手动输入服务器的名称或域地址。  

    3.  在 **“请输入新的网络用户名和密码”** 页中，执行以下操作：  

        -   如果这是连接到服务器的首台计算机，并且你将使用该计算机管理服务器，请使用你在设置期间创建的管理员帐户。  

        -   对于所有其他计算机，首先使用仪表板在该服务器上创建网络用户帐户。 根据使用该计算机的用户执行的任务，创建具有管理员或标准用户权限的用户帐户。  

    4.  如果计算机运行的是 Windows 8、Windows 8.1 或 Windows 10，请跳过此步骤。 如果计算机运行的是 Windows 7，并且其中包含你要在将该计算机加入新网络后继续保留的文档、图片和个人首选项（如桌面背景、屏幕保护程序或 Internet Explorer 收藏夹），请在该向导的“选择是否要移动现有数据和设置” 页上，选择“将数据和设置移动到新的网络用户帐户”。  

    5.  在“选择是否要唤醒该计算机以创建备份”页上，选择是否要自动唤醒该计算机以创建备份。  

    6.  按照此向导中其余的说明将你的计算机加入网络。  

8.  将你的计算机加入该网络后，使用新用户名和密码登录该计算机。  

    > [!NOTE]
    >  当首次使用你的网络帐户登录运行 Windows 8 的计算机时，连接到该服务器后，将显示从旧用户帐户迁移文件和应用程序的说明。 按照 **“如何从我的旧用户帐户迁移文件和应用程序?”** 页面上的说明，将所有文件和应用程序迁移到此网络用户帐户。  

9. 计算机成功连接到服务器之后，连接器 TrayApp 和服务器仪表板的快捷方式将出现在 "开始" 菜单上，可按如下所示使用（如果计算机运行的是 Windows 8、Windows 8.1 或 Windows 10、仪表板和连接器将从计算机的 "开始" 屏幕中获取 TrayApp：  

    -   如果你的计算机运行的是 Windows 8、Windows 8.1 或 Windows 10，则仪表板和连接器 TrayApp 将可作为应用进行搜索。  

    -   通过连接器 TrayApp，你可以启用或禁用“请让我保持远程连接状态” 功能。 还可以双击 TrayApp 来启动快速启动板。 通过快速启动板，你可以访问共享文件夹快捷方式、配置计算机备份、处理警报以及打开远程 Web 访问网站。  

    -   通过“仪表板”链接，你可以管理服务器。  

###  <a name="BKMK_10"></a>在不加入域的情况下将计算机连接到 Windows Server Essentials 服务器  
 本主题介绍如何将 Windows 7、Windows 8、Windows 8.1 或 Windows 10 计算机添加到 Windows Server Essentials 网络，而无需将计算机加入到本地客户端部署中的 Windows Server Essentials 域。 Windows Server Essentials 和 Windows Server Essentials 支持此连接方法。  

 这是常用方法的替代方法，需要将计算机加入到 Windows Server Essentials 域。 借助该方法，如果计算机位于另一个域中，则必须从该域中删除此计算机，然后才能将它添加到 Windows Server Essentials 域中。  

#### <a name="feature-limits"></a>功能限制  
 当客户端计算机未添加到 Windows Server Essentials 域时，某些功能将受到限制：  

-   需要将计算机加入域的所有功能（包括域凭据、组策略和虚拟专用网络（Vpn））均不可用。  

-   将计算机加入到域所需的任何第三方加载项都无法正常工作。  

-   此方法不能用于将外部计算机连接到服务器。  

#### <a name="prerequisites"></a>先决条件  

-   计算机必须具有到本地网络的物理连接。  

-   必须在计算机上安装以下操作系统之一：  

    -   Windows 10 专业版，Windows 10 企业版  

    -    Windows 8.1 Pro、Windows 8.1 Enterprise、Windows 8 专业版、Windows 8 企业版  

    -    Windows 7 专业版（x86 和 x64）、Windows 7 企业版（x86 和 x64）、Windows 7 旗舰版（x86 和 x64）  


-   计算机必须满足 Windows Server Essentials 中客户端计算机的所有其他要求。 有关详细信息，请参阅[将计算机连接到服务器的先决条件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)。  


-   若要在不加入域的情况下启用连接，你必须使用属于本地 Administrators 组成员的帐户登录计算机。  

-   若要将计算机连接到 Windows Server Essentials 服务器，你将需要以下帐户信息：  

    -   Windows Server Essentials 上具有管理员权限的帐户（你的帐户）。  

    -   将使用该计算机的用户的域帐户的用户名和密码。 域帐户还必须在 Windows Server Essentials 服务器上具有管理员权限。  

#### <a name="connect-to-a-windows-server-essentials-network"></a>连接到 Windows Server Essentials 网络  
 在验证已满足所有先决条件后，将计算机连接到 Windows Server Essentials 网络。  

###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>将不同域中的计算机连接到 Windows Server Essentials 网络  

1.  使用属于本地 Administrators 组的成员的帐户登录客户端计算机。  

2.  使用管理员权限打开命令提示符。  

    -   在 Windows 10 中，单击 "**开始**" 按钮，选择 "**所有应用** -> " "**Windows 系统工具** -> " "**命令提示符**"，右键单击 "命令提示符"，然后单击 "以**管理员身份运行**"。  

    -   在 Windows 8 中，在 "**开始**" 页上键入**command** ，然后按 enter。 在结果中，右键单击“命令提示符”，然后单击“以管理员身份运行”。  

    -   在 Windows 7 中，在 "**开始**" 菜单上的 "搜索" 框中输入**命令**，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。  

3.  在命令提示符下，键入以下命令，然后按 Enter：  

    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  


4.  完成 [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)中的步骤。  


####  <a name="BKMK_SecondServer"></a>将第二台服务器加入到网络  

###### <a name="to-join-a-second-server-to-the-network"></a>将第二台服务器加入到网络  

1.  登录你想要连接到 Windows Server Essentials 网络的服务器。  

2.  打开 Internet 浏览器，然后在地址栏中键入 http：/ **/< servername\>/Connect**，其中 *< servername\>* 是运行 Windows server Essentials 的服务器的名称，然后按 enter。  

3.  如果在尝试连接到 Windows Server Essentials 网络的服务器上启用了 Internet Explorer 增强的安全配置，则完成以下操作；否则，跳过此步骤。  

    1.  若要接受阻止消息，请单击“关闭”。  

    2.  将**http：//< servername\>/Connect**网站添加到受信任的网站，如下所示：  

        1.  在浏览器导航窗格中，单击“工具”，然后单击“Internet 选项”。  

        2.  单击“安全” 选项卡，然后单击“受信任站点”。  

        3.  单击“站点”。  

        4.  该网站应该显示在“将该网站添加到区域” 字段中。 单击**添加**。  

        5.  单击“关闭”，然后单击“确定”。  

    3.  刷新网页。  


    4.  若要将第二台服务器连接到运行 Windows Server Essentials 的服务器，请按照[将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)中的说明操作。  


~~~
> [!NOTE]
>  Connecting a second server to a server running Windows Server Essentials differs from connecting to a client computer as follows:  
>   
>  -   There is no user profile migration; therefore, the current profile will not be migrated.  
> -   You cannot back up the second server by using client computer backup, and there is no option to wake up the computer for backup.  
~~~

 将第二台服务器加入到运行 Windows Server Essentials 的服务器之后，将向连接的服务器提供以下功能：  

- 更新与警报状态功能的工作方式与其他客户端计算机相同。  

- 联机与脱机功能的工作方式与其他客户端计算机相同。  

- 你可以使用远程 Web 访问连接到你的第二台服务器。  

- 第二台服务器将包含在运行状况报告中，因为 Windows Server Essentials 将生成与此服务器相关的警报。  

  从运行 Windows Server Essentials 的服务器中管理第二台服务器不同于管理其他客户端计算机，如下所示：  

- 没有用于 TrayApp、快速启动板和仪表板客户端的入口点。  

- 第二台服务器将列在“设备” 选项卡的“服务器” 组内。  

- 因为第二台服务器不支持客户端计算机备份，因此备份状态将显示为“不支持”。 此外，如果你选择第二台服务器并右键单击，则没有任何备份，并还原为第二台服务器显示的相关任务。  

- 如果选择第二台服务器，然后单击 "**查看服务器属性**" 任务，则服务器的 "属性" 页上不会显示任何 "**备份**" 选项卡。  

- 由于 Windows Server 操作系统上没有安全中心，因此第二台服务器的安全状态将显示为 "**不适用**"。  

- 第二台服务器的组策略状态显示为 "**不适用**"。  

###  <a name="BKMK_11"></a>安装连接器软件  
 在使用“将计算机连接到服务器”向导来将计算机连接到服务器时安装 Windows Server Essentials 中的连接器软件。 可以通过在 web 浏览器的地址栏中键入**http\>：//< ServerName/connect**来启动此向导（其中 *<\> ServerName*是服务器的名称）。  

> [!NOTE]
>  如果计算机位于远程位置，若要运行 "将计算机连接到服务器" 向导，请在 web 浏览器的地址栏中键入 http：/ **/< domainname\>/connect** （其中 *< 域\>* 是你的域名组织）。 你可以从网络管理员处获得你的域名信息。  

 连接器软件将执行以下操作：  

-   将计算机连接到 Windows Server Essentials  

-   在夜间自动备份计算机（如果你将服务器配置为创建客户端备份）  

-   帮助管理员监视计算机的运行状况  

-   使你能够从家庭计算机配置并远程管理 Windows Server Essentials  


 有关将计算机连接到 Windows Server Essentials 服务器的逐步说明，请参阅 [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。   


###  <a name="BKMK_12"></a>手动移动计算机数据和设置  
  Windows Server Essentials 和 Windows Server Essentials 仅支持用于运行 Windows 7 操作系统的客户端计算机的用户配置文件迁移。 当你将基于 Windows 7 的计算机连接到服务器时，“将计算机连接到服务器”向导可以自动迁移用户配置文件。  

 将 Windows 8、Windows 8.1 或 Windows 10 计算机连接到服务器时，无法自动传输用户配置文件。 但是，在 Windows 8 计算机上，你可以使用 Windows 轻松传送将数据和设置从原始本地用户传输到加入域的计算机。 若要实现这一点，你必须同时是 Windows 8 源计算机和 Windows 8 目标计算机上的管理员。 有关使用“Windows 轻松传送”来传输文件和设置的信息，请参阅 Microsoft 知识库中的 [文章 2735227](https://support.microsoft.com/kb/2735227) 。  

###  <a name="BKMK_Transfer"></a>在计算机部署期间传输多个用户配置文件  
 在将运行 Windows 7 或 Windows 7 SP1 操作系统的计算机连接到 Windows Server Essentials 服务器之前，你必须先在服务器上创建相应的网络用户帐户，以便传输多个本地用户配置文件。 有关创建网络用户帐户的详细信息，请参阅 [Add a user account](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。  

 用户配置文件迁移仅在运行 Windows 7 （适用于 Windows Server Essentials）或 Windows 7 SP1 （适用于 Windows Server Essentials）的计算机上受支持。 当你使用“将计算机连接到服务器”向导将计算机连接到 Windows Server Essentials 服务器时，系统将向你提供一个选项，用于将旧的本地用户帐户的用户数据和设置移动到新的网络用户帐户中。 为此，在该向导的“移动现有用户数据和设置”页面上，将网络用户帐户映射到存在于计算机上的本地用户帐户，以便传输位于客户端计算机上的多个用户配置文件。  

###  <a name="BKMK_13"></a>卸载连接器软件  
 你可以使用“控制面板”从计算机中卸载连接器软件。 如果连接器软件出现问题或你需要安装较新版本的连接器软件，则通常将执行此操作。 必须以管理员身份登录计算机，才能完成此过程。  

> [!IMPORTANT]
>  如果升级客户端计算机上的操作系统，将自动卸载连接器软件。 完成升级后，必须重新安装连接器软件。 首选的方法是，在升级操作系统之前卸载连接器软件。 在完成升级后卸载连接器软件仍然可行；但是，它可能导致客户端计算机和服务器的状态不一致，直到卸载并重新安装连接器软件为止。  

##### <a name="to-uninstall-connector-software-from-a-computer"></a>从计算机中卸载连接器软件  

1.  在运行 Windows 7、Windows 8、Windows 8.1 或 Windows 10 的计算机上，打开 **"控制面板**"，然后在 "**程序**" 部分中，单击 "**查看已安装的更新**"。  

2.  从已安装的程序列表中，选择“Windows Server Essentials 连接器”，然后单击“卸载”。  

3.  在警告页面上，单击“是”。  

4.  如果出现“用户帐户控制” 窗口，请单击“允许”。  

5.  在 Windows Server Essentials 中，如果显示 "Windows Server Essentials 连接器" 页面建议关闭快速启动板，请单击 **"确定"** 。  

6.  等待程序卸载。 删除该软件后，已安装的程序或更新列表中将不再显示“Windows Server Essentials 连接器”。 此外，计算机的桌面上将不再显示快速启动板和仪表板的快捷方式。  

> [!NOTE]
> - 卸载连接器软件不会从显示在仪表板的“设备” 选项卡上的计算机列表中删除该计算机。 若要从仪表板中删除计算机，请参阅 [Remove a computer from the server](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)。  
>   -   卸载连接器软件时，不会删除客户端计算机上已映射到服务器的共享文件夹。 你必须手动删除映射到服务器的共享文件夹。  
> 
> -   卸载连接器软件不会使计算机脱离原始域。 你必须手动从域中脱离计算机。 有关说明，请参阅 [Remove a computer from a Windows domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)。  


###  <a name="BKMK_14"></a>断开计算机与服务器的连接或将计算机重新连接到服务器  
 若要从服务器中断开计算机连接，你必须完成以下步骤：  


1. 使用“控制面板”从计算机中卸载连接器软件。 有关逐步说明，请参阅 [Uninstall the Connector software](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。   


2. 使计算机脱离 Windows Server Essentials 域，并将其加入到工作组。 有关将 Windows 加入工作组的逐步说明，请参阅 [加入或创建工作组](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup)。  

3. 使用仪表板从服务器中删除计算机 有关逐步说明，请参阅 [Remove a computer from the server](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)。  

   若要将计算机重新连接到之前从 Windows Server Essentials 服务器网络断开连接的服务器，你必须完成以下步骤：  


4. 使用“控制面板”从计算机中卸载连接器软件。 有关逐步说明，请参阅 [Uninstall the Connector software](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。  

5. 使计算机脱离 Windows Server Essentials 域，并将其加入到工作组。 有关将 Windows 加入工作组的逐步说明，请参阅 [加入或创建工作组](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup)。  

6. 使用连接计算机向导将计算机连接到服务器 有关分步说明，请参阅[将计算机连接到服务器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

###  <a name="BKMK_Sleep"></a>备份如何处理睡眠和休眠模式  
 如果你在将计算机连接到服务器时选择“唤醒此电脑以供备份”选项，计算机每天将根据备份计划所指定的时间自动从睡眠或休眠模式中唤醒，以便对该计算机进行备份。 完成备份后，计算机将基于其电源管理设置返回到睡眠或休眠模式。 如果不选择此选项，则服务器在计算机处于睡眠或休眠状态时不会备份计算机。 有关详细信息，请参阅[管理客户端备份](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)。  

##  <a name="BKMK_C"></a>使用快速启动板  
 你可以使用快速启动板访问 Windows Server Essentials 服务器中的共享资源、执行计算机备份以及响应系统运行状况警报。  

-   [快速启动板概述](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  

-   [将快速启动板用于 Mac 计算机](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  

## <a name="see-also"></a>请参阅  

-   [排查将计算机连接到服务器的问题](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  

-   [管理用户帐户](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  


-   [使用共享文件夹](Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [远程工作](Work-Remotely-in-Windows-Server-Essentials.md)  

-   [播放数字媒体](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [使用共享文件夹](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [远程工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)  

-   [播放数字媒体](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

