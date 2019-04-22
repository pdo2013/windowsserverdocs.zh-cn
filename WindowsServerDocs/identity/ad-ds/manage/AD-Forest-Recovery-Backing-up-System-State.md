---
title: AD 林恢复-备份整个服务器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: a5306960bb2dca3849bdb4fc7304781af3f25335
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815868"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>AD 林恢复的备份系统状态数据  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用以下过程来使用 Windows Server Backup 或 wbadmin.exe 对 DC 执行系统状态备份。  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>若要执行使用 Windows Server Backup 的系统状态备份

1. 打开**服务器管理器**，单击**工具**，然后单击**Windows Server Backup**。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，单击**启动**，依次指向**管理工具**，然后单击**Windows Server Backup**。 

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. 如果系统提示，在**用户帐户控制**对话框中，提供备份操作员凭据，然后单击**确定**。
3. 单击**本地备份**。
4. 在 **“操作”** 菜单中，单击 **“一次性备份”**。
5. 在一次性备份向导，在**备份选项**页上，单击**不同的选项**，然后单击**下一步**。

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. 在**选择备份配置**页上，单击**自定义)**，然后单击**下一步**。
7. 上**选择要备份的项**屏幕上，单击**添加的项**，然后选择**系统状态**然后单击**确定**。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，选择要在备份中包含的卷。 如果选择**启用系统恢复**复选框，选择所有关键卷。 

   ![安装备份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. 上**指定目标类型**页上，单击**本地驱动器**或**远程共享文件夹**，然后单击**下一步**。  如果要备份到远程共享文件夹，请执行以下操作：  
   - 键入共享文件夹的路径。
   - 下**访问控制**，选择**不会继承**或**继承**以确定访问备份，然后单击**下一步**。  
   - 在中**为备份提供用户凭据**对话框中，对共享文件夹具有写访问权限的用户提供用户名和密码，然后单击**确定**。

9. Windows Server 2008 R2 和 Windows Server 2008 上**指定高级选项**页上，选择**VSS 副本备份**，然后单击**下一步**。
10. 上**选择备份目标**页上，选择备份位置。  如果您选择本地驱动器选择本地驱动器或共享如果选择了远程选择网络共享。
11. 在确认屏幕上，单击**备份**。
12. 部署完成后单击**关闭**。
13. 关闭 Windows Server Backup。

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>若要执行使用 Wbadmin.exe 的系统状态备份

打开提升的命令提示符，键入以下命令并按 ENTER:  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![安装备份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
