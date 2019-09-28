---
title: AD 林恢复-执行完整服务器恢复
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 1ade1f2e316387fbe84209c1bc7a986fff6f2a71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390537"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>AD 林恢复-执行完整服务器恢复 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用以下过程对 Windows Server 2016、2012 R2 或2012执行完整服务器恢复。 

## <a name="active-directory-full-server-recovery"></a>Active Directory 完全服务器恢复

如果要还原到不同的硬件或不同的操作系统实例，则完全服务器恢复是必需的。 请牢记以下几点：

- 目标服务器上的驱动器号需要与备份中的数字相等，并且需要相同的大小或更大。
- 为了访问 "**修复计算机**" 选项，需要从操作系统 DVD 启动目标服务器。 
- 如果目标 DC 正在 Hyper-v 上的虚拟机中运行，并且备份存储在网络位置上，则必须安装旧版网络适配器。 
- 执行完整服务器恢复之后，需要单独执行 SYSVOL 的权威还原，如[AD 林恢复-执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)中所述。

根据具体方案，使用以下过程之一执行完全还原。 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>使用最新映像通过本地备份执行完整服务器还原
  
1. 开始 Windows 安装程序 "，指定语言、时间和货币格式以及键盘选项，然后单击"**下一步**"。 
2. 单击**修复计算机**。
   @no__t 0Server Restore @ no__t-1
3. 单击**疑难解答**。</br>
   @no__t 0Server Restore @ no__t-1
4. 单击 "**系统映像恢复**"。</br>
   @no__t 0Server Restore @ no__t-1
5. 单击 " **Windows Server 2016**"。 
   @no__t 0Server Restore @ no__t-1
6. 如果要还原最近的本地备份，请单击 **"使用最新的可用系统映像（推荐）"** ，然后单击 "**下一步**"。
   @no__t 0Server Restore @ no__t-1
7. 现在，你可以选择执行以下操作：
   -  对磁盘进行格式化和重新分区
   -  安装驱动程序
   -  取消选择自动重启和检查磁盘错误的**高级**功能。 默认情况下，这些设置处于启用状态。
   @no__t 0Server Restore @ no__t-1
8. 单击“下一步”。
9. 单击 **“完成”** 。 系统将提示你是否确定要继续。 单击 **“是”** 。 
   @no__t 0Server Restore @ no__t-1 
10. 完成后，执行 SYSVOL 的权威还原，如[AD 林恢复中所述-执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>使用任何映像本地或远程执行完整服务器还原

1. 开始 Windows 安装程序 "，指定语言、时间和货币格式以及键盘选项，然后单击"**下一步**"。 
2. 单击**修复计算机**。</br>
3. 单击 "**疑难解答**"，单击 "**系统映像恢复**"，然后单击 " **Windows Server 2016**"。 
4. 如果要还原最近的本地备份，请单击 "**选择系统映像**"，然后单击 "**下一步**"。
5. 现在，你可以选择要还原的备份的位置。 如果映像为本地映像，可以从列表中选择它。 
6. 如果映像位于网络共享上，请选择 "**高级**"。 如果需要安装驱动程序，还可以选择 "**高级**"。
   @no__t 0Server Restore @ no__t-1
7. 如果要从网络还原，请在单击 "**高级**"，然后在**网络上搜索系统映像**。 系统可能会提示你还原网络连接。 选择 "确定"。 </br>
   @no__t 0Server Restore @ no__t-1
8. 键入备份共享位置的 UNC 路径（例如 \\ \ server1\backups），然后单击 **"确定"** 。 还可以键入目标服务器的 IP 地址，例如 \\ \ 192.168.1.3 \ 备份。 
   @no__t 0Server Restore @ no__t-1
9. 键入访问共享所需的凭据，然后单击 "确定"。 
10. 现在，**选择要还原的系统映像的日期和时间**，然后单击 "**下一步**"。
11. 现在，你可以选择执行以下操作：
    - 对磁盘进行格式化和重新分区
    - 安装驱动程序
    - 取消选择自动重启和检查磁盘错误的**高级**功能。 默认情况下，这些设置处于启用状态。
12. 单击“下一步”。
13. 单击 **“完成”** 。 系统将提示你是否确定要继续。 单击 **“是”** 。  
14. 完成后，执行 SYSVOL 的权威还原，如[AD 林恢复中所述-执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>为网络备份启用网络适配器

如果需要从命令提示符启用网络适配器以从网络共享进行还原，请使用以下步骤。

1. 开始 Windows 安装程序 "，指定语言、时间和货币格式以及键盘选项，然后单击"**下一步**"。 
2. 单击**修复计算机**。 I
3. 单击 "**疑难解答**"，单击 "**命令提示符**"。 
4. 键入以下命令，然后按 Enter：  

   ```  
   wpeinit  
   ```

5. 若要确认网络适配器的名称，请键入：  

   ```  
   show interfaces  
   ```  

   键入以下命令，然后在每个命令后面按 ENTER：  

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

   键入 `quit` 返回到命令提示符。 键入 `ipconfig /all` 验证网络适配器是否具有 IP 地址，并尝试对托管备份共享的服务器的 IP 地址进行 ping 操作，以确认连接。 完成后，关闭命令提示符。 

6. 现在网络适配器正在运行，请选择上述步骤完成还原。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
