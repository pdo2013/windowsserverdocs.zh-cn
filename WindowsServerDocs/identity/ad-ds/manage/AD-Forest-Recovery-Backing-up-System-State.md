---
title: AD 林恢复-备份完整服务器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: 14aa7abc19573b76ebc144cb6dea5f510b45e269
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369347"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>AD 林恢复-备份系统状态数据  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用以下过程在 DC 上使用 Windows Server 备份或 wbadmin 执行系统状态备份。  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>使用 Windows Server 备份执行系统状态备份

1. 打开**服务器管理器**，单击 "**工具**"，然后单击 " **Windows Server 备份**"。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，单击 "**开始**"，指向 "**管理工具**"，然后单击 " **Windows Server 备份**"。 

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. 如果系统提示，请在 "**用户帐户控制**" 对话框中提供备份操作员凭据，然后单击 **"确定"** 。
3. 单击 "**本地备份**"。
4. 在 **“操作”** 菜单中，单击 **“一次性备份”** 。
5. 在 "一次性备份" 向导中的 "**备份选项**" 页上，单击 "**其他选项**"，然后单击 "**下一步**"。

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. 在 "**选择备份配置**" 页上，单击 "**自定义**"，然后单击 "**下一步**"。
7. 在 "**选择备份项**" 屏幕上，单击 "**添加项**" 并选择 "**系统状态**"，然后单击 **"确定"** 。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，选择要包含在备份中的卷。 如果选中 "**启用系统恢复**" 复选框，则所有关键卷都处于选中状态。 

   ![安装备份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. 在 "**指定目标类型**" 页上，单击 "**本地驱动器**" 或 "**远程共享文件夹**"，然后单击 "**下一步**"。  如果要备份到远程共享文件夹，请执行以下操作：  
   - 键入共享文件夹的路径。
   - 在 "**访问控制**" 下，选择 "**不继承**或**继承**" 以确定对备份的访问权限，然后单击 "**下一步**"。  
   - 在 "为**备份提供用户凭据**" 对话框中，提供对共享文件夹具有写访问权限的用户的用户名和密码，然后单击 **"确定"** 。

9. 对于 Windows Server 2008 R2 和 Windows Server 2008，在 "**指定高级选项**" 页上，选择 " **VSS 副本备份**"，然后单击 "**下一步**"。
10. 在 "**选择备份目标**" 页上，选择备份位置。  如果选择了本地驱动器，请选择本地驱动器，或者如果选择了远程共享，请选择网络共享。
11. 在确认屏幕上，单击 "**备份**"。
12. 完成此完成后，单击 "**关闭**"。
13. 关闭 Windows Server 备份。

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>使用 Wbadmin 执行系统状态备份

打开提升的命令提示符，键入以下命令并按 ENTER：  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![安装备份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
