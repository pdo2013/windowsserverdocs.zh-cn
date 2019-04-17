---
title: "创建 USB 可启动 Flash 驱动器"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9d587329e1141040b2511e1574649f1844dcec90
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-bootable-usb-flash-drive"></a>创建 USB 可启动 Flash 驱动器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以创建可启动 U 盘以部署 Windows Server Essentials 使用。 第一步是通过使用 DiskPart，它命令行实用工具准备 U 盘。 有关 DiskPart 的信息，请参阅[DiskPart 命令行选项](https://go.microsoft.com/fwlink/?LinkId=207073)。  
  
 其他情况下，你可能想要创建或使用可启动 U 盘，请参阅以下主题：  
  
-   [从现有的客户端计算机备份中还原完整系统](https://technet.microsoft.com/library/jj713539.aspx#BKMK_CreateBootable)  
  
-   [还原或修复你运行的 Windows Server Essentials 服务器](https://technet.microsoft.com/library/jj593197.aspx#BKMK_Restore_2)  
  
### <a name="to-create-a-bootable-usb-flash-drive"></a>若要创建可启动 U 盘  
  
1.  正在运行的计算机中插入 U 盘。  
  
2.  以 administrator 身份打开一个 Command Prompt 窗口。  
  
3.  键入`diskpart`。  
  
4.  在新的命令行窗口中打开，以确定 USB 刷写的驱动器号或驱动器号，在命令提示符下，键入`list disk`，然后单击 enter 键。 `list disk`命令显示所有磁盘计算机上。 请注意的驱动器号或 U 盘驱动器号。  
  
5.  在命令提示符下，键入`select disk <X>`、 了 X 是驱动器号或 USB 驱动器号刷写的驱动器，然后单击 enter 键。  
  
6.  键入`clean`，然后单击 enter 键。 此命令从 U 盘中删除的所有数据。  
  
7.  若要创建一个新的主分区，在 U 盘上，键入`create part pri`，然后单击 enter 键。  
  
8.  若要选择刚刚创建分区，请键入`select part 1`，然后单击 enter 键。  
  
9. 若要格式化分区中，键入`format fs=ntfs quick`，然后单击 enter 键。  
  
    > [!IMPORTANT]
    >  如果 server 平台支持统一可扩展固件接口 (UEFI)，你应为 FAT32 而非 NTFS 格式 U 盘。 要格式化为 FAT32 分区，请键入`format fs=fat32 quick`，然后单击 enter 键。  
  
10. 键入`active`，然后单击 enter 键。  
  
11. 键入`exit`，然后单击 enter 键。  
  
12. 准备您的自定义映像完毕后，请将其保存到 U 盘根。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)   

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)   

 [我们如何帮助你？](https://windows.microsoft.com/windows/support)
