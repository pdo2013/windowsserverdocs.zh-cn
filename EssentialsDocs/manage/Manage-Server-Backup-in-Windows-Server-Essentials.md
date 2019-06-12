---
title: 管理 Windows Server Essentials 中的服务器备份
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7e40a4675cf77d55a3047b41e0ab852fd7cd9de9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433210"
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的服务器备份

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 以下主题包括有关常见备份任务的信息，你可以使用 Windows Server Essentials 仪表板来完成这些任务：  
  
-   [应选择哪个备份？](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [设置或自定义服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [停止正在进行中的服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [远程管理备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [禁用服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [了解有关设置服务器备份的详细信息](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [对服务器上的硬盘进行重新分区](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [从服务器备份中还原文件和文件夹](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a> 应选择哪个备份？  
 如果你最近成功创建了备份，并且你确定该备份包含所有重要数据，则选择服务器备份非常简单。 如果你正尝试从较旧的备份还原到服务器或计算机，则选择一个好的要还原到的备份可能需要进行某些调查，并且可能作出一些妥协。  
  
#### <a name="to-choose-a-backup"></a>选择备份  
  
1.  检查文件或文件夹的所有者，并记下添加或编辑了它们时的日期和时间。 使用这些日期和时间作为起始点。  
  
2.  在还原文件和文件夹向导中的“选择还原选项”  页面上，单击“从选定的备份还原(高级)”  。  
  
3.  选择与在步骤 1 中记下的日期和时间最匹配的备份，具体取决于你想要还原的文件或文件夹是较旧版本还是较新版本。  
  
4.  最佳做法是，你可以将文件和文件夹还原到备用位置，然后让文件和文件夹的所有者将所需文件和文件夹移动到原始位置。 完成上述操作后，即可删除保留在备用位置中的文件和文件夹。  
  
##  <a name="BKMK_1"></a> 设置或自定义服务器备份  
 在安装期间不会自动配置服务器备份。 你应通过计划每日备份来自动保护你的服务器及其数据。 建议维护每日备份计划，因为大多数组织都不能承受丢失在几天内创建的数据的后果。 有关详细信息，请参阅[设置或自定义服务器备份](Set-up-or-customize-server-backup.md)。  
  
##  <a name="BKMK_2"></a> 停止正在进行中的服务器备份  
 无论是在计划的时间定期启动服务器备份，还是手动启动服务器备份，你都可以停止正在进行的备份。  
  
#### <a name="to-stop-a-backup-in-progress"></a>停止正在进行的备份  
  
1.  打开“仪表板”。  
  
2.  在导航栏中，单击“设备”  。  
  
3.  在计算机列表中，单击服务器，然后单击“任务”  窗格中的“停止备份服务器”  。  
  
4.  单击“是”  以确认你的操作。  
  
##  <a name="BKMK_3"></a> 远程管理备份  
 当你离开办公室时，你可以使用 Windows Server Essentials 远程 Web 访问来访问 Remote Web Access 仪表板，进而管理你的服务器。  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>使用远程 Web 访问管理服务器  
  
1. 打开 Web 浏览器。  
  
2. 在地址框中，键入 Windows Server Essentials 域的名称。  
  
3. 出现提示时，输入你的用户名和密码。  
  
4. 当您单击远程 Web 访问中的服务器的名称时，将显示仪表板登录页。  
  
5. 以管理员身份登录到仪表板，然后单击“设备”  。  
  
   有关远程 Web 访问的详细信息，请参阅[远程 Web 访问概述](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)。  
  
##  <a name="BKMK_4"></a> 禁用服务器备份  
 你应通过计划每日备份来自动保护你的服务器及其数据。 建议维护每日备份计划，因为大多数组织都不能承受丢失在几天内创建的数据的后果。  
  
 如果你已配置服务器备份，并且稍后想要使用第三方应用程序备份服务器，则你可以禁用 Windows Server Essentials 服务器备份。  
  
#### <a name="to-disable-server-backup"></a>禁用服务器备份  
  
1.  使用管理员帐户和密码登录 Windows Server Essentials 仪表板。  
  
2.  单击“设备”  选项卡，然后单击服务器的名称。  
  
3.  在“任务”窗格中，单击“为服务器自定义备份”  。  
  
    > [!NOTE]
    >  使用设置服务器备份向导配置服务器备份后，将显示“为服务器自定义备份”  任务。 有关设置服务器备份的详细信息，请参阅 [Set up or customize server backup](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
4.  将显示自定义服务器备份向导。  
  
5.  在“配置选项”  页面上，单击“禁用服务器备份”  。 按照向导中的说明进行操作。  
  
##  <a name="BKMK_5"></a> 了解有关设置服务器备份的详细信息  
 在服务器设置期间不会启用服务器备份。  
  
> [!NOTE]
>  当你配置服务器备份时，你应将至少一个外部硬盘驱动器连接到服务器以用作备份目标硬盘驱动器。  
  
###  <a name="BKMK_Target"></a> 备份目标驱动器  
 你可以使用多个外部存储驱动器进行备份，也可以在现场和场外存储位置之间旋转驱动器。 如果现场硬件发生物理损坏，则通过帮助恢复你的数据，此操作可改进灾难预防计划。  
  
 为你的服务器备份选择存储驱动器时，请考虑以下方面：  
  
-   选择包含足够空间的驱动器来存储数据。 你的存储驱动器所含的容量应至少是要备份数据的存储容量的 2.5 倍。 驱动器还应足够大，以适应以后增长的服务器数据。  
  
-   如果备份目标驱动器包含脱机驱动器，备份配置将不会成功。 若要完成配置，请在选择备份目标时，清除复选框以排除处于脱机状态的驱动器。  
  
-   如果你将包含以前的备份的驱动器选择为备份目标，则该向导允许你选择是否要保留以前的备份。 如果保留这些备份，则该向导不会格式化驱动器。  
  
-   当重新使用外部存储驱动器时，请确保该驱动器为空，或仅包含不需要的数据。  
  
-   你应访问外部存储驱动器制造商的网站，以确保你的备份驱动器在运行 Windows Server Essentials 的计算机上受支持。  
  
> [!CAUTION]
>  设置服务器备份向导将在配置存储驱动器以进行备份时格式化存储驱动器。  
  
### <a name="server-backup-schedule"></a>服务器备份计划  
 你应通过计划每日备份来自动保护你的服务器及其数据。 建议维护每日备份计划，因为大多数组织都不能承受丢失在几天内创建的数据的后果。  
  
 当你使用 Windows Server Essentials 设置服务器备份向导时，你可以选择在一天内多次备份服务器数据。 因为该向导将计划基于差异的备份，因此备份将快速运行，并且不会显著影响服务器性能。 默认情况下，“设置服务器备份”  将计划一个要在每天 12:00 PM 和 11:00 PM 运行的备份。 但是，你可以根据组织需求调整备份计划。 你应该不定期评估备份计划的有效性，并根据需要更改计划。  
  
> [!NOTE]
>  默认安装 Windows Server Essentials 时，将服务器配置为每周自动执行一次碎片整理。 如果你使用非 Microsoft 映像软件，这将导致生成的备份比正常备份大。 如果无需定期对服务器进行碎片整理，你可以遵循以下步骤关闭碎片整理计划：  
> 
> 1. 按 Windows 键 + W 以打开“搜索”  。  
>    2. 在“搜索”文本框中，键入 **Defragment**。  
>    3. 在结果部分中，单击“碎片整理和优化驱动器”  。  
>    4. 在“优化驱动器”  页面上，选择一个驱动器，然后单击“更改设置”  。  
>    5. 在“优化计划”  窗口中，清除“按计划运行(推荐)”  复选框，然后单击“确定”  以保存更改。  
  
### <a name="items-to-be-backed-up"></a>要备份的项  
 默认情况下，将选中所有操作系统文件和文件夹以供备份。 你可以选择备份服务器上的所有硬盘、文件和文件夹，或仅选择单独的硬盘、文件或文件夹进行备份。 若要添加或删除要备份的项，请执行以下操作之一：  
  
-   若要在服务器备份中包含数据驱动器，请选择相邻的复选框。  
  
-   若要从服务器备份中排除数据驱动器，请清除相邻的复选框。  
  
> [!NOTE]
>  如果你想要从备份中排除“操作系统”  项，必须首先清除“系统备份(推荐)”  复选框。  
  
 若要最大程度地减少服务器备份所使用的备份硬盘空间量，你可能想要排除包含你认为没有价值或并非特别重要的文件的所有文件夹。  
  
 例如，你可能有一个包含录制的电视节目的文件夹（使用大量硬盘驱动器空间）。 你可以选择不备份这些文件，因为通常无论如何你都会在查看这些文件后删除它们。 或者，你可能有一个包含不想要保留的临时文件的文件夹。  
  
##  <a name="BKMK_6"></a> 对服务器上的硬盘进行重新分区  
 当在 Windows Server Essentials 服务器上检测到未格式化的内部硬盘驱动器时，将引发一个运行状况警报，其中包含指向“添加新硬盘驱动器向导”的链接。 “添加新硬盘驱动器向导”将向你演练用于格式化硬盘驱动器的各种选项。 完成该向导后，将在硬盘驱动器上创建一个或多个（具体取决于该驱动器大小）已格式化的逻辑硬盘驱动器，并将其格式化为 NTFS。  
  
 如果有必要对硬盘驱动器进行重新分区，请按照以下说明操作：  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>对硬盘驱动器进行重新分区  
  
1.  在“开始”  屏幕上，单击“管理工具”  ，然后双击“计算机管理”  。  
  
2.  在“计算机管理”中，单击“存储”  ，然后双击“磁盘管理”  。  
  
3.  右键单击要进行重新分区的驱动器、单击“删除卷”  ，然后单击“是”  。  
  
    > [!NOTE]
    >  针对硬盘驱动器上的每个分区重复此步骤。  
  
4.  右键单击“未分配”  硬盘驱动器，然后单击“新建简单卷”  。  
  
5.  在新建简单卷向导中，创建和格式化大小为 16 TB (16,000,000 MB) 或更少的卷。  
  
    > [!NOTE]
    >  重复此步骤，直到使用硬盘驱动器上的所有未分配空间为止。  
  
##  <a name="BKMK_7"></a> 从服务器备份中还原文件和文件夹  
 你可以从服务器备份中浏览和还原单独的文件和文件夹。  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>从服务器备份中还原文件和文件夹  
  
1.  打开“仪表板”，然后单击“设备”  选项卡。  
  
2.  单击服务器的名称，然后单击 **“任务”** 窗格中的 **“为服务器还原文件或文件夹”** 。  
  
3.  此时将打开“还原文件或文件夹向导”。 按照向导中的说明还原文件或文件夹。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
