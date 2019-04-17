---
title: "管理 Server 存储在 Windows Server Essentials"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: e0f65dfd25afbd584764d33904ba82e4da4c5443
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>管理 Server 存储在 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
   
 Windows Server Essentials 允许您管理你的所有服务器存储 （包括硬盘驱动器和存储空间） 从**硬盘驱动器**页面上**存储**仪表板中的选项卡。  
  
 以下各部分将帮助你提高服务器存储、 了解和使用存储空间和管理你的硬盘驱动器的信息：  
  
-   [管理硬盘驱动器使用仪表板](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [增加存储在服务器上](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [在硬盘上执行检查和修复](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [格式硬盘驱动器](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [添加新的硬盘](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [存储空间概述](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [创建存储空间](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>管理硬盘驱动器使用仪表板  
 Windows Server Essentials 允许您管理所有硬盘，连接到通过仪表板服务器。 仪表板上**存储**选项卡上，**硬盘驱动器**显示所有硬盘上的服务器来存储数据和服务器备份可用。 服务器监控的每个硬盘驱动器可用空间，并且硬盘空间不足时显示警报。 **硬盘驱动器**选项卡显示以下信息：  
  
-   每个硬盘驱动器的名称  
  
-   每个硬盘容量  
  
-   在每个硬盘上使用过空间量  
  
-   每个的硬盘中的可用空间量  
  
-   每个硬盘; 的状态空白状态意味着硬盘工作正常  
  
-   详细信息窗格中，其中如果所选的硬盘驱动器位于 （而非物理磁盘时） 的存储空间显示所有存储堆栈 （存储池中、 存储空间以及信息硬盘驱动器）  
  
 下表列出了及其说明仪表板中提供硬盘管理任务。 选择硬盘驱动器上时，才显示的一些任务。  
  
### <a name="available-hard-drive-management-tasks"></a>可用硬盘管理任务  
  
|任务名称|描述|  
|---------------|-----------------|  
|**查看硬盘驱动器属性**|打开*HardDriveName***属性**页面。 选择硬盘上时，将显示此任务。 **常规**的选项卡*HardDriveName*属性页中包括以下额外任务：<br /><br /> -   **清理驱动器**： 允许您清理 （此任务将仅适用于 Windows Server Essentials） 硬盘上未使用的文件。<br />-   **检查和修复**： 会硬盘上的文件系统错误检查和尝试自动修复检测到的错误。<br /><br /> **卷影副本**的选项卡*HardDriveName***属性**页面，可启用卷影副本。 此实验还将显示下次卷影副本按计划运行。|  
|**管理存储空间**|**注意：**对于 Windows Server Essentials，此任务将仅显示现有的存储空间时。<br /><br /> 打开**存储空间**控制面板，你可以创建并管理存储池和存储空间。|  
|**创建存储空间**|打开创建存储空间向导中，这允许你使用一个或多个硬盘驱动器提高存储池的容量。|  
|**增加存储池的容量**|**注意：**此任务中才可见所选的硬盘驱动器位于存储空间。<br /><br /> 打开增加存储池中向导中，这允许你使用一个或多个硬盘驱动器提高存储池的容量的容量。|  
  
##  <a name="BKMK_2"></a>增加存储在服务器上  
 提高存储在服务器上的，你可以添加到服务器其他内部硬盘。 若要添加其他内部硬盘，必须关闭服务器、 添加内部硬盘，，然后重新启动的服务器。 你不需要如果硬盘附加 SCSI 控制器关闭服务器。 在此情况下运行的服务器时接通电源硬盘。  
  
 根据格式化硬盘来添加是否，执行以下任一操作：  
  
-   **格式化**如果通过 NTFS 或 ReFS 进行格式化硬盘内部、服务器将它分配驱动器号，并且硬盘上会出现**硬盘驱动器**选项卡。你现在可以创建或将 server 文件夹移到新的硬盘。  
  
-   **不能格式化**下列通知格式化硬盘内部是否显示： 连接到服务器一个或多个格式化的硬盘。 使用下面的过程格式化硬盘。  
  
#### <a name="to-format-the-hard-disk"></a>若要格式化硬盘  
  
1.  打开的面板。  
  
2.  在导航窗格中，单击警报图标以启动**警报查看器**在 Windows Server Essentials，或**健康监视**Windows Server Essentials 中的选项卡。  
  
3.  在**警报查看器**或**健康监视**选项卡上，单击该通知，然后在任务窗格中，单击**解决此问题**。  
  
4.  按照说明来完成添加新硬盘驱动器向导。  
  
###  <a name="BKMK_Clean"></a>硬盘所清理  
  
1.  打开的面板。  
  
2.  在导航窗格中，单击**存储**，然后单击**硬盘驱动器**。  
  
3.  在**硬盘驱动器**部分中，选择分配到新添加的硬盘中，并在任务栏中的驱动器号，单击**查看硬盘驱动器属性**。  
  
4.  在**< Driveletter\ > 属性**，在**常规**选项卡上，单击**清理驱动器**。  
  
##  <a name="BKMK_Check"></a>在硬盘上执行检查和修复  
 硬盘检查和修复流程验证的硬盘上存储文件系统运行状况。 运行**chkdsk**进程上备份的文件存储在的音量。 以下通知的问题可以解决通过运行检查和修复上硬盘：  
  
-   必须签一个或多个中的硬盘服务器备份。  
  
#### <a name="to-check-and-repair-hard-drives"></a>若要检查和修复硬盘驱动器  
  
1.  打开的面板。  
  
2.  单击**服务器文件夹和硬盘驱动器**，然后单击**硬盘驱动器**。  
  
3.  选择显示错误，硬盘，然后选择**查看硬盘驱动器属性**。  
  
4.  在**检查和修复**选项卡上，单击**检查和修复**。  
  
##  <a name="BKMK_3"></a>格式硬盘驱动器  
 在服务器上检测到时未格式化内部硬盘驱动器，健康警报指导完成格式化它的用户。 添加新的难度驱动器向导将指导你完成格式化硬盘，并使您可以通过以下方法之一配置硬盘上：  
  
1.  格式化硬盘，并自动在其上创建一个驱动器。 如果您选择此选项，请完成向导后，使用 NTFS 文件系统格式化硬盘逻辑将创建一个。  
  
2.  格式化硬盘，并将其设置为服务器备份。 如果您选择此选项，设置服务器备份向导启动，并且它将指导你完成服务器备份配置。  
  
3.  如果的存储空间不存在，请使用新的硬盘上创建存储空间。 你必须至少两个硬盘，创建存储空间。  
  
4.  如果已经存在的存储空间，可以使用新的硬盘提高存储池的容量。 如果现有创建服务器上的存储空间，仅显示此选项。 如果您选择此选项，则向导将向存储池中添加此硬盘。  
  
##  <a name="BKMK_4"></a>添加新的硬盘  
 运行 Windows Server Essentials 服务器中插入新硬盘驱动器上，你可以：  
  
-   [使用新的硬盘存储服务器文件夹](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [使用新的硬盘存储 server 备份](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [使用新的硬盘提高存储池的容量](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>使用新的硬盘存储服务器文件夹  
 若要使用新的硬盘存储服务器文件夹，可以将新的服务器文件夹添加到硬盘或移动到硬盘现有服务器文件夹。  
  
##### <a name="to-store-server-folders"></a>存储在服务器文件夹  
  
1.  打开的面板。  
  
2.  单击**存储**选项卡，然后单击**服务器文件夹**。  
  
3.  在**服务器文件夹任务**窗格中，执行以下任一操作：  
  
    1.  若要添加服务器文件夹，请单击**添加文件夹**。  
  
    2.  若要移动服务器文件夹，请选择你想要移动到新的硬盘，然后单击文件夹**移动文件夹**。  
  
    > [!NOTE]
    >  如果你浏览至硬盘，然后选中它作为服务器文件夹位置而无需创建一个文件夹，以下错误消息将出现：**根目录 （如 C:\\，D:\\) 无法添加为服务器文件夹。创建一个新文件夹或选择一个现有下根目录，然后再试**。 若要解决此错误中添加了新的硬盘，, 创建一个新文件夹，然后选择作为保存位置以存储服务器文件夹的新建文件夹。  
  
4.  按照说明来完成向导。  
  
 有关移动服务器文件夹的详细信息，请参阅[添加或移动服务器文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
###  <a name="BKMK_4b"></a>使用新的硬盘存储 server 备份  
 新添加的硬盘可以用于存储 server 备份。  
  
##### <a name="to-store-server-backups"></a>若要存储 server 备份  
  
1.  打开的面板。  
  
2.  单击**设备**选项卡，在列表窗格中，选择的服务器，然后从任务栏中，执行下列操作之一：  
  
    1.  如果未在服务器上配置服务器的备份，请单击**设置服务器备份**。  
  
    2.  如果在服务器上配置服务器的备份，请单击**自定义服务器备份**。  
  
     显示设置服务器备份向导。  
  
3.  在**选择备份目标**页上，选择新的硬盘为备份的目标。  
  
4.  按照说明来完成向导。  
  
###  <a name="BKMK_4c"></a>使用新的硬盘提高存储池的容量  
 当你存储池的容量不足时，你会收到指出，你可以通过使用增加容量的一个存储池中向导向存储池中添加新的硬盘增加存储池的容量的警报。  
  
> [!NOTE]
>  仅当你在服务器上创建一个存储池中，你可以执行此过程。  
  
##### <a name="to-increase-storage-pool-capacity"></a>增加存储池的容量  
  
1.  打开的面板。  
  
2.  单击**存储**选项卡，然后单击**硬盘驱动器**。  
  
3.  选择显示低容量的驱动器。  
  
4.  从任务窗格中，选择**增加存储池的容量**。 增加存储池中向导容量显示。  
  
5.  按照说明来完成向导。  
  
##  <a name="BKMK_5"></a>存储空间概述  
 存储空间可让你进行分组在一起磁盘存储池中。 然后你可以使用池的容量来创建存储空间。 存储空间将显示在的虚拟驱动器**硬盘驱动器**仪表板中的选项卡。 因此，你可以使用存储空间像其他驱动器，它可以轻松地在其上使用文件。 你在运行时低池的容量，你可以创建较大的存储空间，并向存储池中添加更多驱动器。 如果你有两个或多个磁盘存储池中内时，你可以使用驱动器发生故障的不会影响双向镜像创建存储空间？ 两个驱动器发生故障甚至？ 如果创建三向镜像存储空间。  
  
 若要创建存储空间，你只需要是一个或多个额外的驱动器另外安装了 Windows 的驱动器。 这些驱动器既可以是内部或外部硬盘驱动器或固态驱动器。 你可以使用各种类型的驱动器用于存储空间，包括 USB、 SATA 和 SAS 驱动器。  
  
> [!NOTE]
>  如果你运行的 Windows Server Essentials 服务器上配置存储空间，你无法执行与恢复出厂设置**干净数据**选项。 对于此问题解决方法是先删除存储空间，然后再执行与恢复出厂设置**干净数据**选项。  
  
 存储空间的详细信息，请参阅[存储空间通常要求问题 （常见问题）](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)。  
  
##  <a name="BKMK_6"></a>创建存储空间  
 若要开始使用服务器上的存储空间，必须满足以下最低要求：  
  
-   运行 Windows Server Essentials 服务器都必须连接到其他物理驱动器 （不仅仅启动硬盘）、 驱动器不必须维护任何卷，并且必须最少 10 GB 的容量。 Onedrive 物理需要创建一个存储池中;创建存储复原镜像空间需要至少两个物理驱动器。  
  
-   创建存储空间使用复原通过奇偶校验或双向镜像需要至少两个物理驱动器。  
  
> [!NOTE]
>  如果你运行的 Windows Server Essentials 服务器上配置存储空间，你无法执行与恢复出厂设置**干净数据**选项。 对于此问题解决方法是先删除存储空间，然后再执行与恢复出厂设置**干净数据**选项。  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>若要在 Windows Server Essentials 创建存储空间  
  
1.  添加或连接要一起以用作存储空间分组到运行 Windows Server Essentials 服务器的所有驱动器。  
  
2.  从仪表板中，单击**高级： 管理存储空间**。  
  
3.  单击**创建新池和存储空间**。  
  
4.  选择你想要添加到新存储空间，然后单击的驱动器**创建池**。  
  
5.  为的名称和驱动器号的驱动器，然后选择布局。 **双向镜像**，**三向镜像**，并**奇偶校验**可以帮助保护存储空间中的文件在驱动器发生故障。  
  
6.  输入存储空间可以达到，，然后单击最大大小**创建存储空间**。  
  
 在 Windows Server Essentials，你可以创建双向镜像的空间使用创建存储空间向导从仪表板。  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>若要在 Windows Server Essentials 创建存储空间  
  
1.  添加或连接要一起以用作存储空间分组到运行 Windows Server Essentials 服务器的所有驱动器。  
  
2.  从仪表板中，单击**管理存储空间**。 创建存储空间向导显示。  
  
3.  按照说明来完成向导。  
  
 增加存储池的容量的信息，请参阅[使用新的硬盘提高存储池的容量](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理 Server 文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
