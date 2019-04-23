---
title: wbadmin
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ce8109ef6a0885abd02ef1dee9f11d21b7d7e9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858878"
---
# <a name="wbadmin"></a>wbadmin



可以备份和还原操作系统、 卷、 文件、 文件夹和应用程序从命令提示符。

若要配置的定期计划的备份，您必须**管理员**组。 若要执行此命令使用的所有其他任务，您必须**Backup Operators**或**管理员**组，或者您必须被委派了适当的权限。

您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符，请右键单击**命令提示符**，然后单击**以管理员身份运行**。)

## <a name="subcommands"></a>子命令

|子命令|描述|
|----------|-----------|
|[Wbadmin 启用备份](wbadmin-enable-backup.md)|配置并启用了定期计划的备份。|
|[Wbadmin 禁用备份](wbadmin-disable-backup.md)|禁用每日备份。|
|[Wbadmin 启动备份](wbadmin-start-backup.md)|运行一次性备份。 如果使用不带任何参数，将使用每日备份计划中的设置。|
|[Wbadmin 停止作业](wbadmin-stop-job.md)|停止当前运行的备份或恢复操作。|
|[wbadmin get 版本](wbadmin-get-versions.md)|列出了从本地计算机恢复的备份的详细信息或指定另一个位置，从另一台计算机。|
|[Wbadmin get 项](wbadmin-get-items.md)|列出备份中包含的项。|
|[wbadmin 开始恢复](wbadmin-start-recovery.md)|运行恢复的卷、 应用程序、 文件或指定的文件夹。|
|[Wbadmin get 状态](wbadmin-get-status.md)|显示当前运行的备份或恢复操作的状态。|
|[Wbadmin get 磁盘](wbadmin-get-disks.md)|列出当前在线的磁盘。|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|运行系统状态恢复。|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|运行系统状态备份。|
|[wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|删除一个或多个系统状态备份。|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|运行完全系统 （至少所有的卷包含操作系统的状态） 的恢复。 此子命令才可用，如果使用 Windows 恢复环境。|
|[Wbadmin 还原目录](wbadmin-restore-catalog.md)|从指定的存储位置在损坏的本地计算机上的备份目录的情况下恢复备份编录。|
|[Wbadmin delete 目录](wbadmin-delete-catalog.md)|删除本地计算机上的备份目录。 仅当此计算机上的备份目录已损坏且可用于还原目录的另一个位置处存储任何备份，请使用此子命令。|

#### <a name="additional-references"></a>其他参考

-   [备份和恢复](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [在 Windows PowerShell 中的 Windows Server Backup Cmdlet](https://technet.microsoft.com/library/jj902428.aspx)