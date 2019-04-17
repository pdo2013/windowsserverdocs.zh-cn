---
title: "还原或修复你运行的 Windows Server Essentials 服务器"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1ebc539928d13b0d34dfe5a0ee57ce6e98088e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>还原或修复你运行的 Windows Server Essentials 服务器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 本主题提供概述，并且支持还原或维修运行 Windows Server Essentials 服务器过程，并包含以下部分：  
  
-   [服务器系统还原的概述](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [还原或修复系统驱动器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [还原文件和服务器上的文件夹](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a>服务器系统还原的概述  
 服务器时执行还原的状态影响可用的还原方法，并且你可以执行的方式全面还原。  
  
 还原服务器的最常见原因是：  
  
-   不能 inoculated 或删除病毒服务器上。  
  
-   服务器配置设置不正确，并且你无法开头服务器。  
  
-   更换系统驱动器。  
  
-   要撤销服务器，并且想要还原到一个新的服务器。  
  
 从备份，或者还原服务器，或者你可以还原到出厂默认设置的服务器。  
  
-   [从备份中还原该服务器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
-   [重置为出厂默认设置的服务器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a>从备份中还原该服务器  
 此部分中提供的备份选择哪种类型的指南。  
  
 如果有可用的备份，还原服务器的最佳选择是用于从外部的备份中还原制造商的安装媒体。 还原将从你选择的备份中恢复服务器设置和文件夹。 只需配置设置和还原备份之后创建的数据。  
  
 当你选择要通过从之前的备份中还原文件来恢复你服务器时，必须选择你想要还原的特定备份，并且你必须具有有效备份文件直接连接到服务器的外部硬盘上：  
  
-   **如果你具有最新成功备份服务器**，你知道的备份包含所有重要数据，您的选择一目了然。 你只需重新创建你最后一个很好备份和重新配置设置进行更改之后备份之后创建的数据。  
  
-   **如果你要还原服务器，由于存在病毒**，你知道备份发生接收病毒之前的选择。 你可能需要回退几天时间来选择已清理备份。  
  
-   **如果你要还原服务器由于坏配置设置**，选择你知道备份发生之前设置导致服务器的问题的更改的配置。  
  
 当你从备份中还原时，确切的过程和需要解答后续问题取决于硬盘服务器和是否有替换系统驱动器上数：  
  
-   **如果该服务器具有单个硬盘驱动器，并且不更换驱动器**，当您恢复服务器驱动器分区信息将保持不变。 还原向系统卷时，并保留剩余卷上的数据。  
  
-   **如果该服务器具有单个硬盘驱动器并且驱动器将替换**还原向系统卷时，，然后你必须手动将文件夹还原到数据量。 任何非默认共享的文件夹需要创建，因为它们不会创建重新创建服务器存储时。  
  
-   **如果服务器具有多个硬盘和驱动器 0（包含向系统卷）都不能替代**，当您恢复服务器驱动器分区信息将保持不变。 还原向系统卷时，并保留所有剩余卷上的数据。  
  
-   **如果服务器具有多个硬盘和驱动器 0（包含向系统卷）将替换**还原向系统卷时，，然后手动必须还原以前 0 驱动器存储的任何共享的文件夹。  
  
###  <a name="BKMK_FactoryReset"></a>重置为出厂默认设置的服务器  
 如果没有，或你想或所需执行的全系统还原，而还原以前的服务器配置其他原因，你可以还原备份，你可以执行重置为出厂默认设置使用安装或恢复媒体服务器硬件制造商提供的服务器还原。  
  
 通过重置为出厂默认设置还原你的服务器时，将会删除所有现有的设置和已安装的应用程序，你的服务器上，，并且您必须再次配置服务器。 恢复出厂设置后，重新启动你的服务器。  
  
 当你执行出厂重置时，你可以选择保护你的数据或删除它，使用以下几种效果：  
  
-   如果你选择让你的所有数据、的所有数据向系统卷被删除，但其他卷上的数据将保留。  
  
    > [!CAUTION]
    >  如果磁盘设置默认设置不匹配，磁盘上的所有数据将被都删除。 如果更换系统的磁盘，新的磁盘必须大于原始磁盘的系统音量。  
    >   
    >  如果无法读取，系统驱动器分区信息或替换系统驱动器，系统驱动器上的所有数据将被都删除，即使你选择要保留你的所有数据。  
  
-   如果你打算停止使用或用途服务器时，选择要删除您的所有数据。 除了服务器配置、其他设置，并向系统卷上的数据，所有其他数据将被删除，，并重新格式化服务器上的所有硬盘。  
  
> [!NOTE]
>  如果你执行出厂重置之前，在服务器上，配置存储空间，你应使用**高级**部分**管理存储空间**主机手动删除所有的存储空间。  
  
 恢复出厂设置后，你将需要执行下列任务：  
  
-   **重新启动。** 在服务器上，使用该配置服务器向导时都重新输入配置设置。 要配置的客户端计算机远程托管的 Windows Server Essentials 服务器，请打开 web 浏览器，，然后键入**http://***< YourServerName\ >*地址栏中。  
  
-   **重新连接到服务器的客户端计算机。** 如果之前已将计算机连接到服务器，你必须从计算机卸载 Windows Server Essentials 接头软件之前再次将计算机连接到服务器。 有关详细信息，请参阅[卸载接头软件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[将计算机连接到服务器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
##  <a name="BKMK_Restore"></a>还原或修复系统驱动器  
 还原服务器的第一步是还原或修复服务器系统驱动器。 还原系统驱动器后，你将执行任何需要将数据驱动器上的服务器和还原中还原丢失了任何共享恢复。  
  
 三种方法是适用于执行还原：  
  
-   [还原或修复服务器使用安装媒体](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1)。 使用安装媒体服务器制造商提供的从备份中还原。  
  
-   **使用安装媒体还原为出厂设置的默认服务器**。 若要了解如何执行此操作服务器上，请参阅 server 制造商提供的文档。  
  
-   [还原或重置使用恢复 DVD 的客户端计算机服务器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)。 如果你需要恢复运行的 Windows Server Essentials 远程管理的服务器，必须使用还原 DVD 服务器制造商提供的客户端计算机从执行还原。  
  
###  <a name="BKMK_Restore_1"></a>还原或修复服务器使用安装媒体  
 以下过程介绍了如何使用 Windows Server Essentials 安装媒体从备份中还原系统服务器驱动器。 （若要了解如何使用安装媒体还原到出厂默认设置，请参阅 server 制造商提供的文档。）  
  
> [!NOTE]
>  如果你要还原到一个新的服务器数据服务器使用存储空间，你应该恢复驱动器系统第一次，然后登录到 Windows Server Essentials 仪表板上、配置方式类似为旧的服务器上的存储空间，然后恢复数据量。  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>若要从备份使用安装媒体还原服务器系统驱动器  
  
1.  在服务器 DVD 驱动器中插入 Windows Server Essentials 安装 DVD 服务器，重新启动，然后按任意键从 DVD 启动。  
  
    > [!NOTE]
    >  如果未自动启动恢复过程，检查以确保 DVD 驱动器显示启动菜单中的第一个服务器的 BIOS 设置。  
  
     --或  
  
     如果制造商预先加载的服务器上的安装媒体，请在启动时从安装媒体中启动按 f8 键。  
  
2.  Windows Server 后文件将加载，选择你的语言和其他首选项，然后单击**下一步**。  
  
3.  在向导中的下一页上，单击**修复你的计算机**。  
  
    > [!CAUTION]
    >  不要选择**立即安装**选项。 该选项将指导你删除所有配置设置和系统驱动器上的所有数据的全系统安装。  
  
4.  在**选择一个选项**页上，单击**疑难解答**。  
  
5.  在**系统映像恢复**页上，选择当前系统？任一**Windows Server Essentials**或**Windows Server Essentials**。  
  
     打开重新镜像重置你的计算机向导。  
  
6.  在**选择系统映像备份**页上，你可以选择使用最新备份，或者你可以选择之前的备份。 系统将还原到还原或维修服务器你选择的备份的时间位于的状态。 已添加的数据或更改设置后保存的备份时所做的都必须重新创建。  
  
     选择以下选项之一，然后单击**下一步**:  
  
    -   **使用最新可用系统映像（推荐）**  
  
    -   **选择一个系统映像**  
  
    > [!NOTE]
    >  如果你具有最新成功备份服务器，并且你知道的备份包含所有重要数据，你的选择是非常简单。 你只需重新创建你最后一个很好备份和重新配置设置进行更改之后备份之后创建的数据。  
    >   
    >  如果你要还原服务器由于病毒，请选择知道发生接收病毒之前的备份。 你可能需要回退几天时间来选择已清理备份。  
    >   
    >  如果你要还原服务器由于坏配置设置，请选择知道发生导致该问题在服务器配置设置更改之前的备份。  
  
7.  按照该向导中的说明完成系统还原。  
  
8.  已成功还原服务器后，如果你使用过，删除安装 DVD 重启服务器。  
  
> [!NOTE]
>  若要还原和共享文件夹服务器上的，你可能需要执行其他步骤。 有关详细信息，请参阅[还原的文件和文件夹的服务器上](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)。  
  
###  <a name="BKMK_Restore_2"></a>还原或重置使用恢复 DVD 的客户端计算机服务器  
 在 Windows Server Essentials，你可以从启动服务器可启动 U 盘创建、，然后你恢复服务器来自客户端计算机使用恢复收到来自制造商服务器的 DVD。 客户端计算机必须打开服务器所在的网络。 此方法也适用于 Windows Server Essentials。  
  
 下面的过程中提供用于执行服务器还原常规步骤。 步骤是从备份中还原或恢复到出厂默认设置同样适用。 详细说明，请参阅 server 制造商提供的文档。  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>若要还原或重置使用恢复 DVD 的客户端计算机服务器  
  
1.  插入你收到的客户端计算机服务器制造商提供的 Windows Server Essentials 服务器恢复媒体。  
  
     恢复您的服务器向导将打开。  
  
2.  按照向导中的说明，若要创建可启动 U 盘上，您将使用服务器进入恢复模式。  
  
3.  恢复您的服务器向导准备可启动 U 盘后，则在服务器上，插入盘，然后开始在恢复模式的服务器。 若要了解如何在恢复模式下启动你的服务器，请服务器硬件制造商的相关文档。  
  
     服务器进入恢复模式后，则恢复您的服务器向导找到该服务器，并建立连接，然后。  
  
4.  按照该向导中的说明完成还原服务器。  
  
> [!NOTE]
>  这种方法服务器恢复忽略恢复过程中连接到服务器的外部存储设备。 如果你想要清除存储外部设备上的数据，你必须执行此操作手动。  
  
> [!NOTE]
>  如果你在服务器上，创建其他共享的文件夹，从备份还原数据后，服务器可能无法识别的其他共享的文件夹。 你必须重新共享这些文件夹。 有关详细信息，请参阅[还原的文件和文件夹的服务器上](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)。  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a>还原文件和服务器上的文件夹  
 具体取决于你用于恢复或修复你的服务器和使用存储在服务器类型的方法，你可能需要还原系统驱动器之后恢复数据量。 在某些情况下，你可能需要再次共享现有的文件夹，以便服务器可识别它们。  
  
 时，你可能需要还原的文件和文件夹，以下是一些有关的示例：  
  
-   [从 server 备份中还原的文件和文件夹](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup)。 更换系统的磁盘，或是否无法读取系统硬盘上分区的信息，你可以还原系统，但无法从磁盘上的其他卷还原数据。 若要从其他数据量还原的文件和文件夹，必须使用还原的文件和文件夹向导。  
  
-   [还原在服务器上的共享的文件夹](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders)。 如果你在服务器上，创建其他共享的文件夹，从备份中还原系统驱动器后，共享的文件夹数据分区上仍或已还原到数据分区中，但服务器可能无法识别。 你必须重新共享这些文件夹。  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a>从 server 备份中还原的文件和文件夹  
 还原文件和文件夹向导可帮助你保护你的数据，如果您的硬盘停止工作或意外删除你的文件。 与 Windows Server Essentials 备份，可以创建您的硬盘上的所有数据副本，并在外部存储设备上存储数据。 硬盘空间原始数据意外擦除，覆盖，或者不可由于故障，你可以从备份还原数据。 还原文件或文件夹向导可帮助你从现有的备份中还原单个文件或文件夹、多个文件或文件夹或整的硬盘。  
  
 系统恢复后，你可能需要使用还原的文件和文件夹向导还原文件和文件夹的未被恢复过程中保留。 例如，取代系统的磁盘，或是否无法读取系统硬盘上分区的信息，你无法从系统上的其他卷还原数据。  
  
> [!NOTE]
>  你无法使用还原的文件和文件夹向导还原完整的系统驱动器。 有关如何还原完整的系统的信息，请参阅[还原或修复服务器使用安装媒体](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1)或[还原或重置你的客户端计算机使用恢复 DVD 服务器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)。  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>若要从 server 备份中还原的文件和文件夹  
  
1.  打开 Windows Server Essentials 仪表板，然后单击**设备**选项卡。  
  
2.  单击服务器的名称，然后单击**还原文件或文件夹的服务器**中**任务**窗格。  
  
     还原的文件和文件夹向导将打开。  
  
3.  按照该向导中的说明来还原文件或文件夹。  
  
> [!WARNING]
>  有关备份和还原的文件和文件夹的详细信息，请参阅[管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_ConfigreSharedFolders"></a>还原在服务器上的共享的文件夹  
 还原服务器的系统驱动器，如果或已还原到数据分区仍在数据分区上的共享的文件夹后，你可能需要配置服务器识别文件夹的顺序共享的文件夹。 以下过程介绍了如何添加共享的文件夹之前已共享。  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>将现有的文件夹添加到服务器的共享文件夹  
  
1.  在文件资源管理器中找到在硬盘上的共享的文件夹。  
  
2.  右键单击共享文件夹中，单击**属性**，单击**共享**选项卡，然后再将其记下文件夹权限。  
  
3.  登录到 Windows Server Essentials 仪表板上，单击**存储**选项卡，然后单击**添加文件夹**中**服务器文件夹任务**窗格。  
  
     添加文件夹向导将打开。  
  
4.  键入一个名称中共享**名称**框。  
  
5.  单击**浏览**，导航到*< drive\ > \\ < ServerName\ >*\ServerFolders (例如*d:\Contoso\ServerFolders*)，选择你想要共享，然后单击该文件夹**确定**。  
  
6.  单击**下一步**。  
  
7.  指定你写下第 2 步中的权限，然后单击**添加文件夹**。  
  
    > [!IMPORTANT]
    >  这些权限将替换不使用 Windows Server Essentials 仪表板添加到该文件夹的任何现有的权限。  
  
> [!IMPORTANT]
>  完成到共享文件夹的列表中添加文件夹后，确保服务器备份中包含的文件夹。 将文件夹添加到服务器备份的信息，请参阅[设置或自定义服务器备份](Set-up-or-customize-server-backup.md)。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
