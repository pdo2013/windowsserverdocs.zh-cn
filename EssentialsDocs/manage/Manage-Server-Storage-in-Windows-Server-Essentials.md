---
title: 管理 Windows Server Essentials 中的服务器存储
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1637dc6844a0152249fc406a66ef583b9e3a5242
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914654"
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的服务器存储

>适用于：Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
   
 通过 Windows Server Essentials，你可以从仪表板的“存储” 选项卡上的“硬盘驱动器” 页面管理所有服务器存储（包括硬盘驱动器和存储空间）。  
  
 以下部分提供的信息将帮助你增加服务器存储、了解和使用存储空间以及管理硬盘驱动器：  
  
-   [使用仪表板管理硬盘驱动器](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [增加服务器上的存储](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [在硬盘驱动器上执行检查和修复](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [格式化硬盘驱动器](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [添加新的硬盘驱动器](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [存储空间概述](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [创建存储空间](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>使用仪表板管理硬盘驱动器  
 Windows Server Essentials 允许你通过仪表板管理所有连接到服务器的硬盘驱动器。 在仪表板“存储”选项卡上，**硬盘驱动器**将显示服务器上用于存储数据和服务器备份的所有可用硬盘驱动器。 服务器将监视每个硬盘驱动器上的可用空间，如果硬盘空间不足则显示警报。 “硬盘驱动器”选项卡显示以下信息：  
  
- 每个硬盘驱动器的名称  
  
- 每个硬盘驱动器的容量  
  
- 每个硬盘驱动器的已用空间大小  
  
- 每个硬盘驱动器的可用空间大小。  
  
- 每个硬盘驱动器的状态；空白状态表示该硬盘驱动器运行正常  
  
- “详细信息”窗格，在选定硬盘驱动器位于存储空间（而非物理磁盘）时用于显示所有存储堆栈信息（对于存储池、存储空间和硬盘驱动器）  
  
  下表列出仪表板中可用的硬盘驱动器管理任务及其说明。 某些任务仅在选中某个硬盘驱动器时显示。  
  
### <a name="available-hard-drive-management-tasks"></a>可用的硬盘驱动器管理任务  
  
|任务名称|描述|  
|---------------|-----------------|  
|**查看硬盘驱动器属性**|打开 _**属性**页。 选中该硬盘驱动器时显示此任务。 **硬盘名称** “属性”页的“常规” 选项卡包含以下额外任务：<br /><br /> -   **驱动器清理**:允许你清理硬盘驱动器上未使用的文件 (此任务仅在 Windows Server Essentials 中提供)。<br />-   **检查并修复**:检查硬盘驱动器是否存在文件系统错误，并尝试自动修复检测到的错误。<br /><br /> " _**属性**" 页的 "**卷影副本**" 选项卡允许你启用卷影副本。 该选项卡还显示卷影副本计划下一次运行的时间。|  
|**管理存储空间**|**注意：** 对于 Windows Server Essentials, 仅当存在现有存储空间时才显示此任务。<br /><br /> 打开“存储空间” 控制面板，你可以从此处创建和管理存储池以及存储空间。|  
|**创建存储空间**|打开“创建存储空间”向导，该向导允许你使用一个或多个硬盘驱动器来增加存储池的容量。|  
|**增加存储池容量**|**注意：** 此任务仅在选定硬盘驱动器位于存储空间时才可见。<br /><br /> 打开“增加存储池容量”向导，该向导允许你使用一个或多个硬盘驱动器来增加存储池的容量。|  
  
##  <a name="BKMK_2"></a>增加服务器上的存储  
 若要增加服务器上的容量，你可以向服务器添加额外的内部硬盘驱动器。 若要添加额外的内部硬盘驱动器，必须先关闭服务器，再添加内部硬盘驱动器，然后重新启动服务器。 如果硬盘连接到 SCSI 控制器，则无需关闭服务器。 在此情况下，可以在服务器运行时插入硬盘。  
  
 根据要添加的硬盘驱动器是否已格式化，执行以下操作之一：  
  
-   **格式化** 如果内部硬盘已使用 NTFS 或 ReFS 格式化，服务器将为其分配驱动器号，该硬盘将显示在“硬盘驱动器” 选项卡上。现在可以在新的硬盘驱动器上创建服务器文件夹，或将服务器文件夹移动到其中。  
  
-   **未设置格式**如果内部硬盘未格式化, 将显示以下警报:一个或多个未格式化的硬盘驱动器连接到服务器。 使用以下步骤格式化硬盘。  
  
#### <a name="to-format-the-hard-disk"></a>格式化硬盘  
  
1.  打开“仪表板”。  
  
2.  在导航窗格中, 单击警报图标以启动 Windows Server Essentials 中的**警报查看器**或 Windows server essentials 中的 "**运行状况监视**" 选项卡。  
  
3.  在“警报查看器”或“运行状况监视”选项卡中，单击该警报，然后在“任务”窗格中，单击“解决此问题”。  
  
4.  按照说明完成“添加新硬盘驱动器”向导。  
  
###  <a name="BKMK_Clean"></a>清理硬盘驱动器  
  
1.  打开“仪表板”。  
  
2.  在导航窗格中，单击“存储”，然后单击“硬盘驱动器”。  
  
3.  在“硬盘驱动器”部分中，选择为新添加的硬盘分配的驱动器号，然后在“任务”窗格中，单击“查看硬盘驱动器属性”。  
  
4.  在 **< 驱动器号\> "属性**" 的 "**常规**" 选项卡上, 单击 "**驱动器清除**"。  
  
##  <a name="BKMK_Check"></a>在硬盘驱动器上执行检查和修复  
 硬盘驱动器检查和修复过程将验证存储在硬盘驱动器上的文件系统的运行状况。 它在存储备份文件的卷上运行 **chkdsk** 过程。 可以在硬盘驱动器上运行检查和修复解决以下警报问题：  
  
-   必须检查服务器备份中的一个或多个硬盘驱动器。  
  
#### <a name="to-check-and-repair-hard-drives"></a>检查和修复硬盘驱动器  
  
1.  打开“仪表板”。  
  
2.  单击“服务器文件夹和硬盘驱动器”，然后单击“硬盘驱动器”。  
  
3.  选择显示错误的硬盘驱动器，然后选择“查看硬盘驱动器属性”。  
  
4.  在“检查和修复”选项卡上， 单击“检查和修复”。  
  
##  <a name="BKMK_3"></a>格式化硬盘驱动器  
 在服务器上检测到未格式化的内部硬盘驱动器时，运行状况警报将引导用户完成格式化过程。 “添加新硬盘驱动器”向导将引导你完成格式化硬盘驱动器，并支持你采用以下方式之一配置硬盘驱动器：  
  
1.  格式化硬盘驱动器，并自动在该处创建驱动器。 如果选择此选项，当完成该向导时，将创建一个使用 NTFS 文件格式化的逻辑硬盘驱动器。  
  
2.  格式化硬盘驱动器，并将其设置为用于服务器备份。 如果选择此选项，将启动“设置服务器备份”向导，该向导将引导你完成服务器备份配置。  
  
3.  如果存储空间不存在, 请使用新的硬盘驱动器创建存储空间。 必须至少有两个硬盘驱动器才能创建存储空间。  
  
4.  如果已存在存储空间，请使用新的硬盘驱动器增加存储池的容量。 此选项仅当在服务器上创建的现有存储空间存在时才显示。 如果选择此选项，向导会将此硬盘驱动器添加到存储池中。  
  
##  <a name="BKMK_4"></a>添加新的硬盘驱动器  
 当你将新的硬盘驱动器插入运行 Windows Server Essentials 的服务器时，你可以执行以下操作：  
  
-   [使用新的硬盘驱动器存储服务器文件夹](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [使用新的硬盘驱动器存储服务器备份](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [使用新的硬盘驱动器来增加存储池容量](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>使用新的硬盘驱动器存储服务器文件夹  
 若要使用新的硬盘驱动器存储服务器文件夹，可以将新的服务器文件夹添加到硬盘驱动器，或将现有服务器文件夹移动到硬盘驱动器。  
  
##### <a name="to-store-server-folders"></a>存储服务器文件夹  
  
1. 打开“仪表板”。  
  
2. 单击“存储”选项卡，然后单击“服务器文件夹”。  
  
3. 在“服务器文件夹任务” 窗格中，执行以下操作之一：  
  
   1.  若要添加服务器文件夹，请单击“添加文件夹”。  
  
   2.  若要移动服务器文件夹，请选择你想要移动到新硬盘驱动器的文件夹，然后单击“移动文件夹”。  
  
   > [!NOTE]
   >  如果浏览到硬盘驱动器，选择它作为服务器文件夹位置而没有创建文件夹，将出现以下错误消息：**无法将根目录 (如 C:\\、D:\\) 添加为服务器文件夹。请创建一个新文件夹或在根目录下选择一个现有文件夹, 然后重试**。 若要解决此错误，请在新添加的硬盘驱动器上创建新文件夹，然后选择此新建文件夹作为存储服务器文件夹的位置。  
  
4. 按照说明完成向导。  
  
   有关移动服务器文件夹的详细信息，请参阅 [Add or move a server folder](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
###  <a name="BKMK_4b"></a>使用新的硬盘驱动器存储服务器备份  
 可以使用新添加的硬盘驱动器存储服务器备份。  
  
##### <a name="to-store-server-backups"></a>存储服务器备份  
  
1. 打开“仪表板”。  
  
2. 单击“设备”选项卡，从列表窗格中选择服务器，然后从“任务”窗格中，执行以下操作之一：  
  
   1. 如果未在服务器上配置“服务器备份”，请单击“设置服务器备份”。  
  
   2. 如果已在服务器上配置“服务器备份”，请单击“自定义服务器备份”。  
  
      将显示“设置服务器备份”向导。  
  
3. 在“选择备份目标” 页上，选择新的硬盘驱动器作为备份目标。  
  
4. 按照说明完成向导。  
  
###  <a name="BKMK_4c"></a>使用新的硬盘驱动器来增加存储池容量  
 当存储池容量不足时，你会收到一条警报，告知你可以通过使用“增加存储池容量”向导将新的硬盘驱动器添加到存储池来增加存储池容量。  
  
> [!NOTE]
>  仅当你已在服务器上创建存储池时才可以执行此步骤。  
  
##### <a name="to-increase-storage-pool-capacity"></a>增加存储池容量  
  
1.  打开“仪表板”。  
  
2.  单击“存储” 选项卡，然后单击“硬盘驱动器”。  
  
3.  选择显示低容量的驱动器。  
  
4.  从“任务”窗格中，选择“增加存储池容量”。 将显示“增加存储池容量”向导。  
  
5.  按照说明完成向导。  
  
##  <a name="BKMK_5"></a>存储空间概述  
 存储空间允许你将磁盘全部划分到存储池中。 然后你可以使用池容量创建存储空间。 存储空间是显示在仪表板的“硬盘驱动器”选项卡上的虚拟驱动器 你可以像使用其他驱动器一样使用存储空间, 因此可以很容易地处理其中的文件。 当池容量不足时，你可以创建较大的存储空间，然后将更多驱动器添加到存储池中。 如果存储池中有两个或更多的磁盘, 则可以使用不受驱动器故障 (而不是两个驱动器) 影响的双向镜像来创建存储空间 (如果创建三向镜像存储空间)。  
  
 若要创建存储空间，你只需一个或多个额外驱动器即可（除了安装 Windows 的驱动器）。 这些驱动器可以是内部或外部硬盘驱动器，也可以是固态驱动器。 你可以将各种类别的驱动器与存储空间结合使用，包括 USB、SATA 和 SAS 驱动器。  
  
> [!NOTE]
>  如果在运行 Windows Server Essentials 的服务器上配置存储空间, 则无法使用 "**清理数据**" 选项执行恢复出厂设置。 此问题的解决方法是先删除存储空间，然后使用“清理数据”选项执行恢复出厂设置。  
  
 有关存储空间的详细信息，请参阅 [存储空间常见问题 (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)。  
  
##  <a name="BKMK_6"></a>创建存储空间  
 若要开始在服务器上使用存储空间，必须满足以下最低要求：  
  
-   运行 Windows Server Essentials 的服务器必须连接到额外的物理驱动器（而不仅仅是启动驱动器），这些驱动器不得存储任何卷，并且必须具有最少 10 GB 的容量。 创建存储池需要一个物理驱动器；创建复原镜像存储空间需要最少两个物理驱动器。  
  
-   创建通过奇偶校验或双向镜像提供复原能力的存储空间，需要使用至少两个物理驱动器。  
  
> [!NOTE]
>  如果在运行 Windows Server Essentials 的服务器上配置存储空间, 则无法使用 "**清理数据**" 选项执行恢复出厂设置。 此问题的解决方法是先删除存储空间，然后使用“清理数据”选项执行恢复出厂设置。  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>在 Windows Server Essentials 中创建存储空间  
  
1. 将所有要与存储空间组合在一起的驱动器添加或连接到运行 Windows Server Essentials 的服务器。  
  
2. 在仪表板中, **单击 "高级":管理存储空间**。  
  
3. 单击“创建新池和存储空间”。  
  
4. 选择要添加到新存储空间的驱动器，然后单击“创建池”。  
  
5. 为驱动器分配名称和驱动器号，然后选择布局。 “双向镜像”、“三向镜像”和“奇偶校验”有助于保护存储空间中的文件免受驱动器故障影响。  
  
6. 输入存储空间可达到的最大大小，然后单击“创建存储空间”。  
  
   在 Windows Server Essentials 中, 你可以使用仪表板中的 "创建存储空间" 向导来创建双向镜像存储空间。  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>在 Windows Server Essentials 中创建存储空间  
  
1. 将所有要与存储空间组合在一起的驱动器添加或连接到运行 Windows Server Essentials 的服务器。  
  
2. 在仪表板中，单击“管理存储空间”。 将出现“存储空间”向导。  
  
3. 按照说明完成向导。  
  
   有关增加存储池容量信息，请参阅[使用新的硬盘驱动器来增加存储池容量](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理服务器文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
