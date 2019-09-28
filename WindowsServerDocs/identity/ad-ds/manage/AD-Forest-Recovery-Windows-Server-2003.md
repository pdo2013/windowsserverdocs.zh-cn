---
title: AD 林恢复-Windows Server 2003 恢复
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 43a2034cb707d4333abdce5f5b2b09d6c4b5a33a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390057"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>AD 林恢复-Windows Server 2003 恢复

>适用于：Windows Server 2003

本主题包括运行 Windows Server 2003 的域控制器（Dc）的林恢复过程。 林恢复的一般过程与 Windows Server 2003 Dc 没有任何不同，但特定的过程可能因不同的工具而有所不同。 例如，Ntdsutil.exe 可以用来备份和还原运行 Windows Server 2003 Dc 的 Dc，而 Windows Server 备份或 Wbadmin 用于运行 Windows Server 2008 或更高版本的 Dc。  
  
- [备份系统状态数据](#backing-up-the-system-state-data)  
- [执行非权威还原](#performing-a-nonauthoritative-restore)  
- [安装和配置 DNS 服务器服务](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>备份系统状态数据
使用以下过程来备份系统状态数据，以及你为当前备份操作选择的、运行 Windows Server 2003 的 DC 的任何其他数据。 Windows Server 2003 包含了 Ntbackup 工具，你可以使用它来备份系统状态数据。  
  
**管理员**或**备份操作员**的成员身份或同等身份是备份文件和文件夹所需的最低要求。   
  
如果要将系统状态数据备份到磁带，并且备份程序指示没有未使用的媒体，则可能必须使用可移动存储。 这会将磁带添加到可用媒体池，以便备份能够使用。  
  
只能备份本地计算机上的系统状态数据。 不能在远程计算机上备份。  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>若要备份运行 Windows Server 2003 的域控制器上的系统状态数据  
  
1. 单击 "**开始**"，指向 "**所有程序**"，指向 "**附件**"，指向 "**系统工具**"，然后单击 "**备份**"。  
2. 在**欢迎**页上，单击 "**高级模式**"。  
3. 在 "**备份**" 选项卡上，选中要备份的任何驱动器、文件夹或文件的复选框。  
4. 选中 "**系统状态**" 复选框。  
5. 单击 "**开始备份**"。  
  
## <a name="performing-a-nonauthoritative-restore"></a>执行非权威还原  

使用以下过程执行运行 Windows Server 2003 的 DC 的非权威还原。 通过在 Windows Server 2003 中的 Active Directory 上执行非权威还原，你将自动执行 SYSVOL 的非权威还原。 不需要其他步骤。  
  
> [!NOTE]
> 如果你还在重新安装 Windows Server 2003 操作系统，则你可能或不能将计算机加入到域中，并且你可以在安装操作系统的过程中为计算机指定任何名称。 不要安装 Active Directory。 重新安装操作系统后，请直接执行步骤4。  
  
在仅还原了系统状态数据的 Windows Server 2003 域控制器上，还需要在恢复前重新安装在 Dc 上运行的所有软件应用程序。 还原域中第一个 DC 上的 AD DS 也会还原注册表，因为它们都属于系统状态数据。 如果在这些 Dc 上运行任何应用程序，并且这些应用程序的任何信息存储在注册表中，请记住这一点。  
  
若要节省重新安装软件所需的时间，请确定是否需要在 Dc 上安装的应用程序与虚拟 DC 克隆兼容。 此类应用程序可以在克隆之前安装在源 DC 上，以节省在克隆的虚拟 Dc 上安装这些应用程序所需的时间和精力。  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>执行非权威还原
  
1. 启动 DC 后，按 F8 以目录服务还原模式（DSRM）重新启动计算机。  
2. 选择 "**目录服务还原模式" （仅限 Windows 域控制器）** 。  
3. 选择要在还原模式下启动的操作系统。  
4. 以管理员身份登录（您只能使用本地计算机帐户，没有域登录选项可用）。  
5. 在命令提示符下，键入**ntbackup**，然后按 enter。  
6. 在**欢迎**页上，单击 "**高级模式**"，然后选择 "**还原和管理媒体**" 选项卡。（请勿选择 "**还原向导**"。）  
7. 选择要从中还原的相应备份文件，并确保选中 "**系统磁盘**" 和 "**系统状态**" 复选框。  
8. 单击 "**开始还原**"。  
9. 还原操作完成后，重新启动计算机。  
  
使用以下过程在运行 Windows Server 2003 的 DC 上执行 SYSVOL 的权威（也称为主）还原。 仅在域中还原的第一个 Windows Server 2003 DC 上执行此过程。  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>执行 SYSVOL 的权威还原  
  
1. 执行上一过程中的步骤1到步骤8。  
2. 在 "**确认还原**" 对话框中，单击 "**高级**"。  
3. 若要对 SYSVOL 执行权威还原，请在**还原复制的数据集时选中此复选框，将还原的数据标记为所有副本的主数据**。  

   > [!NOTE]
   > 将还原的数据标记为备份中的主数据等效于在以下注册表子项下将**BurFlags**项设置为 D4：  
   >   
   > **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets @ no__t** *GUID*  

4. 还原操作完成后，重新启动计算机。  
  
## <a name="install-and-configure-the-dns-server-service"></a>安装和配置 DNS 服务器服务

如果从备份还原的 DC 正在运行 Windows Server 2003，则你可以安装 DNS 服务器而不将 DC 连接到任何网络。  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>安装和配置 DNS 服务器服务  
  
1. 打开 Windows 组件向导。 打开向导：  

   - 依次单击“开始”、“控制面板”，然后单击“添加或删除程序”。  
   - 单击 "**添加/删除 Windows 组件**"。  

2. 在 "**组件**" 中，选中 "**网络服务**" 复选框，然后单击 "**详细信息**"。  
3. 在 "**网络服务" 的子组件**中，选中 "**域名系统（DNS）** " 复选框，单击 **"确定**"，然后单击 "**下一步**"。  
4. 如果系统提示，请在 "**复制文件**" 中键入分发文件的完整路径，然后单击 **"确定"** 。  

   安装完成后，请完成以下步骤以配置 DNS 服务器。  

5. 单击 "**开始**"，指向 "**所有程序**"，指向 "**管理工具**"，然后单击 " **DNS**"。  
6. 为在关键故障发生之前在 DNS 服务器上托管的相同 DNS 域名创建 DNS 区域。 有关详细信息，请参阅添加正向查找区域（[https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)）。  
7. 配置在发生严重故障之前存在的 DNS 数据。 例如：  

   - 配置要存储在 AD DS 中的 DNS 区域。 有关详细信息，请参阅更改区域类型（[https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)）。  
   - 将域控制器定位器（DC 定位程序）资源记录的权威 DNS 区域配置为允许安全动态更新。 有关详细信息，请参阅只允许安全动态更新（[https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)）。  

8. 确保父 DNS 区域包含此 DNS 服务器上托管的子区域的委派资源记录（名称服务器（NS）和粘附主机（A）资源记录）。 有关详细信息，请参阅创建区域委派（[https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)）。  
9. 配置 DNS 之后，在命令提示符下，键入以下命令，然后按 ENTER：  

   **net stop netlogon**

10. 键入以下命令，然后按 Enter：  

    **net start netlogon**

    > [!NOTE]
    > Net Logon 将为此 DC 在 DNS 中注册 DC 定位器资源记录。 如果在子域中的某个服务器上安装 DNS 服务器服务，此 DC 将不能立即注册其记录。 这是因为它当前在恢复过程中是隔离的，其主 DNS 服务器是目录林根 DNS 服务器。 请将此计算机配置为与灾难之前相同的 IP 地址，以避免 DC 服务查找失败。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复多域林中的单个域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-通过 Windows Server 2003 域控制器恢复林](AD-Forest-Recovery-Windows-Server-2003.md) 
