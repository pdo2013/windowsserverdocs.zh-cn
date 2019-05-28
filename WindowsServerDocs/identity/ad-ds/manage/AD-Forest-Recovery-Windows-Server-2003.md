---
title: AD 林恢复-Windows Server 2003 恢复
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: e2af1bfc295469d43e59593d69d4ba88f476e427
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034147"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>AD 林恢复-Windows Server 2003 恢复

>适用于：Windows Server 2003

本主题包括运行 Windows Server 2003 的域控制器 (Dc) 的林恢复过程。 林恢复的一般过程是使用 Windows Server 2003 域控制器，没有什么不同，但特定的过程，因为不同的工具可能不同。 例如，可以使用 Ntdsutil.exe 来备份和还原的域控制器都运行 Windows Server 2003 域控制器，而 Windows Server Backup 或 Wbadmin.exe 是用于域控制器都运行 Windows Server 2008 或更高版本。  
  
- [备份系统状态数据](#backing-up-the-system-state-data)  
- [执行非权威还原](#performing-a-nonauthoritative-restore)  
- [安装和配置 DNS 服务器服务](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>备份系统状态数据
使用以下过程来备份系统状态数据，以及您为运行 Windows Server 2003 DC 的当前备份操作选择的任何其他数据。 Windows Server 2003 包括 Ntbackup 工具，可用于备份系统状态数据。  
  
中的成员身份**管理员**或**Backup Operators**，或等效身份是备份文件和文件夹所需的最低。   
  
如果你要备份到磁带时，系统状态数据和备份计划指示没有任何未使用的媒体可用，您可能必须使用可移动存储。 这到可用媒体池添加您的磁带，因此该备份可以使用它。  
  
你仅可以在本地计算机上备份系统状态数据。 您不能备份它在远程计算机上。  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>若要在域控制器上备份系统状态数据的运行 Windows Server 2003  
  
1. 单击**启动**，依次指向**所有程序**，指向**附件**，指向**系统工具**，然后单击**备份**.  
2. 上**欢迎**页上，单击**高级模式**。  
3. 上**备份**选项卡上，选中的任何驱动器、 文件夹或你想要备份的文件的复选框。  
4. 选择**系统状态**复选框。  
5. 单击**开始备份**。  
  
## <a name="performing-a-nonauthoritative-restore"></a>执行非权威还原  

使用以下过程来执行运行 Windows Server 2003 DC 非权威还原。 通过 Windows Server 2003 中 Active Directory 上执行非权威还原，自动执行非权威 SYSVOL 还原。 无需其他步骤是必需的。  
  
> [!NOTE]
> 如果你要重新安装 Windows Server 2003 操作系统，可能会或可能不将计算机加入到域，您也可以在安装操作系统的过程提供给计算机的任何名称。 不要安装 Active Directory。 后重新安装操作系统，直接转到步骤 4。  
  
在 Windows Server 2003 域控制器还原仅系统状态数据，需要重新安装之前恢复 Dc 运行的任何软件应用程序。 还原域中的第一个 DC 上的 AD DS 还将还原注册表，因为它们都是系统状态数据的一部分。 如果有任何这些 Dc 上运行的应用程序和必须存储在注册表中的任何信息，请记住这一点。  
  
若要保存重新安装软件所需的时间，确定是否需要在域控制器上安装的应用程序与虚拟 DC 克隆兼容。 为了节省时间和将其安装在克隆虚拟域控制器上所需工作量克隆之前可以在源 DC 上安装此类应用程序。  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>若要执行非权威还原
  
1. 启动 DC 后，按 f8 键可以在目录服务还原模式 (DSRM) 重新启动计算机。  
2. 选择**目录服务还原模式 （仅 Windows 域控制器）** 。  
3. 选择你想要在还原模式下启动的操作系统。  
4. 以管理员身份 （也可以仅使用本地计算机帐户，任何域登录选项可用） 登录。  
5. 在命令提示符下键入**ntbackup**，然后按 ENTER。  
6. 上**欢迎**页上，单击**高级模式**，然后选择**还原和管理媒体**选项卡。(不要选择**还原向导**。)  
7. 选择适当的备份文件要还原，请确保**系统磁盘**并**系统状态**选中的复选框。  
8. 单击**开始还原**。  
9. 还原操作完成后，重新启动计算机。  
  
使用以下过程在运行 Windows Server 2003 DC 上执行 SYSVOL 的权威 （也称为主） 还原。 仅在还原域中第一个 Windows Server 2003 DC 上执行此过程。  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>若要执行 SYSVOL 的权威还原  
  
1. 在前面过程中执行步骤 1 至 8。  
2. 在中**确认还原**对话框中，单击**高级**。  
3. 若要执行 SYSVOL 的权威还原，请选中复选框**当还原复制的数据集时，将还原的数据作为所有副本的主数据**。  

   > [!NOTE]
   > 将还原的数据标记为备份中的主数据相当于设置**BurFlags** D4 到以下注册表子项下的条目：  
   >   
   > **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\\** *GUID*  

4. 还原操作完成后，重新启动计算机。  
  
## <a name="install-and-configure-the-dns-server-service"></a>安装和配置 DNS 服务器服务

如果您从备份中还原 DC 运行 Windows Server 2003，可以无需连接到任何网络的 DC 安装 DNS 服务器。  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>若要安装和配置 DNS 服务器服务  
  
1. 打开 Windows 组件向导。 若要打开向导：  

   - 依次单击“开始”  、“控制面板”  ，然后单击“添加或删除程序”  。  
   - 单击**添加/删除 Windows 组件**。  

2. 在中**组件**，选择**网络服务**复选框，然后依次**详细信息**。  
3. 在中**网络服务的子组件**，选择**域名系统 (DNS)** 复选框，单击**确定**，然后单击**下一步**。  
4. 如果系统提示，在**从中复制文件**，键入分发文件的完整路径，然后单击**确定**。  

   安装完成后，完成以下步骤来配置 DNS 服务器。  

5. 单击**启动**，依次指向**所有程序**，指向**管理工具**，然后单击**DNS**。  
6. 严重故障之前的 DNS 服务器上创建了托管的相同 DNS 域名的 DNS 区域。 有关详细信息，请参阅添加正向查找区域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。  
7. 配置 DNS 数据，如其关键工作不正常之前已存在。 例如：  

   - 配置存储在 AD DS 的 DNS 区域。 有关详细信息，请参阅更改区域类型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。  
   - 配置域控制器定位程序 （DC 定位器） 资源记录，以允许安全动态更新具有权威的 DNS 区域。 有关详细信息，请参阅允许仅安全动态更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。  

8. 请确保父 DNS 区域包含委派的资源记录 （名称服务器 (NS) 和粘附主机 (A) 资源记录） 此 DNS 服务器托管子区域。 有关详细信息，请参阅创建区域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。  
9. 配置 DNS 后，在命令提示符下键入以下命令，然后按 ENTER:  

   **net stop netlogon**

10. 键入以下命令，然后按 Enter：  

   **net start netlogon**

   > [!NOTE]
   > Net Logon 将 DC 定位器资源记录在 DNS 中注册为此 DC。 如果在子域中的服务器上安装 DNS 服务器服务，此 DC 不能立即注册其记录。 这是因为它是当前隔离的因为恢复过程中，并将其主 DNS 服务器的一部分是林根 DNS 服务器。 使用相同的 IP 地址，因为它在发生灾难，以避免 DC 服务查找失败之前配置此计算机。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计出一个自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-方面的常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复单个的多域林中域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-与 Windows Server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md) 
