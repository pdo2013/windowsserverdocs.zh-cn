---
title: "解决计算机备份和还原 Windows Server Essentials 中的错误"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>解决计算机备份和还原 Windows Server Essentials 中的错误

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

使用以下步骤进行疑难解答中的 Windows Server Essentials，包括备份配置问题、完整或无法从备份、备份健康警报和文件、文件夹或全系统恢复的问题的计算机备份。  
  
> [!NOTE]
>  来自社区的 Windows Server Essentials 的最新的疑难解答信息，请访问[Windows Server Essentials 论坛](https://social.technet.microsoft.com/Forums//winserveressentials/threads)。  
  
##  <a name="BKMK_TroubleshootBackupConfigurationIssues"></a>疑难解答备份配置为已连接的计算机的问题  
 使用这些步骤解决 Windows Server Essentials 服务器备份的计算机的备份配置的问题。  
  
### <a name="errors"></a>错误  
  
-   备份配置未成功完成  
  
-   错误计算机收集信息  
  
-   从备份错误删除计算机  
  
### <a name="resolutions"></a>分辨率  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>若要配置已连接的计算机的备份时发生的错误疑难解答  
  
1.  请确保你的计算机已连接到网络通过网络设备。  
  
2.  请确保的计算机连接到的网络设备还会连接到网络，已打开电源，并且运行正常。  
  
3.  请确保在服务器上运行 Windows Server 客户端备份服务和 Windows Server 客户端计算机备份提供程序服务。  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>在服务器上启动计算机备份服务  
  
    1.  在服务器上，单击**开始**，单击**管理工具**，然后单击**服务**。  
  
    2.  向下滚动到和单击**Windows Server 客户端计算机备份提供程序服务**。 如果不是服务的状态**已启动**，该服务，右键单击，然后单击**开始**。  
  
    3.  单击**Windows Server 客户端计算机备份服务**。 如果不是服务的状态**已启动**，该服务，右键单击，然后单击**开始**。  
  
    4.  关闭**服务**。  
  
4.  请确保在客户端计算机上运行的 Windows Server 客户端计算机备份提供程序服务。  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>在客户端计算机上启动计算机备份服务  
  
    1.  在客户端计算机上，单击**开始**，类型**服务**中**搜索程序和文件**文本框中并按 Enter。  
  
    2.  向下滚动到和单击**Windows Server 客户端计算机备份提供程序服务**。 如果不是服务的状态**已启动**，该服务，右键单击，然后单击**开始**。  
  
    3.  关闭**服务**。  
  
5.  检查以确定备份数据库中是否有错误的警报。 如果错误，请按照通知中的说明操作来修复备份的数据库。  
  
6.  从计算机中，卸载 Windows Server Essentials 接头软件，然后重新安装它。 有关详细信息，请参阅主题[卸载接头软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安装接头软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。  
  
##  <a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>未正确完成备份疑难解答  
 备份不成功状态时，已成功中的任何部分数据不是备份的适用于你要还原。 但是时备份不完整的状态，, 并非所有项目中都指定备份配置已备份，但某些数据也许可以适用于你要还原。  
  
### <a name="errors"></a>错误  
  
-   备份不完整  
  
-   无法从备份  
  
### <a name="resolutions"></a>分辨率  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>若要确定未备份成功的卷  
  
1.  打开 Windows Server Essentials 仪表板，然后单击**计算机和备份**。  
  
2.  单击其中备份是否未成功，完成，然后单击计算机的名称**查看电脑属性**中**任务**窗格。  
  
3.  单击备份，是否不成功完成，然后单击**查看详细信息**。  
  
4.  在**备份详细信息**对话框中，而绿色检查显示在每个已成功备份的卷的状态。  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>若要解决卷无法从备份  
  
1.  请确保已连接到计算机，已启用、硬盘且正常工作正常。  
  
2.  运行**chkdsk /f /r**若要在硬盘上解决的任何错误 (**/f**) 以及如何恢复坏扇区较高的可读性信息 (**/r**)。 有关详细信息运行**chkdsk**，请参阅[CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562)。  
  
3.  请确保计算机未被关机或网络断开连接时运行的备份。  
  
4.  请确保备份运行为每个卷上没有足够的可用空间。 备份需要创建 VSS 快照客户端计算机上的一些其他磁盘空间。 在不保留的系统任何音量，必须是免费的可用磁盘空间至少 10%。 系统保留卷上，如果卷的大小是不超过 500 MB，VSS 需要 32 MB 的可用空间来创建快照;音量是否 500 MB 或更高版本，VSS 需要 320 MB 的可用空间。  
  
     如果没有足够的可用空间的卷上可用，请尝试这些解决方法之一：  
  
    -   将扩展音量。 你可以扩展除系统保留音量任何基本或动态音量。  
  
        ###### <a name="to-extend-a-volume"></a>延长音量  
  
        1.  在控制面板中，单击**系统和安全**。  
  
        2.  下**管理工具**，单击**创建和格式化硬盘分区**。  
  
        3.  右键单击你想要扩展的音量。 如果**扩展卷**已启用，请选择该选项。 如果未启用该选项，能扩展音量。  
  
        4.  按照扩展卷延长卷向导中的步骤。  
  
    -   删除卷提供更多空间中的内容。  
  
        > [!NOTE]
        >  如果你需要释放一些空间系统保留卷上，你可以移动到不同的卷系统恢复映像。 有关说明，请参阅[部署系统恢复映像](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx)。  
  
    -   从客户端备份排除音量。 仅当它并不重要对你来说维护卷上的数据的备份副本，则执行此操作。  
  
        > [!WARNING]
        >  如果系统保留音量排除客户端备份，不备份系统客户端，并将无法在计算机上执行全面的系统还原。  
  
5.  检查可能表示上备份要成功完成服务器没有足够的磁盘空间服务器上的其他通知。 按照说明警报中的来解决此问题。  
  
6.  运行**vssadmin**在 command prompt 下进行故障排除卷影副本服务 (VSS) 的问题。 了解有关**vssadmin**，请参阅[VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332)。  
  
##  <a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>备份健康警报问题的疑难解答  
  
### <a name="errors"></a>错误  
  
-   Windows Server 解决方案计算机备份提供商服务停止工作  
  
-   Windows Server 解决方案客户端计算机备份提供商服务停止工作  
  
### <a name="resolutions"></a>分辨率  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>若要解决备份健康提醒  
  
1.  如果警报通知备份数据库有问题，请按照通知中的说明来解决此问题。  
  
2.  如果警报通知，备份服务无法运行的，请尝试服务器或收到错误消息的位置的客户端计算机上启动该服务。  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>若要开始服务器上的备份服务  
  
    1.  在服务器上，单击**开始**，然后单击**管理工具**，然后单击**服务**。  
  
        > [!NOTE]
        >  如果该服务器管理远程，必须使用远程桌面连接访问 server 桌面。 使用远程桌面连接有关的信息，请参阅[连接到另一台计算机使用远程桌面连接](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection)。  
  
    2.  向下滚动到和单击**Windows Server 客户端计算机备份提供程序服务**。 如果不是服务的状态**已启动**，该服务，右键单击，然后单击**开始**。  
  
    3.  单击**Windows Server 客户端计算机备份服务**。 如果不是服务的状态**已启动**，该服务，右键单击，然后单击**开始**。  
  
    4.  关闭**服务**。  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>在客户端计算机上启动备份服务  
  
    1.  在客户端计算机上，单击**开始**，类型**服务**中**搜索程序和文件**文本框，然后单击 enter 键。  
  
    2.  右键单击**Windows Server 客户端计算机备份提供程序服务**，然后单击**开始**。  
  
    3.  关闭**服务**。  
  
3.  检查的客户端计算机或相关备份服务或驱动程序问题的服务器上的事件日志。  
  
4.  重新启动的服务器或收到错误消息的位置的客户端计算机。  
  
5.  检查运行状况可能会产生影响客户端备份的其他问题的警报。  
  
##  <a name="BKMK_FileAndFolder"></a>解决文件或文件夹还原  
  
### <a name="errors"></a>错误  
  
-   文件或文件夹还原未成功完成。  
  
### <a name="resolutions"></a>分辨率  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>解决了无法从的文件或文件夹还原  
  
1.  请确保你的计算机已连接到网络通过网络设备。  
  
2.  请确保的计算机连接到的网络设备还会连接到网络，已打开电源，并且运行正常。  
  
3.  检查以确定备份数据库中是否有错误的警报。 如果错误，请按照通知中的说明操作来修复备份的数据库。  
  
4.  请尝试从另一台的备份中还原文件或文件夹。  
  
5.  请确保**Windows Server 解决方案计算机还原驱动程序**安装和正常工作。  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>若要查看 Windows Server 解决方案计算机还原驱动程序的状态  
  
    1.  单击**开始**，类型**设备管理器**中**搜索程序和文件**文本框，然后单击 enter 键。  
  
    2.  在设备管理器中，单击**系统设备**，滚动到**Windows Server 解决方案计算机还原驱动程序**。  
  
    3.  如果未显示驱动程序：  
  
        1.  打开具有管理员权限的命令提示符下，并运行以下命令：  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe？  我**  
  
        2.  恢复设备管理器。 应该会显示驱动程序。  
  
    4.  如果显示的图标是计算机监视器，驱动程序已安装并运行正常。 关闭设备管理器。  
  
    5.  如果显示的图标显示不是计算机监视器  
  
        1.  右键单击**Windows Server 解决方案计算机还原驱动程序**，然后单击**属性**。  
  
        2.  单击**驱动程序**选项卡，然后单击**更新驱动程序**。  
  
        3.  单击**自动搜索更新的驱动程序软件**，然后按照说明进行操作更新的驱动程序。  
  
    6.  关闭设备管理器。  
  
6.  从计算机中，卸载 Windows Server Essentials 接头软件，然后重新安装它。 有关详细信息，请参阅主题[卸载接头软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安装接头软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。  
  
##  <a name="BKMK_Troubleshootfullsystemrestoreissues"></a>解决了全面的系统还原  
  
### <a name="errors"></a>错误  
  
-   不能在客户端计算机上后登录完整的系统还原。  
  
### <a name="resolutions"></a>分辨率  
 如果你更改计算机的名称，并且需要稍后到的计算机名称更改之后还原，你域帐户，你尝试登录时之前保存的备份中还原你会收到此错误:"服务器上的安全数据库不具有此工作站信任关系计算机帐户"。 若要重新获得对计算机的网络访问权限，删除接头软件，计算机中删除的 Windows 域中，然后重新连接到服务器。  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>更改计算机名称后重新获取网络访问权限还原计算机  
  
1.  本地管理员身份登录到计算机。  
  
2.  卸载接头软件。 有关详细信息，请参阅[卸载接头软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。  
  
3.  将计算机从域移除。 有关详细信息，请参阅[从 Windows 域删除一台计算机](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)。  
  
4.  重新连接到服务器的计算机。 有关详细信息，请参阅[将计算机连接到服务器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
## <a name="see-also"></a>请参阅  
  
-   [支持 Windows Server Essentials](Support-Windows-Server-Essentials.md)