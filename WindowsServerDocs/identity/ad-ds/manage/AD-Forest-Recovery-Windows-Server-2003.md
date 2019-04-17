---
title: "广告森林恢复-Windows Server 2003 恢复"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: daebbc65b9864554fde839c3865f507ab787e6b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>广告森林恢复-Windows Server 2003 恢复

>适用于：Windows Server 2003

本主题包含运行 Windows Server 2003 的域控制器 (Dc) 的森林恢复过程。 不与 Windows Server 2003 Dc 不同的常规森林恢复过程但的具体过程可能不同由于不同的工具。 例如，可以使用 Ntdsutil.exe 备份和还原运行 Windows Server 2003 Dc 的域控制器，而 Windows Server 备份或 Wbadmin.exe 用于运行 Windows Server 2008 或更高版本的域控制器。  
  
-   [备份系统状态数据](#Backing-up-the-System-State-data)  
  
-   [执行权威还原](#Performing-a-nonauthoritative restore)  
  
-   [安装和配置 DNS 服务器服务](#Install-and-configure-the-DNS-Server-service)  
  
  
## <a name="backing-up-the-system-state-data"></a>备份系统状态数据  
 使用以下步骤备份系统状态数据，以及所选的运行 Windows Server 2003 DC 当前备份操作的其他数据。 Windows Server 2003 包括 Ntbackup 工具，可用于备份系统状态的数据。  
  
 在会员**管理员**或**备份运营商**，或等效的最低要求备份文件和文件夹。   
  
 如果你将系统状态数据备份到保护带，并且备份计划指示，没有任何未使用的 media 可用，你可能需要使用可移动存储。 这样你的录像带免费媒体池中添加以便该备份可以使用它。  
  
 你仅可以在本地计算机上备份系统状态数据。 你不能备份在远程计算机上。  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>若要备份的域控制器上的系统状态数据的运行 Windows Server 2003  
  
1.  单击**开始**，指向**所有程序**，指向**附件**，指向**系统工具**，然后单击**备份**。  
  
2.  在**欢迎**页上，单击**高级模式**。  
  
3.  在**备份**选项卡上，选中任何驱动器、文件夹或你想要备份的文件的复选框。  
  
4.  选择**系统状态**复选框。  
  
5.  单击**启动备份**。  
  
  
## <a name="performing-a-nonauthoritative-restore"></a>执行权威还原  
 使用下面的过程中执行的运行 Windows Server 2003 DC 未授权还原。 在 Windows Server 2003 Active Directory 上执行权威还原，自动执行 SYSVOL 非授权恢复。 任何其他步骤不是必需的。  
  
> [!NOTE]
>  如果你还将重新安装 Windows Server 2003 操作系统，你可能会或可能不计算机加入域和操作系统的设置期间提供任何名称计算机。 不要安装 Active Directory。 重新安装后操作系统，直接转到第 4 步。  
  
 Windows Server 2003，域控制器上仅系统状态数据还原，你需要还重新安装任何之前恢复域控制器运行的软件应用程序。 还原在域中的第一个 DC 广告 DS 也将还原注册表，因为它们都系统状态数据的一部分。 如果你运行的这些域控制器上的任何应用程序，并且他们拥有的注册表中存储的任何信息，请记住这一点。  
  
 为了节省重新安装软件所需的时间，确定是否需要安装域控制器上的应用程序虚拟 DC 复制与兼容。 复制以节省时间和精力复制虚拟域控制器上将它们安装所需之前，可以源 DC 上安装此类应用程序。  
  
#### <a name="to-perform-a-nonauthoritative-restore"></a>若要执行权威还原  
  
1.  开始 DC 后，按 F8 键重启计算机中目录服务还原模式 (DSRM)。  
  
2.  选择**目录服务还原模式（仅用于 Windows 域控制器）**。  
  
3.  选择你想要在还原模式下启动的操作系统。  
  
4.  以 administrator 身份（你可以只有使用本地计算机帐户，未域登录选项可用）进行登录。  
  
5.  在命令提示符下，键入**ntbackup**，然后按 ENTER。  
  
6.  上**欢迎**页上，单击**高级模式**，然后选择**还原和管理媒体**选项卡 (不选择**还原向导**。)  
  
7.  选择相应的备份文件从还原，并确保**系统的磁盘**和**系统状态**复选框处于选中状态。  
  
8.  单击**开始还原**。  
  
9. 在完成恢复操作后，重启计算机。  
  
 使用下面的步骤执行上运行 Windows Server 2003 DC SYSVOL 授权（也称为主）还原。 仅在第一个在域中还原的 Windows Server 2003 DC 上执行此过程。  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>若要执行的 SYSVOL 授权恢复  
  
1.  以前的过程中执行步骤 1 到 8。  
  
2.  在**确认还原**对话框中，单击**高级**。  
  
3.  若要执行 SYSVOL 授权还原，请选中复选框**时还原复制的数据集，作为的主要数据所有副本将还原的数据**。  
  
    > [!NOTE]
    >  相当于设置的备份中的主要数据原样参与还原的数据**BurFlags**进入 D4 以下注册表子项下：  
    >   
    >  **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative 副本 Sets\\***GUID*  
  
4.  在完成恢复操作后，重启计算机。  
  
 
## <a name="install-and-configure-the-dns-server-service"></a>安装和配置 DNS 服务器服务  
 如果从备份中还原直流运行的 Windows Server 2003，你可以在无需连接到任何网络域控制器安装 DNS 服务器。  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>若要安装和配置 DNS 服务器服务  
  
1.  打开 Windows 组件向导。 若要打开该向导：  
  
    -   单击**开始**，单击**“控制面板”**，然后单击**添加或删除程序**。  
  
    -   单击**添加/删除 Windows 组件**。  
  
2.  在**组件**、选择**网络服务**复选框，然后依次单击**详细信息**。  
  
3.  在**网络服务的子组件**、选择**域名系统 (DNS)**复选框、单击**确定**，然后单击**下一步**。  
  
4.  你将看到提示后，在**复制其文件的**、键入 distribution 文件的完整路径，然后单击**确定**。  
  
     安装完成后，完成以下步骤来配置 DNS 服务器。  
  
5.  单击**开始**，指向**所有程序**，指向**管理工具**，然后单击**DNS**。  
  
6.  关键故障之前 DNS 服务器上创建了托管相同 DNS 域名称 DNS 区域。 有关详细信息，请参阅添加反向查找区域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。  
  
7.  配置它前关键故障不存在 DNS 数据。 例如：  
  
    -   配置 DNS 存储在广告 DS 的区域。 有关详细信息，请参阅更改区域类型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。  
  
    -   配置 DNS 域控制器（DC 定位器）定位器资源记录，以允许安全的动态更新的授权的区域。 有关详细信息，请参阅允许仅安全的动态更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。  
  
8.  确保家长 DNS 区域包含委派资源记录（恕服务器 (NS)，并将结合主机 () 资源记录）此 DNS 服务器上托管孩子区域。 有关详细信息，请参阅创建区域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。  
  
9. 配置 DNS 后，在命令提示符下，键入以下命令，并按 enter 键：  
  
     **网络停止 netlogon**  
  
10. 键入以下命令，并按 enter 键：  
  
     **网络开始 netlogon**  
  
    > [!NOTE]
    >  网络登录将 DC 定位资源记录 DNS 注册针对此直流。 如果你的孩子域中的服务器上安装 DNS 服务器服务，该 DC 不能立即注册其记录。 这是因为它目前独立属于恢复过程，并其主 DNS 服务器原样森林根 DNS 服务器。 将该计算机配置不如前灾难以避免 DC 服务查找故障的同一 IP 地址。

## <a name="next-steps"></a>后续步骤
-   [广告森林恢复-先决条件](AD-Forest-Recovery-Prerequisties.md)  
-   [广告森林恢复-在寻找自定义森林恢复套餐](AD-Forest-Recovery-Devising-a-Plan.md)  
- [广告森林恢复-找出问题](AD-Forest-Recovery-Identify-the-Problem.md)
-   [广告森林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [广告森林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
-   [广告森林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
-   [广告森林恢复-恢复单个域中多域森林](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [广告森林恢复-森林恢复与 Windows Server 2003 该域控制器](AD-Forest-Recovery-Windows-Server-2003.md) 
