---
title: Windows Server Essentials 安装疑难解答
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: 4756d3735fd710930e0eb124b7b5c58c50078d9e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432422"
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Windows Server Essentials 安装疑难解答

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本主题提供安装 Windows Server Essentials 时发生的问题的疑难解答。 以下各部分提供了指南：  
  

-   [常规故障排除步骤](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [对驱动程序问题进行故障排除](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [常规故障排除步骤](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [对驱动程序问题进行故障排除](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  对于 Windows Server Essentials 社区的最新疑难解答信息，我们建议您访问[Windows Server Essentials 论坛](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 论坛是寻求帮助或提出问题的好地方。  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a> 常规故障排除步骤  
 如果 Windows Server Essentials 的安装失败，请执行以下步骤来帮助确定导致失败的问题。  
  
> [!IMPORTANT]
>  非常重要，您不手动重新启动你的服务器安装 Windows Server Essentials 时。 在安装和初始配置期间，服务器将自动重新启动几次。 如果你在看到“服务器安装成功”  消息之前已手动重新启动服务器，这可能会中断安装并导致安装失败。  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>若要确定失败的安装的 Windows Server Essentials 中的问题  
  
1.  严重你的服务器硬件是否符合最低要求。 有关硬件要求的信息，请参阅[Windows Server Essentials 的系统要求](../get-started/system-requirements.md)。  
  
2.  如果从 MSDN 收到 Windows Server Essentials 安装 DVD，验证该 DVD 是否通过检查 SHA1 和有效。 有关详细信息，请参阅[文件校验和完整性验证程序实用工具的可用性和描述](https://go.microsoft.com/fwlink/?LinkId=220495)(https://go.microsoft.com/fwlink/?LinkId=220495)。  
  
3.  确认服务器上的网络适配器是否已通过网线连接到路由器。  
  
4.  如果该计算机具有多个网络适配器，请验证是否仅启用一个网络适配器。  
  
    > [!IMPORTANT]
    >  不要断开网络电缆或安装 Windows Server Essentials 时重新启动路由器。  
  
5.  在查看"服务器安装和部署" [Release Documentation for Windows Server Essentials](../get-started/release-notes.md)  
  
6.  如果收到错误消息时发生错误设置安装期间，服务器使用服务器恢复 DVD 和硬件的制造商提供的说明进行操作来将服务器还原到出厂默认设置。  
  
##  <a name="BKMK_TroubleshootDrivers"></a> 对驱动程序问题进行故障排除  
 安装 Windows Server Essentials 时最常见的问题是需要手动安装驱动程序的存储控制器。 Windows 包含适用于很多存储控制器的驱动程序，但它可能不包含适用于特定硬件的驱动程序。  
  
 你可能需要为你的特定硬件手动安装网卡驱动程序。  
  
###  <a name="BKMK_StorageDrivers"></a> 添加适用于存储控制器驱动程序  
 如果您的硬件要求不包括与 Windows Server Essentials 的存储驱动程序，请使用以下信息完成设置。  
  
 如果在安装过程中看到以下消息，则需要手动添加适用于你的存储控制器的驱动程序：  
  
 **Windows Server 安装程序错误**  
  
 找不到硬盘驱动器上能够托管 Windows Server Essentials。 是否要加载其他存储驱动程序？  
  
 使用下列过程安装存储控制器驱动程序。  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>手动安装存储控制器驱动程序  
  
1. 查找适用于你的存储控制器的驱动程序。 它们由硬件制造商提供，并且也可能位于制造商的网站上。  
  
2. 在软盘驱动器或 U 盘上创建一个名为 DRIVERS 的文件夹，然后将驱动程序复制到该文件夹中。  
  
3. 使用这些驱动程序将软盘驱动器或 U 盘连接到计算机  
  
4. 从 Windows Server Essentials DVD 计算机启动。  
  
    如果缺少任何存储控制器驱动程序，将显示 Windows Server Essentials 安装错误对话框。  
  
5. 在 Windows Server Essentials 安装错误对话框中，单击**是**加载其他存储驱动程序。  
  
6. 在“请选择驱动程序的 inf 文件”  提示下，导航到软盘驱动器或 U 盘上的 DRIVERS 文件夹中的 .inf 文件、选择该文件、右键单击文件名，然后单击“打开”  。 这将加载驱动程序。  
  
   > [!NOTE]
   >  在尝试加载该文件之前，请验证其文件扩展名 (.inf) 采用小写字母。 此操作区分大小写，如果文件扩展名包含大写字母，驱动程序文件将不会加载。  
  
7. 在提示下，单击“是”  ，使存储驱动程序可在安装程序的文本模式阶段内使用。  
  
   安装程序现在应继续正常运行。  
  
###  <a name="BKMK_AddingNICdrivers"></a> 添加网络适配器驱动程序  
 如果 Windows Server Essentials 不支持在计算机上的网络适配器，您的服务器将没有网络连接之后安装完成，并且你将无法再将计算机连接到你的服务器。  
  
 在 Windows Server Essentials 安装结束时，系统将通知您如果网络适配器驱动程序不会自动安装。 你也可以使用“控制面板”中的“网络连接”  来检查缺少的网络适配器驱动程序。 如果在服务器上看不到与网络适配器相关联的网络连接，则需要安装驱动程序。  
  
 如果计算机缺少支持的适用于任何网络适配器的驱动程序，则需要手动安装正确的网络适配器驱动程序，然后重新启动服务器。 使用以下过程。  
  
##### <a name="to-install-a-network-adapter-driver"></a>安装网络适配器驱动程序  
  
1.  从网络适配器制造商处获取缺少的驱动程序。  
  
2.  请按照制造商的安装说明来安装驱动程序。  
  
3.  重新启动计算机。  
  
    > [!IMPORTANT]
    >  如果在安装缺少的网络适配器驱动程序后不重新启动服务器，客户端计算机上的 Windows Server Essentials 连接器软件安装可能会失败。
