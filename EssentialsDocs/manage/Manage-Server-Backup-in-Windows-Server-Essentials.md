---
title: "管理 Windows Server Essentials 中的服务器备份"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 2b0cd926b15d65e5cd4c784681c40df29b18a48f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的服务器备份

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 下面的主题包括有关你可以通过使用 Windows Server Essentials 仪表板达到的常见备份任务信息：  
  
-   [应选择哪个备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [设置或自定义服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [停止服务器正在进行的备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [远程管理你的备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [禁用服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [了解有关服务器备份设置的详细信息](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [重新服务器上的硬盘分区](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [从 server 备份中还原的文件和文件夹](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>应选择哪个备份  
 如果你拥有最新的成功备份，并且你知道的备份包含所有重要数据，则选择备份可以是非常简单。 如果想要从旧的备份中还原到服务器或电脑，选择良好的备份还原到可能需要一些调查和，可能是，某些损害。  
  
#### <a name="to-choose-a-backup"></a>选择备份  
  
1.  请与所有者的文件或文件夹，并记下的日期和时间他们添加或编辑他们。 作为第一步中使用这些的日期和时间。  
  
2.  在**选择恢复选项**还原的文件和文件夹向导中的页面上，单击**从 （高级） 我选择的备份中还原**。  
  
3.  是否想要还原较早版本或更新版本的文件或文件夹，根据选择最适合的日期和时间，述第 1 步备份。  
  
4.  作为最佳做法，你可以还原到另一个位置的文件和文件夹，然后让所有者的文件和文件夹移到原始位置的所他们的需求。 当他们完成后时，可以删除的文件和文件夹保留在其他位置。  
  
##  <a name="BKMK_1"></a>设置或自定义服务器备份  
 在安装过程中未自动配置服务器的备份。 你应该保护服务器和及其数据自动安排每天备份。 建议你维护每天备份套餐，因为组织大多数不可丢失已创建几天的数据。 有关详细信息，请参阅[设置或自定义服务器备份](Set-up-or-customize-server-backup.md)。  
  
##  <a name="BKMK_2"></a>停止服务器正在进行的备份  
 是否服务器备份定期次启动或手动启动备份服务器时，你可以停止正在备份。  
  
#### <a name="to-stop-a-backup-in-progress"></a>若要停止在进行备份  
  
1.  打开的面板。  
  
2.  在导航栏中，单击**设备**。  
  
3.  在计算机的列表中，单击服务器，然后单击**服务器停止备份**中**任务**窗格。  
  
4.  单击**是**以确认你的操作。  
  
##  <a name="BKMK_3"></a>远程管理你的备份  
 当你不在办公室，可用于 Windows Server Essentials 远程 Web 访问访问 Windows Server Essentials 仪表板管理您的服务器。  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>使用远程网站的访问权限来管理服务器  
  
1.  打开 web 浏览器。  
  
2.  在地址框中，键入 Windows Server Essentials 域名。  
  
3.  看到提示后，输入你的用户名和密码。  
  
4.  当你单击中远程访问 Web 服务器的名称时，仪表板登录页面的显示方式。  
  
5.  登录到以 administrator 身份，仪表板上，然后单击**设备**。  
  
 有关远程网站访问的详细信息，请参阅[远程访问 Web 概述](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)。  
  
##  <a name="BKMK_4"></a>禁用服务器备份  
 你应该保护服务器和及其数据自动安排每天备份。 建议你维护每天备份套餐，因为组织大多数不可丢失已创建几天的数据。  
  
 如果你已经拥有服务器备份配置，并且稍后想要使用第三方应用程序备份服务器，您可以禁用 Windows Server Essentials 服务器备份。  
  
#### <a name="to-disable-server-backup"></a>若要禁用服务器备份  
  
1.  登录到 Windows Server Essentials 仪表板使用管理员帐户和密码。  
  
2.  单击**设备**选项卡，然后单击服务器的名称。  
  
3.  在任务窗格中，单击**服务器自定义备份**。  
  
    > [!NOTE]
    >  **服务器自定义备份**配置服务器备份使用设置服务器备份向导后，将显示任务。 有关服务器备份设置的详细信息，请参阅[设置或自定义服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
4.  显示自定义服务器备份向导。  
  
5.  在**配置选项**页上，单击**禁用服务器备份**。 按照该向导中的说明进行操作。  
  
##  <a name="BKMK_5"></a>了解有关服务器备份设置的详细信息  
 服务器安装过程中未启用服务器备份。  
  
> [!NOTE]
>  配置服务器备份时，你应连接到服务器要用作备份目标硬盘驱动器的至少一个外部硬盘。  
  
###  <a name="BKMK_Target"></a>备份目标驱动器  
 对于备份，可以使用多个存储外部驱动器，可以旋转之间现场和现场存储位置驱动器。 这可以提高你灾难准备工作，帮助你恢复你的数据，如果出现硬件现场物理损坏的计划。  
  
 当选择服务器备份存储驱动器，请考虑以下各项：  
  
-   选择一个驱动器包含足够空间来存储您的数据。 存储驱动器应包含至少 2.5 倍你想要备份的数据存储容量。 此外应大到足以容纳服务器数据未来发展驱动器。  
  
-   如果备份目标驱动器包含离线状态下的驱动器，将会失败备份配置。 若要选择备份目标时，请完成配置，，清除该复选框来排除处于离线状态的驱动器。  
  
-   如果你选择包含之前的备份作为备份目的地的驱动器，该向导将允许你选择你是否想要保留之前的备份。 如果你保存的备份，该向导不会格式化驱动器。  
  
-   时遭存储外部驱动器，请确保该驱动器为空或包含你不需要的数据。  
  
-   你应该访问确保备份你的驱动器上运行 Windows Server Essentials 计算机的支持您存储的外部驱动器制造商的网站。  
  
> [!CAUTION]
>  设置服务器备份向导格式存储驱动器时它将用于备份配置它们。  
  
### <a name="server-backup-schedule"></a>服务器备份计划  
 你应该保护服务器和及其数据自动安排每天备份。 建议你维护每天备份套餐，因为组织大多数不可丢失已创建几天的数据。  
  
 当你使用 Windows Server Essentials 设置服务器备份向导时，你可以选择在一天中的多个时间备份 server 的数据。 该向导计划基于差异的备份，因为备份快速，运行，而不显著受影响服务器的性能。 默认情况下，**设置向上服务器备份**计划备份中午 12:00 和晚上 11:00 每天运行。 但是，你可以调整备份计划根据你的组织的需求。 有时，你应评估你的备份计划的效果，并计划根据需要更改。  
  
> [!NOTE]
>  在 Windows Server Essentials 默认安装中，服务器配置为自动执行碎片整理每周一次。 这可能导致大于正常备份。 如果你使用非 Microsoft 映像软件。 如果不需要进行碎片整理定期服务器，你可以按照以下步骤来关闭在碎片碎片整理计划：  
>   
>  1.  按 Windows 键 + W 以打开**搜索**。  
> 2.  在搜索文本框中，键入**碎片整理**。  
> 3.  在结果部分中，单击**碎片整理和优化驱动器**。  
> 4.  在**优化驱动器**页上，选择一个驱动器，然后单击**更改设置**。  
> 5.  在**优化计划**窗口中，清除**（推荐） 计划运行**复选框，然后依次单击**确定**保存更改。  
  
### <a name="items-to-be-backed-up"></a>若要进行备份的项目  
 默认情况下的备份选择所有操作系统的文件和文件夹。 你可以选择在服务器上，备份所有硬盘、 文件和文件夹，或者选择个别硬盘、 文件或备份到的文件夹。 若要添加或删除威胁项的备份，执行以下任一操作：  
  
-   若要服务器备份中包含的数据驱动器，请选择旁边的复选框。  
  
-   要排除从服务器备份数据驱动器，请清除旁边的复选框。  
  
> [!NOTE]
>  如果你想要排除**操作系统**项从备份中，你必须先清除**系统备份 （推荐）**复选框。  
  
 以最小化服务器备份使用的备份硬盘空间量，你可能想要排除任何文件夹，其中包含你不考虑宝贵或尤为重要的文件。  
  
 例如，你可能必须包含录制的电视节目的使用大量硬盘驱动器空间的文件夹。 你可以选择不备份这些文件，因为你通常删除它们后仍查看它们。 或者，你可能已包含你不想要保留的临时文件的文件夹。  
  
##  <a name="BKMK_6"></a>重新服务器上的硬盘分区  
 Windows Server Essentials 服务器上检测到时未格式化的内部硬盘驱动器，健康警报包含添加新的难度驱动器向导的链接。 添加新的难度驱动器向导将指导你完成格式化硬盘了各种选项。 完成向导后，将在硬盘上创建一个或多个，具体取决于驱动器的大小逻辑硬盘驱动器格式化，并 NTFS 格式。  
  
 如果需要重新硬盘分区，请按照以下说明进行操作：  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>若要重新硬盘分区  
  
1.  在**开始**屏幕上，单击**管理工具**，然后双击**计算机管理**。  
  
2.  在计算机管理单击**存储**，然后双击**磁盘管理**。  
  
3.  右键单击你想要重新分区、 单击该驱动器**删除音量**，然后单击**是**。  
  
    > [!NOTE]
    >  硬盘上每一个分区的重复此步骤。  
  
4.  右键单击**未指派**硬盘驱动器上，然后单击**新建简单卷**。  
  
5.  在新建简单卷向导中创建和格式化的卷，为 16 TB (16,000,000 MB) 或更少。  
  
    > [!NOTE]
    >  重复此步骤，直到在硬盘上未分配的所有空间都使用。  
  
##  <a name="BKMK_7"></a>从 server 备份中还原的文件和文件夹  
 你可以浏览，并从 server 备份中还原个别文件和文件夹。  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>若要从 server 备份中还原的文件和文件夹  
  
1.  打开仪表板，然后单击**设备**选项卡。  
  
2.  单击服务器的名称，然后单击**还原文件或文件夹的服务器**中**任务**窗格。  
  
3.  还原文件或文件夹向导将打开。 按照该向导中的说明来还原文件或文件夹。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
