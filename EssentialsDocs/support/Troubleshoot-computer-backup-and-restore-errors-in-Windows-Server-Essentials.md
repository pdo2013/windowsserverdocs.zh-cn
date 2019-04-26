---
title: Windows Server Essentials 中的计算机备份和还原错误疑难解答
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 06/25/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37e79661442ba9f66a564b6c6c8fb57db1978454
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812328"
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Windows Server Essentials 中的计算机备份和还原错误疑难解答

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

使用这些过程以解决 Windows Server Essentials 中的计算机备份问题，包括备份配置问题、不完整或不成功的备份、备份运行状况警报和文件、文件夹或完整系统还原的问题。  
  
> [!NOTE]
>  从 Windows Server Essentials 社区的最新疑难解答信息，请访问[Windows Server Essentials 论坛](https://social.technet.microsoft.com/Forums//winserveressentials/threads)。  
  
##  <a name="BKMK_TroubleshootBackupConfigurationIssues"></a> 对已连接的计算机的备份配置问题进行故障排除  
 使用以下过程解决在 Windows Server Essentials 服务器上备份的计算机的备份配置问题。  
  
### <a name="errors"></a>错误  
  
-   备份配置未成功完成  
  
-   收集计算机信息时出错  
  
-   从备份中删除计算机时出错  
  
### <a name="resolutions"></a>解决方法  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>解决在为连接的计算机配置备份时发生的错误  
  
1.  确保计算机已通过网络设备连接到网络。  
  
2.  确保计算机连接到的网络设备也已经连接到网络、已接通电源并且能正常工作。  
  
3.  请确保 Windows Server Client Backup Service 和 Windows Server Client Computer Backup Provider Service 在服务器上运行。  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>在服务器上启动计算机备份服务  
  
    1.  在服务器上，依次单击“开始”、“管理工具”，然后单击“服务”。  
  
    2.  向下滚动并单击“Windows Server Client Computer Backup Provider Service”。 如果该服务的状态不是“已启动” ，则右键单击该服务，然后单击“启动” 。  
  
    3.  单击“Windows Server Client Computer Backup Service”。 如果该服务的状态不是“已启动” ，则右键单击该服务，然后单击“启动” 。  
  
    4.  关闭“服务”。  
  
4.  确保 Windows Server Client Computer Backup Provider Service 正在客户端计算机上运行。  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>在客户端计算机上启动计算机备份服务  
  
    1.  在客户端计算机上，单击“开始”，在“搜索程序和文件”文本框中键入“Services”，然后按 Enter 键。  
  
    2.  向下滚动并单击“Windows Server Client Computer Backup Provider Service”。 如果该服务的状态不是“已启动” ，则右键单击该服务，然后单击“启动” 。  
  
    3.  关闭“服务”。  
  
5.  检查警报以确定备份数据库中是否存在错误。 如果存在错误，请按照该警报中的说明修复备份数据库。  
  
6.  从计算机上卸载 Windows Server Essentials 连接器软件，并重新安装它。 有关详细信息，请参阅主题[卸载连接器软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安装连接器软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。  
  
##  <a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a> 对未正确完成的备份进行故障排除  
 当备份的状态为失败时，备份的任何部分都未成功并且没有可供你还原的数据。 但是，当备份具有不完整的状态时，不是所有在备份配置中指定的项都已备份，但某些数据可能可以供你还原。  
  
### <a name="errors"></a>错误  
  
-   备份不完整  
  
-   备份失败  
  
### <a name="resolutions"></a>解决方法  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>标识未成功备份的卷  
  
1.  打开 Windows Server Essentials 仪表板，然后单击“计算机和备份”。  
  
2.  单击备份未成功完成的计算机的名称，然后单击“任务”  窗格中的“查看计算机属性”  。  
  
3.  单击未成功完成的备份，然后单击“查看详细信息”。  
  
4.  在“备份详细信息”对话框中，绿色复选标记将显示在每个已成功备份的卷的状态中。  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>解决卷的未成功备份问题  
  
1.  请确保硬盘已连接到计算机上、处于打开状态并且能正常工作。  
  
2.  运行 **chkdsk /f /r** 来修复硬盘 (**/f**) 上的任何错误并从任何坏扇区 (**/r**) 恢复可读信息。 有关运行 **chkdsk**的详细信息，请参阅 [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562)。  
  
3.  请确保在运行备份时计算机未关闭或从网络断开连接。  
  
4.  请确保每个卷上有足够的可用空间供备份运行。 备份需要客户端计算机上的一些额外磁盘空间来创建 VSS 快照。 在不是系统保留的任何卷上，必须至少有 10% 的可用磁盘空间可供使用。 在系统保留卷上，如果卷的大小小于 500 MB，则 VSS 需要 32 MB 的可用空间来创建快照；如果卷是 500 MB 或更大，则 VSS 需要 320 MB 的可用空间。  
  
     如果在卷上没有足够的可用空间，请尝试以下解决方法之一：  
  
    -   扩展该卷。 你可以扩展除系统保留卷以外的任何基本或动态卷。  
  
        ###### <a name="to-extend-a-volume"></a>扩展卷  
  
        1.  在控制面板中，单击“系统和安全” 。  
  
        2.  在“管理工具” 下，单击“创建和格式化硬盘分区” 。  
  
        3.  右键单击想要扩展的卷。 如果“扩展卷”  已启用，则选择该选项。 如果未启用该选项，则不能扩展卷。  
  
        4.  按照扩展卷向导中的步骤扩展该卷。  
  
    -   删除卷中的内容以获得更多可用空间。  
  
        > [!NOTE]
        >  如果你需要释放系统保留卷上的空间，你可以将系统恢复映像移动到其他卷。 有关说明，请参阅 [部署系统恢复映像](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx)。  
  
    -   从客户端备份中排除卷。 仅当维护卷上的数据的备份副本并不重要时，才执行此操作。  
  
        > [!WARNING]
        >  如果从客户端备份中排除系统保留卷，将不会备份客户端系统，并且你将无法在计算机上执行完整系统还原。  
  
5.  检查服务器上的其他警报，可能会指示服务器上没有足够的磁盘空间用于成功完成备份。 按照警报中的说明来修复该问题。  
  
6.  在命令提示符下运行 **vssadmin** 来对卷影复制服务 (VSS) 问题进行故障排除。 有关 **vssadmin**的信息，请参阅 [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332)。  
  
##  <a name="BKMK_TroubleshootBackupHealthAlertIssues"></a> 解决备份运行状况警报问题  
  
### <a name="errors"></a>错误  
  
-   Windows Server Solutions Computer Backup Provider Service 停止工作  
  
-   Windows Server Solutions Client Computer Backup Provider Service 停止工作  
  
### <a name="resolutions"></a>解决方法  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>解决备份运行状况警报问题  
  
1.  如果一个警报告知你备份数据库有问题，请按照警报中的说明修复该问题。  
  
2.  如果一个警报告知你备份服务未运行，请在收到此错误消息的服务器或客户端计算机上尝试启动该服务。  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>在服务器上启动备份服务  
  
    1.  在服务器上，依次单击“开始”、“管理工具”，然后单击“服务”。  
  
        > [!NOTE]
        >  如果你在远程管理服务器，则必须使用远程桌面连接访问服务器桌面。 有关使用远程桌面连接的信息，请参阅 [使用远程桌面连接连接到另一台计算机](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection)。  
  
    2.  向下滚动并单击“Windows Server Client Computer Backup Provider Service”。 如果该服务的状态不是“已启动” ，则右键单击该服务，然后单击“启动” 。  
  
    3.  单击“Windows Server Client Computer Backup Service”。 如果该服务的状态不是“已启动” ，则右键单击该服务，然后单击“启动” 。  
  
    4.  关闭“服务”。  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>在客户端计算机上启动备份服务  
  
    1.  在客户端计算机上，单击“开始”，在“搜索程序和文件”文本框中键入“服务，然后单击 Enter。  
  
    2.  右键单击“Windows Server Client Computer Backup Provider Service” ，然后单击“开始” 。  
  
    3.  关闭“服务” 。  
  
3.  检查客户端计算机或服务器上的事件日志，以查看是否存在与备份服务或驱动程序相关的问题。  
  
4.  重新启动收到错误消息的服务器或客户端计算机。  
  
5.  检查运行状况警报，了解可能会影响客户端备份的其他问题。  
  
##  <a name="BKMK_FileAndFolder"></a> 对文件或文件夹还原进行故障排除  
  
### <a name="errors"></a>错误  
  
-   文件或文件夹还原未成功完成。  
  
### <a name="resolutions"></a>解决方法  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>解决失败的文件或文件夹还原问题  
  
1.  确保计算机已通过网络设备连接到网络。  
  
2.  确保计算机连接到的网络设备也已经连接到网络、已接通电源并且能正常工作。  
  
3.  检查警报以确定备份数据库中是否存在错误。 如果存在错误，请按照该警报中的说明修复备份数据库。  
  
4.  尝试从另一个备份中还原文件或文件夹。  
  
5.  请确保“Windows Server 解决方案计算机还原驱动程序”已安装并且工作正常。  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>检查 Windows Server 解决方案计算机还原驱动程序的状态  
  
    1.  单击**启动**，类型**设备管理器**中**搜索程序和文件**文本框中，然后单击 ENTER。  
  
    2.  在设备管理器中，单击“系统设备”，滚动到“Windows Server 解决方案计算机还原驱动程序”。  
  
    3.  如果未显示该驱动程序：  
  
        1.  使用管理员权限打开命令提示符，并运行以下命令：  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe?  �i**  
  
        2.  刷新设备管理器。 该驱动程序应该会出现。  
  
    4.  如果显示的图标是计算机监视器，则该驱动程序已安装并运行正常。 关闭设备管理器。  
  
    5.  如果显示的图标不是计算机监视器  
  
        1.  右键单击“Windows Server 解决方案计算机还原驱动程序” ，然后单击“属性” 。  
  
        2.  单击“驱动程序”选项卡，然后单击“更新驱动程序”。  
  
        3.  单击“自动搜索更新的驱动程序软件” ，然后按照说明更新驱动器。  
  
    6.  关闭设备管理器。  
  
6.  从计算机上卸载 Windows Server Essentials 连接器软件，并重新安装它。 有关详细信息，请参阅主题[卸载连接器软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安装连接器软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。  
  
##  <a name="BKMK_Troubleshootfullsystemrestoreissues"></a> 解决完整系统还原的问题  
  
### <a name="errors"></a>错误  
  
-   在进行完整系统还原之后无法登录到客户端计算机。  
  
### <a name="resolutions"></a>解决方法  
 如果更改了计算机的名称，并且以后需要还原到在更改计算机名称之前保存的备份，则在还原后，当你尝试在域帐户下进行登录时，你将收到此错误：“服务器上的安全数据库没有此工作站信任关系的计算机帐户。” 若要重新获得对计算机的网络访问，请删除连接器软件，将计算机从 Windows 域中删除，然后重新连接到服务器。  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>在更改计算机名称后重新获得对已还原计算机的网络访问  
  
1.  以本地管理员身份登录计算机。  
  
2.  卸载连接器软件。 有关详细信息，请参阅 [Uninstall the Connector software](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。  
  
3.  从域中删除计算机。 有关详细信息，请参阅 [Remove a computer from a Windows domain](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)。  
  
4.  重新将计算机连接到服务器。 有关详细信息，请参阅[如何将计算机连接到服务器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
## <a name="see-also"></a>请参阅  
  
-   [支持 Windows Server Essentials](Support-Windows-Server-Essentials.md)