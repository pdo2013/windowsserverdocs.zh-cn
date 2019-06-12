---
title: AD 林恢复-执行整个服务器恢复
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 9cf89c9f4875f602abea89e366cadfba8d0599c3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443012"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>AD 林恢复-执行整个服务器恢复 

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用以下过程来为 Windows Server 2016、 2012 R2 或 2012年执行整个服务器恢复。 

## <a name="active-directory-full-server-recovery"></a>Active Directory 整个服务器恢复

整个服务器恢复时需要将还原到不同的硬件或不同的操作系统实例。 请牢记以下几点：

- 在目标服务器必须是相等的数字在备份驱动器的数量和他们需要具有相同大小或更高版本。
- 目标服务器需要访问以启动从操作系统 DVD**修复计算机**选项。 
- 如果的目标 DC 的 VM 中运行的 HYPER-V 和备份存储的网络位置上，必须安装旧版网络适配器。 
- 执行整个服务器恢复之后，您需要单独执行 SYSVOL 的权威还原中所述[AD 林恢复-执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

具体取决于你的方案，使用以下过程之一执行完整还原。 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>执行整个服务器还原使用最新映像的本地备份
  
1. 启动 Windows 安装程序，指定的语言、 时间和货币格式及键盘选项然后单击**下一步**。 
2. 单击**修复计算机**。
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. 单击**疑难解答**。</br>
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. 单击**系统映像恢复**。</br>
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. 单击**Windows Server 2016**。 
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. 如果要还原的最新的本地备份，请单击**使用最新的可用系统映像 （推荐）** 然后单击**下一步**。
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. 系统现在将提供一个选项为：
   -  格式化并重新分区磁盘
   -  安装驱动程序
   -  取消选中**高级**功能自动重新启动并检查磁盘错误。 默认情况下启用这些策略。
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. 单击“下一步”  。
9. 单击 **“完成”** 。 系统将提示询问您是否确定要继续。 单击 **“是”** 。 
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. 此操作完成后执行 SYSVOL 的权威还原，如中所述[AD 林恢复-执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>执行整个服务器还原与任何本地或远程的映像

1. 启动 Windows 安装程序，指定的语言、 时间和货币格式及键盘选项然后单击**下一步**。 
2. 单击**修复计算机**。</br>
3. 单击**进行故障排除**，单击**系统映像恢复**，然后单击**Windows Server 2016**。 
4. 如果要还原的最新的本地备份，请单击**选择系统映像**然后单击**下一步**。
5. 现在可以选择你想要还原的备份的位置。 如果图像是本地你可以从列表中选择它。 
6. 如果图像是网络共享上，选择**高级**。 您还可以选择**高级**如果你需要安装驱动程序。
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. 如果要还原从网络单击后**高级**选择**搜索系统映像在网络上**。 您可能会提示以恢复网络连接。 选择确定。 </br>
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. 键入备份共享位置的 UNC 路径 (例如， \\\server1\backups)，单击**确定**。 此外可以如键入目标服务器的 IP 地址\\\192.168.1.3\backups。 
   ![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
9. 键入凭据即可访问该共享，然后单击确定。 
10. 现在**选择要还原的日期和时间的系统映像**然后单击**下一步**。
11. 系统现在将提供一个选项为：
    - 格式化并重新分区磁盘
    - 安装驱动程序
    - 取消选中**高级**功能自动重新启动并检查磁盘错误。 默认情况下启用这些策略。
12. 单击“下一步”  。
13. 单击 **“完成”** 。 系统将提示询问您是否确定要继续。 单击 **“是”** 。  
14. 此操作完成后执行 SYSVOL 的权威还原，如中所述[AD 林恢复-执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>启用网络备份的网络适配器

如果你需要启用从命令提示符下的网络适配器，从网络共享中还原，使用以下步骤。

1. 启动 Windows 安装程序，指定的语言、 时间和货币格式及键盘选项然后单击**下一步**。 
2. 单击**修复计算机**。 I
3. 单击**进行故障排除**，单击**命令提示符下**。 
4. 键入以下命令，然后按 Enter：  

   ```  
   wpeinit  
   ```

5. 若要确认网络适配器的名称，请键入：  

   ```  
   show interfaces  
   ```  

   键入以下命令和每个命令后面按 ENTER:  

   ```  
   netsh  
   ```  

   ```  
   interface  
   ```  
  
   ```  
   tcp  
   ```  

   ```  
   ipv4  
   ```  
  
   ```  
   set address "Name of Network Adapter" static IPv4 Address SubnetMask IPv4 Gateway Address 1  
   ```  

   例如：  
  
   ```  
   set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
   ```  

   类型`quit`以返回到命令提示符。 类型`ipconfig /all`若要验证网络适配器具有 IP 地址，并重试托管备份的共享，以确认连接的服务器的 IP 地址执行 ping 操作。 完成后，请关闭命令提示符。 

6. 现在，使用的网络适配器，选择上述步骤来完成还原。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
