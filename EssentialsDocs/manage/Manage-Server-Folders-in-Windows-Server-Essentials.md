---
title: "管理 Windows Server Essentials 服务器文件夹"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1f3640f21b95acafa850b2204cd52f9c0f324e5
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>管理 Windows Server Essentials 服务器文件夹

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 作为服务器管理员，你可以管理 （也称为共享文件夹当访问来自启动栏、 远程访问 Web、 服务器我的 Windows Phone 的应用或我服务器应用适用于 Windows 8） 的任何 server 文件夹的访问权限服务器上通过任务**服务器文件夹**仪表板，使用户不同级别的访问权限各种文件的选项卡。  
  
 以下主题提供的信息将帮助您了解、 创建和管理 server 文件夹：  
  
-   [管理 server 文件夹使用仪表板](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [管理 server 文件夹的访问权限](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [添加或移动服务器文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [添加缺少服务器文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [了解共享的文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [了解卷影副本](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>管理 server 文件夹使用仪表板  
 Windows Server Essentials 便可以通过仪表板中执行常见的管理任务。 **服务器文件夹**仪表板中的页面将提供以下：  
  
-   服务器文件夹，其中显示列表：  
  
    -   文件夹的名称  
  
    -   文件夹的说明  
  
    -   文件夹的位置  
  
    -   文件夹位置，则可用的可用空间量  
  
    -   有关任何文件夹; 执行的任务简短的状态信息**状态**字段将为空，如果该文件夹是否正常运行，并且没有任务正在运行  
  
-   可能会提供有关选定的文件夹的其他信息的详细信息窗格中  
  
-   包含一组文件夹相关的管理任务的任务窗格  
  
 下表介绍了各种服务器文件夹任务 Windows Server Essentials 仪表板上可用。 大部分任务是特定的文件夹，，当你在列表中选择某个文件夹，它们是才可见。  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>服务器仪表板上的文件夹任务  
  
|任务名称|描述|  
|---------------|-----------------|  
|打开该文件夹|显示在文件资源管理器 （在以前版本的 Windows 中称为 Windows 资源管理器） 中选定的文件夹中的内容。|  
|删除该文件夹|使您能够删除的用户创建文件夹。 此任务中不可用服务器安装创建默认文件夹。|  
|移动文件夹|打开向导中，可帮助你将服务器文件夹移到新位置。|  
|停止共享文件夹|将停止共享选定的文件夹，但不会删除。 如果不会再共享文件夹，它没有仪表板中。 此任务中不可用服务器安装创建默认文件夹。|  
|查看文件夹属性|将显示属性选定的文件夹，并使您可以：<br /><br /> -更改的用户创建文件夹的名称。<br /><br /> -更改选定的文件夹的说明。<br /><br /> -查看文件夹的大小。<br /><br /> -在文件资源管理器中打开选定的文件夹。<br /><br /> -指定选定的文件夹的用户帐户的访问权限。<br /><br /> -隐藏远程网站访问和 Web 服务的应用程序中选定的文件夹。<br /><br /> -指定文件夹配额。|  
|将文件夹添加|可帮助你创建一个新的服务器文件夹和指定允许的每个用户帐户的访问权限的级别。|  
|了解服务器文件夹|打开 internet 上描述的使用和功能服务器文件夹帮助主题。|  
  
##  <a name="BKMK_1"></a>管理 server 文件夹的访问权限  
 Windows Server Essentials 可以让你通过使用服务器文件夹中心位置为你客户端计算机上存储的文件。 将你的文件存储在服务器文件夹可确保你的文件在始终是安全的方式每个客户可以访问的位置。  
  
 使用服务器文件夹存储你的文件，您可以：  
  
-   通过使用服务器备份和还原来帮助防御总服务器故障备份服务器文件夹。  
  
-   通过适用于 Windows Phone 和 Windows 8 中使用 Internet 浏览器通过远程 Web 访问，或通过我服务器应用存储从任何位置服务器文件夹的访问文件。  
  
-   新的服务器文件夹从任何客户端计算机的访问。  
  
 你可以通过使用上的任务管理服务器上的任何 server 文件夹的访问**服务器文件夹**仪表板中的选项卡。 下表列出了默认情况下创建的安装 Windows Server Essentials 或打开服务器上的媒体流式传输时服务器文件夹。  
  
|服务器文件夹名称|描述|  
|------------------------|-----------------|  
|客户端计算机备份|默认情况下，Windows Server Essentials 创建客户端计算机备份存储在该文件夹中。 客户端计算机备份的设置可以进行修改通过网络管理员联系。|  
|公司|用来存储和访问与你的组织网络用户的相关文档。|  
|文件历史记录备份|默认情况下，Windows Server Essentials 使用文件历史记录来创建存储在该文件夹的文件备份。 这些文件历史记录设置可以进行修改的网络管理员。|  
|文件夹重定向|用来存储和访问已设置为文件夹重定向网络用户的文件夹。|  
|用户|用来存储和访问网络用户的文件。 用户特定文件夹将自动在生成**用户**服务器为你创建的每个网络用户帐户的文件夹。|  
|音乐|用来存储和访问网络用户的音乐文件。 当你打开 media 共享，则该文件夹是可用。|  
|图片|用来存储和访问网络用户的图片文件。 当你打开 media 共享，则该文件夹是可用。|  
|录制的电视节目|用来存储和访问网络用户录制的电视节目。 当你打开 media 共享，则该文件夹是可用。|  
|视频|用来存储和访问网络用户的视频文件。 当你打开 media 共享，则该文件夹是可用。|  
  
 若要隐藏或服务器文件夹，请设置或修改服务器文件夹属性，请参阅下面的过程：  
  
-   [隐藏服务器文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [设置权限服务器文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [查看或修改服务器文件夹属性](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>隐藏服务器文件夹  
 与网络管理员，你可以选择隐藏的任何这些服务器文件夹和防止它们显示远程网站访问的网站或 Web 服务 （如我服务器） 的应用程序。  
  
> [!NOTE]
>  你必须要执行此过程网络管理员联系。  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>若要隐藏服务器文件夹显示在远程访问 Web  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击**存储**，然后单击**服务器文件夹**。  
  
3.  在列表中视图中，选择你想要查看或修改其属性服务器文件夹。  
  
4.  在**< ServerFolder\ > 任务**窗格中，单击**查看文件夹属性**。  
  
5.  在**< FolderName\ > 属性**，单击**共享**、 选择**隐藏远程网站访问和 Web 服务的应用程序从该文件夹**，然后单击**应用**。  
  
###  <a name="BKMK_Perms"></a>设置权限服务器文件夹  
 对于通过仪表板添加服务器的任何其他 server 文件夹，你可以选择它三个不同的访问设置：  
  
-   **读取/写入**  
  
     如果你想要允许此人创建、 更改，并删除服务器文件夹中的任何文件，请选择该设置。  
  
-   **仅阅读**  
  
     如果你想要允许此人仅阅读服务器文件夹中的文件，请选择该设置。 仅阅读访问权限的用户无法创建、 更改或删除服务器文件夹中的任何文件。  
  
-   **不能访问**  
  
     如果你不希望此人访问服务器文件夹中的任何文件，请选择该设置。  
  
> [!IMPORTANT]
>  文件夹属性中显示的权限代表只能由仪表板的用户。 它们不包括如组或所服务帐户、 用户权限，或者包含任何授予权限，可以通过使用其他本机工具文件夹上设置或包括通过仪表板而未添加的用户。  
  
> [!NOTE]
>  你必须要执行此过程网络管理员联系。  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>在服务器上设置为服务器文件夹的权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击**存储**，然后单击**服务器文件夹**。  
  
3.  在列表中视图中，选择你想要查看或修改其属性服务器文件夹。  
  
4.  在**< ServerFolder\ > 任务**窗格中，单击**查看文件夹属性**。  
  
5.  在**< FolderName\ > 属性**，单击**共享**，并选择列出的用户帐户适当的用户的访问权限级别，然后单击**应用**。  
  
> [!NOTE]
>  默认情况下，当你添加到你的网络的用户帐户是子文件夹中创建用户下**用户**服务器上的文件夹。 仅用户或管理员，可以从网络计算机访问子文件夹。 设置了权限的每个子文件夹下**用户**，所以没有常规访问权限的顶级**用户**文件夹。  
  
> [!NOTE]
>  你无法修改的共享权限**文件历史记录备份**，**文件夹重定向**，并**用户**服务器文件夹。 因此，这些服务器文件夹的文件夹属性不包括**共享**选项卡。  
  
###  <a name="BKMK_10"></a>查看或修改服务器文件夹属性  
 你可以修改服务器文件夹的名称，其说明，并定义的用户帐户具有访问服务器文件夹通过**查看文件夹属性**任务上**服务器文件夹**仪表板中的选项卡。  
  
> [!NOTE]
>  你还可以在 Windows Server Essentials 和 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色，修改文件夹配额。  
  
##### <a name="to-view-or-modify-folder-properties"></a>若要查看或修改文件夹属性  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击**存储**，然后单击**服务器文件夹**。  
  
3.  在列表中视图中，选择你想要查看或修改其属性服务器文件夹。  
  
4.  在**< ServerFolder\ > 任务**窗格中，单击**查看文件夹属性**。  
  
5.  在**< Foldername\ > 属性**，在**常规**选项卡上，查看或修改的名称和 server 文件夹的说明。  
  
    > [!NOTE]
    >  你还可以在 Windows Server Essentials 和 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色，修改文件夹配额服务器文件夹达到其指定的大小时提供一条警告消息。  
  
##  <a name="BKMK_5"></a>添加或移动服务器文件夹  
 你可以**添加更多服务器文件夹**来存储您在安装期间创建默认服务器文件夹除了服务器上的文件。 在主服务器或运行 Windows Server Essentials 成员服务器上，你可以添加服务器文件夹。  
  
 你可以**移动服务器文件夹**位于上运行的 Windows Server Essentials 主服务器，并在显示**服务器文件夹**仪表板到另一台硬盘驱动器需要时使用移动文件夹向导的选项卡。 如果，您可以移动到另一个硬盘位置地址服务器文件夹：  
  
-   数据硬盘上不会再具有足够空间来存储数据。  
  
-   你想要更改默认存储位置。 更快的移动，请考虑移动服务器文件夹，虽然它不包含的任何数据。  
  
-   你想要删除现有的硬盘驱动器，而不会丢失均位于服务器文件夹。  
  
 在移动之前文件夹，确保：  
  
-   确保你具有备份你的服务器。  
  
-   确保所有客户端备份会停止，并不正在进行，如果你打算移动客户端计算机备份文件夹。 移动客户端计算机备份文件夹，时服务器无法完成文件夹移动，直到备份任何客户端计算机。  
  
-   请确保该服务器未执行关键系统的任何操作。 完成任何更新或正在进行，开始文件夹之前的备份移动或此过程可能需要长时间才能完成建议。  
  
-   移动文件夹中的文件都使用。 你将无法在移动时，请访问服务器文件夹。  
  
 如果 server 文件夹中的文件实现以下技术，则不支持移动从 NTFS ReFS 到的文件夹：  
  
-   备用数据流量  
  
-   对象 Id  
  
-   简称 （8.3 的名称）  
  
-   压缩  
  
-   EFS 加密  
  
-   事务 NTFS，TxF （引入了 Windows Vista 与）  
  
-   稀疏文件  
  
-   硬链接  
  
-   扩展属性  
  
-   配额  
  
###  <a name="BKMK_6"></a>若要添加或移动服务器文件夹的位置  
 通常，你应该添加，或移动服务器文件夹拖动到硬盘驱动器拥有最大的可用空间量。 如果可能，请避免添加或移动到系统驱动器 （如 c:） 共享的文件夹，因为这可能会使一下，才能进行操作系统和它的更新的必要的驱动器空间。 此外，请避免添加或服务器文件夹移动到外部硬盘驱动器，因为它们可能方便地断开，并且这样一来，你可能无法访问你的文件。 相反，我们建议你在内部驱动器上创建文件夹。  
  
 无法添加服务器文件夹或将其移到以下位置，将导致错误，如果对于添加或移动选择以下任一位置，则：  
  
-   未使用 NTFS 或 ReFS 文件系统进行格式化硬盘  
  
-   %Windir%文件夹  
  
-   映射的网络驱动器  
  
-   包含的共享的文件夹的文件夹  
  
-   位于下硬盘驱动器**可移动存储设备**  
  
-   是根目录的硬盘驱动器 （如 C:\\，D:\\，E:\\)  
  
-   现有的共享文件夹的子文件夹  
  
-   安装了 Windows Server Essentials 体验角色与运行 Windows Server Essentials 或 Windows Server 2012 R2 成员服务器  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>步骤来添加或移动服务器文件夹  
  
> [!NOTE]
>  你必须服务器管理员才能完成这些步骤。  
  
##### <a name="to-add-a-server-folder"></a>若要添加服务器文件夹  
  
1.  打开的面板。  
  
2.  单击**存储**，然后单击**服务器文件夹**。  
  
3.  在**服务器文件夹任务**，单击**添加文件夹**。 这将启动添加文件夹向导。  
  
4.  按照说明来完成向导。  
  
    > [!NOTE]
    >  -   如果你浏览特定文件夹通过使用浏览按钮来指定 server 文件夹位置，所导航到该文件夹被添加为服务器文件夹中。  
    > -   您可以通过远程访问 Web 可以访问哪些服务器文件夹。 有关详细信息，请参阅[管理访问服务器文件夹](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)。  
  
##### <a name="to-move-a-server-folder"></a>若要移动服务器文件夹  
  
1.  打开的面板。  
  
2.  单击**存储**，然后单击**服务器文件夹**。  
  
3.  从 server 文件夹的列表中，选择你想要移动的文件夹。  
  
4.  在任务窗格中，单击**移动文件夹**。  
  
5.  按照说明来完成向导。  
  
##  <a name="BKMK_9"></a>添加缺少服务器文件夹  
 服务器检测到时，预定义的服务器文件夹？公司、 用户、 客户端计算机备份、 文件历史记录备份，或文件夹重定向？ 不再共享 （适用于某种原因或另一个），生成警报指导用户，若要解决此问题。 建议你尝试从 server 备份中还原该文件夹。 但是，如果该服务器未备份，选择缺少文件夹，然后单击**重新创建缺少文件夹**要重新配置服务器文件夹的位置。  
  
> [!NOTE]
>  仅预定义的文件夹？公司、 用户、 客户端计算机备份、 文件历史记录备份，或文件夹重定向？ 可以重新创建。 无法重新创建用户创建服务器文件夹和媒体 server 文件夹。  
  
 在恢复或重新创建缺少文件夹后，它应该不会再列为**失踪**。  
  
 从 server 备份中还原文件的信息，请参阅部分了解更关于还原的文件和文件夹主题中的[管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)。  
  
##  <a name="BKMK_11"></a>了解共享的文件夹  
 有多种不同的方法，你可以访问你的共享的文件夹 Windows Server Essentials 从连接到服务器的设备。 有关详细信息，请参阅主题[使用共享的文件夹](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)。  
  
##  <a name="BKMK_Shadow"></a>了解卷影副本  
 服务器卷影副本，用户可以查看共享的文件和文件夹的以前的时间点。 访问以前版本的文件或卷影副本非常有用，原因是用户可以：  
  
1.  **恢复不小心删除的文件**。 如果你无意中删除某个文件，你可以打开以前的版本，并将其复制到某个安全的位置。  
  
2.  **恢复意外覆盖文件**。 如果意外覆盖某个文件时，你可以恢复以前版本的文件。 (版本数取决于你有多少快照创建。)  
  
3.  **在工作时文件的版本进行比较**。 当你想要检查版本的文件之间发生了什么变化，你可以使用以前的版本。  
  
 若要使用的客户端计算机卷影副本，请右键单击服务器共享的文件夹，然后选择**还原以前的版本**。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理服务器存储](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [使用共享的文件夹](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
