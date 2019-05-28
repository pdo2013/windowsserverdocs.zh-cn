---
title: 创建可启动的 U 盘
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 05/04/2018
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cb62a460c09fdb2874bcc051176a05e88cee19e7
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621278"
---
# <a name="create-a-bootable-usb-flash-drive"></a>创建可启动的 U 盘

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

可以创建可启动 USB 闪存驱动器以用于部署 Windows Server Essentials。 第一步是使用命令行实用程序 DiskPart 准备 U 盘。 有关 DiskPart 的信息，请参阅 [DiskPart 命令行选项](https://go.microsoft.com/fwlink/?LinkId=207073)。  


> [!TIP]
> 若要在恢复或在 PC 而不是服务器上重新安装 Windows 中创建用于可启动 USB 闪存驱动器，请参阅[创建恢复驱动器](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)。
  
 有关你可能希望创建或使用可启动 U 盘的其他方案，请参阅以下主题：  
  
-   [从现有客户端计算机备份还原完整系统](../manage/restore-a-full-system-from-an-existing-client-computer-backup.md)  
  
-   [还原或修复运行 Windows Server Essentials 的服务器](../manage/restore-or-repair-your-server-running-windows-server-essentials.md)  

  
### <a name="to-create-a-bootable-usb-flash-drive"></a>创建可启动的 U 盘  
  
1.  将 U 盘插入正在运行的计算机。  
  
2.  以管理员身份打开“命令提示符”窗口。  
  
3.  键入 `diskpart`。  
  
4.  在新打开的命令行窗口中，要确定 USB 闪存驱动器编号或驱动器号，请在命令提示符处键入 `list disk`，然后单击 ENTER。 `list disk` 命令将显示计算机上的所有磁盘。 请记住 U 盘的驱动器编号或驱动器号。  
  
5.  在命令提示符下，键入 `select disk <X>`，其中 X 是 USB 闪存驱动器的驱动器编号或驱动器号，然后单击 Enter。  
  
6.  键入 `clean`，然后单击 Enter。 此命令会删除 U 盘中的所有数据。  
  
7.  若要在 USB 闪存驱动器上创建新的主分区，请键入 `create partition primary`，然后单击 Enter。  
  
8.  若要选择刚创建的分区，请键入 `select partition 1`，然后单击 Enter。  
  
9. 若要对分区进行格式化，请键入 `format fs=ntfs quick`，然后单击 Enter。  
  
    > [!IMPORTANT]
    >  如果你的服务器平台支持统一可扩展固件接口 (UEFI)，则应将 U 盘格式化为 FAT32，而非 NTFS。 若要将分区格式化为 FAT32，请键入 `format fs=fat32 quick`，然后单击 Enter。  
  
10. 键入 `active`，然后单击 Enter。  
  
11. 键入 `exit`，然后单击 Enter。  
  
12. 完成自定义映像的准备后，将其保存到 U 盘的根目录。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)   

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [部署准备的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)   

 [我们如何帮助你？](https://windows.microsoft.com/windows/support)
