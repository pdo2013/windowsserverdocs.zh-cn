---
title: "广告森林恢复-执行完整 server 恢复"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adfs
ms.openlocfilehash: 5de71cc005e9ec4c1425f9fa805b3c043ab4ea36
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>广告森林恢复-执行完整 server 恢复 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
 
使用下面的过程中对 Windows Server 2016、2012 R2 或 2012 年执行完整 server 恢复。 

## <a name="active-directory-full-server-recovery"></a>Active Directory 完整 Server 恢复
完全服务器恢复是必要，如果你要还原到不同的硬件或其他操作系统实例。 牢记以下：

- 关注目标服务器需要等于备份数驱动器的数量，他们需要不同大小或更高版本。
- 目标服务器需要从操作系统 DVD 以便访问启动**修复你的计算机**选项。 
- 如果 DC Hyper-V 和备份，在 VM 中运行的目标存储在网络位置，你必须安装旧版网络适配器。  
- 执行完整服务器恢复操作后，你需要单独执行 SYSVOL，授权还原中所述[广告森林恢复-执行 DFSR 复制 SYSVOL 权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。


根据你的情况下，使用以下过程中的一个执行完整还原。  
  
### <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>执行完整服务器具有最新的图像与本地备份还原
  
1.  启动 Windows 安装程序、语言、时间和货币格式和键盘选项指定和单击**下一步**。  
2.  单击**修复你的计算机**。</br>
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3.  单击**疑难解答**。</br>
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4.  单击**系统映像恢复**。</br>
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5.  单击**Windows Server 2016**。  
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6.  如果你要还原的最新的本地备份，请单击**使用最新可用系统映像（推荐）**单击**下一步**。
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7.  你现在将提供选项：
    -  格式和重新分区磁盘
    -  安装驱动程序
    -  取消选择**高级**功能的自动重新启动并检查磁盘的错误。  默认情况下启用了这些。
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. 单击**下一步**。
9. 单击**完成**。  你将会提示你询问你是否想要继续。  单击**是**。  
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. 此操作完成后执行 SYSVOL，授权还原中所述[广告森林恢复-执行 DFSR 复制 SYSVOL 权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。
 

### <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>执行完整服务器与本地或远程任何映像还原
1.  启动 Windows 安装程序、语言、时间和货币格式和键盘选项指定和单击**下一步**。  
2.  单击**修复你的计算机**。</br>
3.  单击**疑难解答**，单击**系统映像恢复**，然后单击**Windows Server 2016**。  
4.  如果你要还原的最新的本地备份，请单击**选择系统映像**单击**下一步**。

5.  现在，你可以选择你想要还原的备份的位置。  本地图像是否可以从列表中选择它。  
6.  如果在网络共享上图像，请选择**高级**。  你还可以选择**高级**如果你需要安装驱动程序。
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7.  如果你要还原从网络后，单击**高级**选择**系统映像网络上的搜索**。  系统可能提示你还原网络连接。  选择确定。 </br>
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. 键入 UNC 通往备份共享位置 (如 \\\server1\backups)，然后单击**确定**。  你还可以键入目标服务器，如 \\\192.168.1.3\backups 的 IP 地址。  
![服务器还原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. 键入凭据需访问该共享，然后单击确定。  
11. 现在**选择的日期和时间的系统映像还原**单击**下一步**。
12. 你现在将提供选项：
    1.   格式和重新分区磁盘
    2.   安装驱动程序
    3.   取消选择**高级**功能的自动重新启动并检查磁盘的错误。  默认情况下启用了这些。
13. 单击**下一步**。
14. 单击**完成**。  你将会提示你询问你是否想要继续。  单击**是**。   
15. 此操作完成后执行 SYSVOL，授权还原中所述[广告森林恢复-执行 DFSR 复制 SYSVOL 权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。


### <a name="enabling-the-network-adapter-for-a-network-backup"></a>启用网络备份的网络适配器
如果你需要启用从命令提示符的网络适配器还原从网络共享，使用以下步骤。

1.  启动 Windows 安装程序、语言、时间和货币格式和键盘选项指定和单击**下一步**。  
2.  单击**修复你的计算机**。 我
3.  单击**疑难解答**，单击**权限的命令提示符**。  
4.  键入以下命令，然后按 enter 键：  
  
    ```  
    wpeinit  
    ```   
5.  若要确认该网络适配器的名称，键入：  
  
    ```  
    show interfaces  
    ```  
  
     键入以下命令并在每个命令后按 enter 键：  
  
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
  
     键入`quit`返回到 command prompt 下。 键入`ipconfig /all`验证该网络适配器具有 IP 地址，然后重试 ping 承载以确认连接的备份共享的服务器的 IP 地址。 完成后，请关闭命令提示符。  
  
6.  现在，该网络适配器工作、选择上述步骤以完成还原。

## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
