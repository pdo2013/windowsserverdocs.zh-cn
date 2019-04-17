---
title: "广告森林恢复-备份完整服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: b1af97c2eb23d65c2d106906bc0f5bb1f10b23ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>广告森林恢复-备份完整服务器  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

完整服务器备份建议准备的森林恢复，因为它可以还原到不同的硬件或其他操作系统实例。  使用 Windows Server 备份，你可以执行服务器完整的备份。 

## <a name="windows-server-backup"></a>Windows Server 备份
Windows Server 备份不是默认情况下安装。 在 Windows Server 2016 和 Windows Server 2012 R2，按照以下步骤安装它。

>[!NOTE]
>需要注意的步骤可能略有不同，Windows Server 2016 和 Windows Server 2012 R2 之间。

若要将其安装在 Windows Server 2008 和 Windows Server 2008 R2 的步骤，请参阅[安装 Windows Server 备份](https://technet.microsoft.com/library/cc771232.aspx)。  

### <a name="to-install-windows-server-backup"></a>若要安装 Windows Server 备份
1. 打开**服务器管理器**单击**添加角色和功能**。
2. 在**添加角色和功能向导**单击**下一步**。
3. 在**安装类型**屏幕上，将默认**角色基于或功能的安装**单击**下一步**。
4. 在**选择服务器**屏幕上，单击**下一步**。
5. 在**服务器角色**屏幕单击**下一步**。
6. 在**功能**屏幕上，选择**Windows Server 备份**单击**下一步**<ph x="4">
！[</ph>安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. 单击**安装**。
8. 安装完成后，单击**关闭**。


### <a name="to-perform-a-backup-with-windows-server-backup"></a>若要进行备份与 Windows Server 备份

1. 打开**服务器管理器**，单击**工具**，然后单击**Windows Server 备份**。
    - 在 Windows Server 2008 R2 和 Windows Server 2008、单击**开始**，指向**管理工具**，然后单击**Windows Server 备份**。 
![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. 你将看到提示后，在**用户帐户控制**对话框中，提供备份运营商的凭据，然后单击**确定**。
3. 单击**本地备份**。
4. 在**操作**菜单上，单击**一次备份**。
5. 在备份一次向导中，在**备份选项**页上，单击**不同的选项**，然后单击**下一步**。
![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. 在**选择备份配置**页上，单击**完整服务器（推荐）**，然后单击**下一步**。
7. 在**指定目的地类型**页上，单击**本地驱动器**或**远程共享文件夹**，然后单击**下一步**。
8. 在**选择备份目标**页面上，选择备份的位置。  如果你选择在本地驱动器选择本地驱动器或共享如果选择了远程选择网络共享。
9. 在确认屏幕中，单击**备份**。
![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)
10. 完成后对此单击**关闭**。
11. 关闭 Windows Server 备份。

>[!NOTE]
>如果你收到错误消息，指出不备份存储位置可用时，你将需要排除的某个已选择卷或添加一个新的音量或远程共享。
>如果你收到一个警告，指出所选的音量，也包含在要备份的项目列表，可以决定是否删除，然后单击**确定**。

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>使用 Wbadmin.exe 备份 windows server
Wbadmin.exe 是使您能够备份和还原你的操作系统、卷、文件、文件夹和应用程序的 command prompt 下一个命令行实用工具。

#### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>若要执行使用 Wbadmin.exe 完全服务器备份  
  
1.  打开提升了权限的命令提示符下，键入以下命令，并按 ENTER:  

        wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:

![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
