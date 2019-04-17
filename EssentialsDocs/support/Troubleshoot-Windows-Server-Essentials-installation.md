---
title: "解决 Windows Server Essentials 安装"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 293b392203269a65efffcefb3744bedc659f71c9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>解决 Windows Server Essentials 安装

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本主题提供的安装 Windows Server Essentials 时出现问题的疑难解答。 以下几个方面提供指南：  
  

-   [一般疑难解答步骤](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [疑难解答驱动程序问题](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [一般疑难解答步骤](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [疑难解答驱动程序问题](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  对于从 Windows Server Essentials 社区的最新的疑难解答信息，我们建议你访问[Windows Server Essentials 论坛](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 论坛是一个不错的进行搜索以获取帮助，或者提出问题。  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a>一般疑难解答步骤  
 如果 Windows Server Essentials 安装将失败，可以采取这些步骤以帮助标识导致失败的问题。  
  
> [!IMPORTANT]
>  非常重要，您不手动重新启动您的服务器时安装 Windows Server Essentials。 服务器自动重新启动多次设置和初始配置过程。 如果你手动以前重新启动服务器你看到**服务器安装成功**消息，可能已中断安装并导致其无法。  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>若要确定中安装的 Windows Server Essentials 失败的问题  
  
1.  验证你的服务器硬件满足最低要求。 硬件要求有关的信息，请参阅[Windows Server Essentials 系统要求](../get-started/system-requirements.md)。  
  
2.  如果你从 MSDN 收到 Windows Server Essentials 安装 DVD，DVD 通过验证是否有效检查 SHA1 之和。 有关详细信息，请参阅[可用性和文件校验完整性验证程序实用程序的描述](https://go.microsoft.com/fwlink/?LinkId=220495)(https://go.microsoft.com/fwlink/?LinkId=220495)。  
  
3.  验证的服务器上的网络适配器通过网络电缆连接到路由器。  
  
4.  如果该服务器具有多个网络适配器，确认已启用该只有一个网络适配器。  
  
    > [!IMPORTANT]
    >  不要拔下网络电缆或重新安装 Windows Server Essentials 同时启动路由器。  
  
5.  在查看"安装服务器和部署"[发布 Windows Server Essentials 文档](../get-started/release-notes.md)  
  
6.  如果你收到错误消息时出错设置在安装期间，服务器使用服务器恢复 DVD 和你的硬件制造商提供的说明进行操作服务器恢复到出厂默认设置。  
  
##  <a name="BKMK_TroubleshootDrivers"></a>疑难解答驱动程序问题  
 安装 Windows Server Essentials 时需要手动安装驱动程序的存储控制器最常见的问题。 Windows 包括适用于许多存储控制器，驱动程序，但它可能包括你的特定硬件的驱动程序。  
  
 你可能需要手动安装网络卡特定的硬件的驱动程序。  
  
###  <a name="BKMK_StorageDrivers"></a>添加存储控制器的驱动程序  
 如果你的硬件要求，将不包括 Windows Server Essentials 与的存储驱动程序，请使用以下信息以完成安装。  
  
 如果你在安装期间看到以下消息时，你将需要手动添加存储控制器的驱动程序：  
  
 **Windows Server 安装错误**  
  
 找不到硬盘驱动器能够承载 Windows Server Essentials。 你想要加载额外的存储驱动程序？  
  
 使用下面的过程中安装存储控制器驱动程序。  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>若要手动安装存储控制器驱动程序  
  
1.  找到你存储控制器的驱动程序。 这些制造商提供的硬件，并且还可能在制造商的网站上提供。  
  
2.  创建一个文件夹上软盘调用驱动程序或 U 盘，和的驱动程序复制到文件夹中。  
  
3.  连接到计算机的软盘驱动器或 U 盘驱动程序  
  
4.  Windows Server Essentials DVD 从计算机启动。  
  
     如果缺少任意存储控制器驱动程序，将显示 Windows Server Essentials 安装错误对话框。  
  
5.  在 Windows Server Essentials 安装错误对话框中，单击**是**加载额外的存储驱动程序。  
  
6.  在**请选择驱动程序的 inf 文件**提示，导航到的驱动程序文件夹中的磁盘或 U 盘上的.inf 文件、选择该文件、右键单击文件的名称，然后单击**打开**。 这将加载驱动程序。  
  
    > [!NOTE]
    >  尝试加载文件之前，验证文件名称扩展 (.inf) 处于小写字母。 此操作区分大小写，并且如果文件扩展名具有大写字母不将加载驱动程序文件。  
  
7.  在提示符下，单击**是**设置文本模式阶段做出存储驱动程序可用。  
  
 设置现在应正常继续进行。  
  
###  <a name="BKMK_AddingNICdrivers"></a>添加网络适配器驱动程序  
 如果 Windows Server Essentials 不支持的计算机上的网络适配器，则你服务器设置完毕，并且你将无法连接到服务器的计算机后不可具有网络连接。  
  
 在 Windows Server Essentials 安装结束时，将通知如果网络适配器驱动程序没有自动安装。 你还可以使用**网络连接**控制面板查找丢失的网络适配器驱动程序中。 如果你未看到与你的服务器上的网络适配器的网络连接，你需要安装驱动程序。  
  
 如果计算机缺少受支持的任何网络适配器驱动程序，你将需要手动安装正确的网络适配器驱动程序，然后重新启动的服务器。 使用下面的过程。  
  
##### <a name="to-install-a-network-adapter-driver"></a>安装网络适配器驱动程序  
  
1.  获取的网络适配器的制造商丢失的驱动程序。  
  
2.  按照制造商的安装说明以安装驱动程序。  
  
3.  重启计算机。  
  
    > [!IMPORTANT]
    >  如果你不重新启动服务器安装丢失的网络适配器驱动程序后的客户端计算机上的 Windows Server Essentials 接头软件安装可能失败。
