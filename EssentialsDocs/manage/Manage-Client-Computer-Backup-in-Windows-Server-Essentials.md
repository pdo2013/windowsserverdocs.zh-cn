---
title: 管理 Windows Server Essentials 中的客户端计算机备份
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d184c97e47f04b9d7434aaeb0d328761bcfac1c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839268"
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的客户端计算机备份

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 本主题包含有关可通过使用 Windows Server Essentials 仪表板完成的客户端计算机的常见备份任务的信息，并包含以下部分：  
  
-   [修复备份数据库向导的工作原理](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [了解计算机备份设置](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [客户端计算机备份设置](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [更改备份的时间计划运行](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [更改计算机备份保留策略](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [备份重置为默认设置](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [使用修复和恢复工具](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [修复备份数据库](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [禁用计算机备份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [运行备份清理任务](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [在客户端计算机上的任务栏中查看警报](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [从快速启动板查看备份状态](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [停止正在进行从快速启动板中进行的备份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [从快速启动板启动备份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [计算机备份的工作原理](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [帮助防止数据因客户端备份数据库的损坏而丢失的提示](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [从客户端计算机备份还原文件或文件夹](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [了解文件历史记录](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a> 修复备份数据库向导的工作原理  
 如果 Windows Server Essentials 在备份数据库中检测到错误，它会向你发送一个运行状况通知，并且警报状态将更改为红色（指示紧急情况）。  
  
 在 Windows Server Essentials 仪表板上，单击警报状态图标以查看备份数据库错误通知。 该通知包括“修复”  按钮，用于启动“修复备份数据库”向导。 完成该向导可能需要几个小时。  
  
 该向导将检查选定计算机的备份数据库，并尝试修复该数据库（如果检测到错误）。 在某些情况下，该向导无法修复整个备份数据库。 在启动向导之前，验证在服务器上使用的所有外部硬盘驱动器均已连接到该服务器、处于打开状态，并可正常运行。 通过重新连接丢失的硬盘驱动器可以自动修复备份数据库。  
  
> [!CAUTION]
>  应在尝试修复数据库之前备份该数据库。 该向导可能无法恢复某些备份，具体取决于在备份数据库中找到的错误类型。 某些或所有备份可能会永久丢失。  
  
##  <a name="BKMK_2"></a> 了解计算机备份设置  
 在为客户端计算机配置了备份后，你可以为要发生的备份指定不同的时间窗口。 同样，可以指定长于或短于默认时间的备份保留时间。  
  
 使用“还原为默认值”  选项，你可以将备份窗口和保留策略重置为在初始备份配置期间提供的默认值。  
  
 默认值如下：  
  
|备份设置|默认|描述|  
|--------------------|-------------|-----------------|  
|开始时间|下午 6:00|指定开始每日备份的时间。 建议将此值设置为用户不使用其计算机时的时间。|  
|结束时间|上午 9:00|指定必须完成备份的时间。 如果备份不需要指定的持续时间，则它仅使用成功备份计算机所需的时间。|  
|保留每日备份|5 天|指定保留每日备份的天数。 例如，使用默认设置，即将每日备份保留 5 天。 在第 6 日以及之后的每一天，都将删除最旧的每日备份。|  
|保留每周备份|4 周|指定保留每周最后一个备份的周数。 例如，使用默认设置，即将每周备份保留 4 周。 在第 5 周以及之后的每一周，都将删除最旧的每周备份。|  
|保留每月备份|6 个月|指定保留每月最后一个备份的月数。 例如，使用默认设置，即将每月备份保留 6 个月。 在第 7 个月以及之后的每一个月，都将删除最旧的每月备份。|  
  
##  <a name="BKMK_3"></a> 客户端计算机备份设置  
 如果备份处于禁用状态，则可以从仪表板中设置计算机的备份。 当你为计算机设置备份时，你可以选择备份计算机上的所有内容，或选择想要备份的卷和文件夹。  
  
> [!NOTE]
>  计算机必须处于联机状态，你才能设置备份。  
  
> [!IMPORTANT]
>   Windows Server Essentials 不支持备份和还原客户端计算机上的动态磁盘。  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>设置客户端计算机备份  
  
1.  打开“仪表板”，然后单击“设备”选项卡。  
  
2.  单击要为其设置备份的客户端计算机的名称，然后在“任务”窗格中，单击“为此计算机设置备份”。  
  
    > [!NOTE]
    >  如果已为客户端计算机设置备份，则“任务”窗格中将列出“为此计算机自定义备份”，而不是“为此计算机设置备份”。  
  
3.  在“设置备份”向导中，你可以选择备份所有文件夹，或选择想要备份的某些文件夹。 按照向导中的说明进行操作。  
  
4.  在为计算机设置备份后，单击“关闭”  。  
  
### <a name="critical-system-files"></a>关键系统文件  
 安装 Windows 操作系统时，安装程序将在系统驱动器上创建文件夹，它将在这些文件夹中放置系统需要启动并运行的文件。 关键系统文件包括具有 .dll、.exe、.ocx 和 .sys 文件扩展名的文件。 其中某些文件采用 True Type 字体。 此外，系统状态文件，如系统的注册表所需操作系统才能正常运行。  
  
### <a name="find-the-file-you-are-looking-for"></a>查找所需文件  
 你可以从现有备份中还原计算机的所有文件夹、多个文件和文件夹或者一个文件或文件夹。  
  
 在选择你想要用于从中还原的备份之后，将读取备份文件并显示所有文件和文件夹。 通过双击顶层文件夹，然后向下钻取文件夹的整个层次结构直到找到所需文件，可向下钻取要还原的特定文件或文件夹。  
  
### <a name="why-am-i-unable-to-select-some-items"></a>为什么无法选择某些项？  
 “选择要备份的项”页面的选择菜单上的复选框可以为每个文件夹指示不同状态。 当复选框处于以下状态时：  
  
-   **选中**，选择关联的文件夹和文件夹内容以供备份。  
  
-   **取消选中**，从备份中排除关联的文件夹和文件夹内容。  
  
-   **固定**，选择关联文件夹以供备份，但从备份中排除该文件夹内的一个或多个项。  
  
 如果无法选择特定文件夹，则执行以下操作：  
  
-   可以配置卷以供备份，但该卷可能处于脱机状态。 对于可移动 USB 驱动器，这是常见做法。 处于脱机状态的卷将以灰色文本显示。  
  
-   你仅可以从格式化为 NTFS 文件系统的本地驱动器中备份数据。 格式化为 FAT（包括 FAT32）或 ReFS 文件系统的驱动器不显示在要备份的驱动器列表中。  
  
> [!IMPORTANT]
>  卷影复制服务 (VSS) 不支持在同一快照集中创建虚拟卷和主机卷的卷影副本。 如果有必要备份虚拟卷，则 VSS 支持在虚拟硬盘 (VHD) 上创建卷的快照。 有关详细信息，请参阅 [Servicing and Backing Up Virtual Hard Disks](https://go.microsoft.com/fwlink/p/?LinkId=256577)（维护和备份虚拟硬盘）。  
  
##  <a name="BKMK_4"></a> 更改备份的时间计划运行  
 应在尽可能少的用户使用联网计算机时计划备份过程。 此时间通常介于傍晚或早晨之间。 备份的默认时间为下午 6:00 到上午 9:00。 服务器仅在计划的时间窗口期间尝试备份客户端计算机。  
  
 但是，如果你的企业在这些传统的休息时间期间仍处于工作状态，则你可能想要更改这些默认设置。  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>更改计划运行备份的时间  
  
1.  打开服务器“仪表板”，然后单击“设备”。  
  
2.  在“设备任务” 中，单击“自定义计算机备份和文件历史记录设置” 。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，此任务已被重命名**客户端计算机备份任务**。  
  
3.  在“客户端计算机备份设置和工具” 中的“计算机备份”  选项卡上，你可以更改开始时间和结束时间以满足你的需求。  
  
4.  单击“应用”，然后单击“确定”。  
  
##  <a name="BKMK_5"></a> 更改计算机备份保留策略  
 随着时间的推移，所有计算机的每日备份都会在服务器上累积增加。 若要帮助你管理这些备份，Windows Server Essentials 有助于管理计算机备份的数据库。 你可以配置所有计算机的备份保留数量。  
  
 备份保留策略可确定在备份清理过程中删除备份之前备份的保留时间。 备份清理在每个星期六的下午 11:59 运行。 它将删除超出备份保留策略的所有备份。 备份保留策略的默认值如下：  
  
-   **将每日备份保留 5 天。** 将每日的第一个备份保留为每日备份。 5 天后，将在清理过程中删除最旧的每日备份。  
  
-   **将每周备份保留 4 周。** 将每周的第一个备份保留为每周备份。 4 周后，将在清理过程中删除最旧的每周备份。  
  
-   **将每月备份保留 6 个月。** 将每月的第一个备份保留为每月备份。 6 个月后，将在清理过程中删除最旧的每月备份。  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>更改计算机备份保留策略  
  
1.  打开 **“仪表板”**。  
  
2.  单击“设备”。  
  
3.  在“设备任务”窗格中，单击“自定义计算机备份和文件历史记录设置”。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，此任务已被重命名**客户端计算机备份任务**。  
  
4.  在“客户端计算机备份设置和工具” 的“客户端计算机备份保留策略”  部分中，对满足你的需求的保留策略进行更改。  
  
5.  完成上述操作后，单击“确定”以应用这些更改并关闭对话框。 在下次运行备份清理时，将应用更新的保留策略。  
  
    > [!NOTE]
    >  更新的保留策略适用于网络上为进行备份而配置的所有客户端计算机。  
  
##  <a name="BKMK_6"></a> 备份重置为默认设置  
 为客户端计算机配置了备份后，网络管理员可以指定不同的时间窗口。 同样，管理员可以指定长于或短于默认时间的备份保留时间。 使用“重置为默认值”  按钮，你可以将备份窗口和保留策略重置为在初始备份配置期间提供的默认值。  
  
 默认值如下：  
  
-   备份开始时间：下午 6:00  
  
-   结束时间：上午 9:00  
  
-   保留每日备份的天数：5 天  
  
-   保留每周最后一个备份的周数：4 周  
  
-   保留每月最后一个备份的月数：6 个月  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>将计算机备份重置为默认设置  
  
1.  打开“仪表板” ，然后打开“设备”  页面。  
  
2.  在“设备任务” 中，单击“自定义计算机备份和文件历史记录设置” 。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，单击**客户端计算机备份任务**。  
  
3.  在“客户端计算机备份设置和工具”  页面上的“计算机备份”  选项卡上，单击“重置为默认值” 。  
  
    > [!NOTE]
    >  你可能想要记下自定义的计划和保留策略设置，因为它们将在重置为默认设置时消失。  
  
4.  单击“应用”，然后单击“确定”。  
  
##  <a name="BKMK_7"></a> 使用修复和恢复工具  
 **修复备份：** 如果计算机备份的数据库由于某些原因而损坏或无法使用，你可以尝试使用“修复备份数据库”向导修复该数据库。 该向导将分析备份文件，以确定是否有可以修复的任何问题。 然后，该向导将尝试修复找到的所有问题。  
  
> [!WARNING]
>  如果无法修复问题，该向导可能会永久删除计算机备份文件。  
  
 **计算机恢复：** 你可以创建可启动 U 盘，以用于从现有备份中恢复计算机。 必须使用大小为 1 GB 或更大的 U 盘。 创建可启动 U 盘后，将其插入到要还原的计算机中，然后启动到 U 盘以启动完整系统还原过程。  
  
##  <a name="BKMK_8"></a> 修复备份数据库  
 如果收到一条告知你计算机备份数据库有问题的警报，则可以尝试修复它。  
  
#### <a name="to-repair-the-backup-database"></a>修复备份数据库  
  
1.  打开 **“仪表板”**。  
  
2.  单击“设备”。  
  
3.  在“设备任务”窗格中，单击“自定义计算机备份和文件历史记录设置”。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，单击**客户端计算机备份任务**。  
  
4.  在“客户端计算机备份设置和工具”中，请单击“工具”选项卡。  
  
5.  在“修复备份”  部分中，单击“立即修复” 。 这将打开“修复备份数据库向导”。  
  
6.  按照“修复备份数据库向导”中的说明操作。  
  
7.  数据库修复可能需要几个小时，具体取决于备份数据库的大小。 单击“关闭”，然后单击“确定”以关闭“自定义计算机备份和文件历史记录设置”页面。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，单击**客户端计算机备份设置和工具**。  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>查看备份数据库修复结果  
  
1.  打开 **“仪表板”**。  
  
2.  单击“设备”。  
  
3.  在“设备任务”窗格中，单击“自定义计算机备份和文件历史记录设置”。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，单击**客户端计算机备份任务**。  
  
4.  在“客户端计算机备份设置和工具”中，请单击“工具”选项卡。  
  
5.  结果将显示在“修复备份”部分中。  
  
##  <a name="BKMK_9"></a> 禁用计算机备份  
 使用仪表板快速禁用网络上计算机的备份。  
  
#### <a name="to-disable-backup-for-a-computer"></a>禁用计算机备份  
  
1.  打开 **“仪表板”**。  
  
2.  单击 **“设备”** 选项卡。  
  
3.  单击要禁用其备份的计算机的名称。  
  
4.  在“任务”窗格中，单击“为计算机自定义备份” 。 这将显示“自定义备份”向导。  
  
5.  单击“禁用此计算机的备份”，然后选择你想要保留还是删除现有备份文件。  
  
6.  单击“保存更改”，然后单击“关闭”。  
  
 有关如何在禁用备份后启用计算机备份的信息，请参阅[为客户端计算机设置备份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)。  
  
##  <a name="BKMK_10"></a> 运行备份清理任务  
 完成所有备份后，计划在每个星期六的下午 11:59 运行客户端计算机备份清理任务。 根据备份保留策略，清理任务将从客户端计算机备份数据库中删除备份。 备份保留策略的默认设置如下：  
  
-   保留每日备份的天数：5 天  
  
-   保留每周最后一个备份的周数：4 周  
  
-   保留每月最后一个备份的月数：6 个月  
  
 有关更改备份保留策略的信息，请参阅[更改计算机备份保留策略](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>运行客户端备份数据库清理任务  
  
1.  使用管理员用户帐户和密码登录服务器。  
  
2.  在服务器上，打开“任务计划程序” 。 你可以从“管理工具”控制台中访问任务计划程序。  
  
3.  在任务计划程序中，依次展开“任务计划程序库” 、“Microsoft” 、“Windows” ，然后单击“Windows Server Essentials” 。  
  
4.  单击“备份清理”任务，然后在“操作”窗格中单击“运行”。 将状态更改为“正在运行”  ，直到完成该任务为止。  
  
##  <a name="BKMK_11"></a> 在客户端计算机上的任务栏中查看警报  
 出于以下原因，你可以在计算机的任务栏中收到备份通知：  
  
-   备份已开始。  
  
-   备份已结束。  
  
-   没有最新的成功备份。 将最后一个成功备份包含在通知后的天数。  
  
-    仅限 Windows Server Essentials:向计算机添加一个新卷。 管理你的网络的用户需要运行自定义备份向导，以包括或排除用于以后备份的卷。  
  
##  <a name="BKMK_12"></a> 从快速启动板查看备份状态  
 使用快速启动板查看计算机的快速备份状态。  
  
> [!TIP]
>  有关快速状态，将光标移动到桌面通知区域中的快速启动板图标上。 备份状态将显示在工具提示中。  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>查看计算机的备份状态  
  
1.  使用用户名和密码登录到“快速启动板”。  
  
2.  单击“备份” 。  
  
3.  在“备份属性”  对话框的“备份状态”  部分中，将显示备份的状态。  
  
    -   **以前没有备份**：尚未运行任何备份，或已删除备份历史记录。  
  
    -   **备份挂起**：备份正在等待服务器上的另一个过程完成。  
  
    -   **连接**：备份正在连接到服务器。  
  
    -   **无法连接**：备份无法连接到 Windows Server 客户端计算机备份提供程序服务。  
  
        > [!NOTE]
        >  在 Windows Server Essentials 中，此服务已重命名为 Windows Server Essentials 客户端计算机管理服务。  
  
    -   **正在备份**：显示已完成备份的百分比。  
  
    -   **备份已取消**：如果你或网络管理员已取消备份，则显示开始备份的日期和时间。  
  
    -   **备份已成功**：如果已成功完成备份，则显示开始备份的日期和时间。  
  
    -   **备份不完整**：如果备份未成功完成，则显示开始备份的日期和时间。  
  
    -   **备份未成功**：如果备份未成功完成，则显示开始备份的日期和时间。  
  
4.  单击“确定”以关闭“备份属性”对话框。  
  
##  <a name="BKMK_13"></a> 停止正在进行从快速启动板中进行的备份  
 你可以轻松停止正在进行的备份。  
  
#### <a name="to-stop-a-backup-in-progress"></a>停止正在进行的备份  
  
1.  打开“快速启动板” 。  
  
    > [!NOTE]
    >  如果需要，使用用户名和密码登录到“快速启动板”。  
  
2.  单击“备份” 。  
  
3.  当备份正在进行中时，在“备份属性”  对话框的“备份状态”  部分中，将“开始备份”  按钮更改为“停止备份”  。 单击“停止备份” ，然后单击“是”  以进行确认。 备份将继续运行，直到你单击“是”为止。  
  
4.  当你停止正在进行的备份时，状态将更改为包含开始备份的日期和时间的“备份已取消”  。 相反，如果看到**成功**、**不完整**或**失败**状态，则已完成最后一个备份。  
  
5.  单击“确定”以关闭“备份属性”对话框。  
  
##  <a name="BKMK_14"></a> 从快速启动板启动备份  
 有时候，你希望在服务器上设置定期计划的备份时间之前备份你的文件和文件夹。 快速启动板使你能够手动启动计算机备份。  
  
#### <a name="to-start-a-backup"></a>启动备份  
  
1.  打开“快速启动板” 。  
  
    > [!NOTE]
    >  如果需要，使用用户名和密码登录到“快速启动板”。  
  
2.  单击“备份” 。  
  
3.  在“备份属性”对话框的“备份状态”部分中，单击“开始备份”，然后单击“确定”。  
  
4.  键入备份的说明，然后单击“确定” 。 状态将更改为“开始备份”，然后再更改为包含完成百分比的“正在备份”。  
  
5.  开始备份后，该按钮将更改为“停止备份”。 如果备份正在进行中并且你想要停止它，则单击“停止备份” ，然后单击“是” 。 当你停止正在进行的备份时，状态将更改为包含已开始备份的日期和时间的“备份已取消”。  
  
6.  成功完成备份后，状态将更改为包含完成备份的日期和时间的“备份已成功”  。  
  
7.  单击“确定”以关闭“备份属性”对话框。  
  
##  <a name="BKMK_15"></a> 计算机备份的工作原理  
 在每天进行备份期间，都会发生以下情况：  
  
-   逐一备份网络计算机。  
  
-   完成正在进行的备份，即使你关闭备份窗口也是如此。  
  
-   安装 Windows 更新，并重新启动 Windows Server Essentials（如果需要）。  
  
-   在星期日，运行备份清理。  
  
> [!IMPORTANT]
>  卷影复制服务 (VSS) 不支持在同一快照集中创建虚拟卷和主机卷的卷影副本。 如果有必要备份虚拟卷，则 VSS 支持在虚拟硬盘 (VHD) 上创建卷的快照。 有关详细信息，请参阅 [Servicing and Backing Up Virtual Hard Disks](https://go.microsoft.com/fwlink/p/?LinkId=256577)（维护和备份虚拟硬盘）。  
  
##  <a name="BKMK_16"></a> 帮助防止数据因客户端备份数据库的损坏而丢失的提示  
 如果客户端备份数据库损坏，你可能会丢失重要数据。  
  
 若要防止数据因客户端备份数据库的损坏而丢失，请考虑以下方面：  
  
-   启用备份客户端备份数据库的其他方法。 例如，具体取决于正在运行的 Windows Server Essentials 的版本，使用服务器备份、 在线备份或 Microsoft Azure 备份来备份数据库。  
  
    > [!IMPORTANT]
    >  确保选择备份“客户端计算机备份”文件夹中的所有文件。  
  
-   如果必须还原数据库，请确保还原“客户端计算机备份”  文件夹中的所有文件。 如果没有还原所有文件，则数据库可能会变得不一致。  
  
-   在清除或还原客户端备份数据库之前，请确保停止当前正在进行的所有客户端备份。  
  
     如果你正在运行 Windows Server Essentials，你还应停止 Windows Server 客户端计算机备份服务和 Windows Server Client Computer Backup Provider Service。  
  
     如果你正在运行 Windows Server Essentials，你还应停止 Windows Server 计算机备份服务。  
  
     完成还原操作后，请重新启动该服务。  
  
##  <a name="BKMK_17"></a> 从客户端计算机备份还原文件或文件夹  
 你可以从备份中浏览和还原单独的文件和文件夹。  
  
> [!NOTE]
>  无法使用服务器上的仪表板将文件和文件夹还原到客户端计算机。 必须打开客户端计算机上的仪表板，才能完成该任务。  
  
> [!IMPORTANT]
>  无法将文件和文件夹还原到已配置为动态磁盘的硬盘。  
  
#### <a name="to-restore-files-and-folders"></a>还原文件和文件夹  
  
1.  打开客户端计算机上的“仪表板”  。  
  
2.  单击 **“设备”** 选项卡。  
  
3.  单击你想要为其还原文件和文件夹的计算机，然后单击“任务”窗格中的“为计算机还原文件或文件夹”。 此时将打开“还原文件或文件夹向导”。  
  
4.  按照向导中的说明进行操作。  
  
##  <a name="BKMK_FileHistory"></a> 了解文件历史记录  
 Windows Server Essentials 的文件历史记录功能将自动备份位于具有文件历史记录功能的网络计算机的库、联系人、桌面和收藏夹文件夹中的文件。 如果丢失、损坏或删除原始文件，则可以还原它们。 还可以从某个特定时间点查找不同版本的文件。 随着时间的推移，你将具有一个完整的文件历史记录。  
  
 在 Windows Server Essentials 中，您可以自定义中的文件历史记录设置**文件历史记录**页**客户端计算机备份设置和工具**。  
  
 在 Windows Server Essentials 中，您可以自定义文件历史记录设置通过转到**用户**选项卡上，，然后选择**更改文件历史记录设置**任务。  
  
 文件历史记录页面提供了以下选项：  
  
|备份设置|默认|描述|  
|--------------------|-------------|-----------------|  
|打开/关闭|开|默认情况下，当安装 Windows Server Essentials 时，文件历史记录处于打开状态。|  
|备份数据|文档和桌面|有三个预配置的设置，允许你指定各种备份解决方案。 你可以选择以下选项之一：<br /><br /> -文档和桌面<br /><br /> -所有库、 桌面、 联系人和收藏夹<br /><br /> 的库、 桌面、 联系人和收藏夹不包括音乐、 视频和图片库中的数据中所有数据|  
|备份频率|每小时|指定文件历史记录创建选定数据备份的频率。 你可以从若干选项中进行选择，频率范围介于每 10 分钟到每天。|  
|副本保留时间|1 年|指定文件历史记录保留备份副本的时间量。|  
  
 有关文件历史记录问题的信息，请参阅[文件历史记录疑难解答](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md)。in  
  
## <a name="see-also"></a>请参阅  
  
-   [管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
