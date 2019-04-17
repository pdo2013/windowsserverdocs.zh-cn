---
title: "广告森林恢复-备份完整服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adfs
ms.openlocfilehash: a86d61536f8b426e1a5258c661d4e53da63d4162
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>广告森林恢复的数据备份系统状态  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
 
使用下面的过程中使用 Windows Server 备份或 wbadmin.exe DC 在执行系统状态备份。  
  
## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>若要执行使用 Windows Server 备份系统状态备份  
1. 打开**服务器管理器**，单击**工具**，然后单击**Windows Server 备份**。
    - 在 Windows Server 2008 R2 和 Windows Server 2008、单击**开始**，指向**管理工具**，然后单击**Windows Server 备份**。 
![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. 你将看到提示后，在**用户帐户控制**对话框中，提供备份运营商的凭据，然后单击**确定**。
3. 单击**本地备份**。
4. 在**操作**菜单上，单击**一次备份**。
5. 在备份一次向导中，在**备份选项**页上，单击**不同的选项**，然后单击**下一步**。
![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. 在**选择备份配置**页上，单击**自定义)**，然后单击**下一步**。
7. 上**选择要备份的项**屏幕上，单击**添加项目**选择**系统状态**单击**确定**。
    - 在 Windows Server 2008 R2 和 Windows Server 2008、选择以包括在备份中的卷。 如果你选择**启用系统恢复**复选框，选中所有关键卷。 
![安装备份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  
8. 在**指定目的地类型**页上，单击**本地驱动器**或**远程共享文件夹**，然后单击**下一步**。  如果你备份到远程的共享文件夹，请执行以下操作：  
  
 1.  键入的共享文件夹的路径。  
 2.  下**访问控制**、选择**文件不沿用文件**或**继承**以确定访问备份，然后单击**下一步**。  
 3.  在**提供用于备份用户凭据**对话框中，对于具有写入访问权限的共享文件夹的用户提供用户名和密码，然后单击**确定**。
9. Windows Server 2008 R2 和 Windows Server 2008、上**指定高级选项**页上，选择**VSS 复制备份**，然后单击**下一步**。
10. 在**选择备份目标**页面上，选择备份的位置。  如果你选择在本地驱动器选择本地驱动器或共享如果选择了远程选择网络共享。
11. 在确认屏幕中，单击**备份**。
12. 完成后对此单击**关闭**。
13. 关闭 Windows Server 备份。

  
## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>若要执行使用 Wbadmin.exe 系统状态备份  
  
1.  打开提升了权限的命令提示符下，键入以下命令，并按 ENTER:  
  
    ```  
    wbadmin start systemstatebackup -backuptarget:<targetDrive>:
    ```  
![安装备份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
