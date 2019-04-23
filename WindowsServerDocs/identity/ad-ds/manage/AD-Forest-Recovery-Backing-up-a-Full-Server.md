---
title: AD 林恢复-备份整个服务器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: fec8de8ea1dadb392f6a3bd1c881e8df2266f404
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846518"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>AD 林恢复-备份整个服务器  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

建议的完整服务器备份来准备林恢复，因为它可以还原到不同的硬件或不同的操作系统实例。  使用 Windows Server Backup 可以执行你的服务器的完整备份。 

## <a name="windows-server-backup"></a>Windows Server 备份

默认情况下不安装 Windows Server Backup。 在 Windows Server 2016 和 Windows Server 2012 R2 中，通过执行以下步骤安装它。

>[!NOTE]
>请注意步骤可能略有不同，Windows Server 2016 和 Windows Server 2012 R2 之间。

若要安装 Windows Server 2008 和 Windows Server 2008 R2 中的步骤，请参阅[安装 Windows Server Backup](https://technet.microsoft.com/library/cc771232.aspx)。  

### <a name="to-install-windows-server-backup"></a>若要安装 Windows Server Backup

1. 打开**服务器管理器**然后单击**添加角色和功能**。
2. 上**添加角色和功能向导**单击**下一步**。
3. 上**安装类型**屏幕上，保留默认**基于角色或基于功能的安装**然后单击**下一步**。
4. 上**服务器选择**屏幕上，单击**下一步**。
5. 上**服务器角色**屏幕单击**下一步**。
6. 上**功能**屏幕上，选择**Windows Server Backup**然后单击**下一步**
   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. 单击“安装” 。
8. 安装完成后，单击**关闭**。

### <a name="to-perform-a-backup-with-windows-server-backup"></a>若要使用 Windows Server Backup 执行备份

1. 打开**服务器管理器**，单击**工具**，然后单击**Windows Server Backup**。
   - 在 Windows Server 2008 R2 和 Windows Server 2008 中，单击**启动**，依次指向**管理工具**，然后单击**Windows Server Backup**。

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. 如果系统提示，在**用户帐户控制**对话框中，提供备份操作员凭据，然后单击**确定**。
3. 单击**本地备份**。
4. 在 **“操作”** 菜单中，单击 **“一次性备份”**。
5. 在一次性备份向导，在**备份选项**页上，单击**不同的选项**，然后单击**下一步**。

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. 上**选择备份配置**页上，单击**整个服务器 （推荐）**，然后单击**下一步**。
7. 上**指定目标类型**页上，单击**本地驱动器**或**远程共享文件夹**，然后单击**下一步**。
8. 上**选择备份目标**页上，选择备份位置。  如果您选择本地驱动器选择本地驱动器或共享如果选择了远程选择网络共享。
9. 在确认屏幕上，单击**备份**。

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. 部署完成后单击**关闭**。
11. 关闭 Windows Server Backup。

>[!NOTE]
>如果收到一个错误，指出没有备份存储位置可用，你将需要排除一个已选择的卷，或者添加一个新卷或远程共享。
>如果收到一个警告，指出所选的卷也包含在备份的项的列表，确定是否要删除，然后单击**确定**。

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>使用 Wbadmin.exe 来备份 windows server

Wbadmin.exe 是一个命令行实用工具，可用于备份和还原操作系统、 卷、 文件、 文件夹和应用程序从命令提示符。

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>若要执行使用 Wbadmin.exe 的完整服务器备份
  
- 打开提升的命令提示符，键入以下命令并按 ENTER:  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![安装备份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
