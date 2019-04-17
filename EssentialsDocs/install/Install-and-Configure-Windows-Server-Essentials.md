---
title: "安装和配置 Windows Server Essentials"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cdad118c30fbf303b55ec7ea25bbe3e209c016db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="install-and-configure-windows-server-essentials"></a>安装和配置 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 本文提供了有关安装和配置 Windows Server Essentials 的分步说明进行操作。 在开始安装之前，请查看并完成的任务，述[之前安装的 Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md)。  

 本文提供了有关安装和配置 Windows Server Essentials 的分步说明进行操作。 在开始安装之前，请查看并完成的任务，述[之前安装的 Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md)。  
  
 你在安装和配置 Windows Server Essentials 两个步骤：  
  

1.  [第 1 步：安装 Windows Server Essentials 操作系统](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation)在此步骤，安装操作系统服务器上。  
  
2.  [第 2 步：配置 Windows Server Essentials 操作系统](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure)通过提供有关你的公司、域设置和网络管理员联系信息此步骤，以完成安装。 此信息用于获取服务器随时可供使用。  

1.  [第 1 步：安装 Windows Server Essentials 操作系统](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation)在此步骤，安装操作系统服务器上。  
  
2.  [第 2 步：配置 Windows Server Essentials 操作系统](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure)通过提供有关你的公司、域设置和网络管理员联系信息此步骤，以完成安装。 此信息用于获取服务器随时可供使用。  

  
###  <a name="BKMK_ManualInstallation"></a>第 1 步：安装 Windows Server Essentials 操作系统  
  
> [!IMPORTANT]
>  操作系统安装后，不自定义你服务器直到你完成[第 2 步：配置 Windows Server Essentials 操作系统](#BKMK_Step2Configure)。  
  
 **预计时间：**约为 30 分钟。  
  
> [!NOTE]
>  在此过程整个预计的完成时间基于最低硬件要求。  
  
##### <a name="to-install-the-operating-system"></a>若要安装的操作系统  
  
1.  将你的计算机连接到你的网络通过网络电缆。  
  
    > [!IMPORTANT]
    >  在安装期间不断开你的计算机连接从网络。 这样可能会导致安装失败。  
  
2.  打开你的计算机和 DVD 驱动器中插入 Windows Server Essentials DVD。  
  
     如果你正在执行无人照看的安装，连接可移动媒体（如软盘或 U 盘），其中包含你的答案文件。 根据你的答案文件的内容，你可能无法看到部分或全部以下安装屏幕。  
  
3.  重启计算机。 当消息**按任意键从 CD 或 DVD 启动**显示，请按任意键。  
  
    > [!NOTE]
    >  如果你的计算机未从 DVD 启动，请确保 CD-ROM 驱动器首先列出 BIOS 启动序列中。 有关 BIOS 启动序列的详细信息，请参阅从计算机制造商的文档。  
  
4.  选择**语言**你想要安装，**时间和货币格式**，并**键盘或输入的法**，然后单击**下一步**。  
  
5.  单击**立即安装**。  
  
6.  在**输入的产品密钥**，输入产品密钥。  
  
7.  朗读**许可条款**。 如果你接受这些条款，请选择**我接受许可条款**复选框，然后依次单击**下一步**。  
  
    > [!NOTE]
    >  如果您不选择接受许可条款，则不会继续安装。  
  
8.  在**您需要哪种类型的安装？**，单击**自定义：安装 Windows（高级）**  
  
9. 在**你希望在将 Windows 安装？**，请选择要安装 Windows 操作系统的硬盘。 验证你的内部硬盘驱动器的所有可用于安装。  
  
    > [!IMPORTANT]
    >   Windows Server Essentials 必须安装作为 c： 音量和卷的大小必须至少 60 GB。 建议你在您的操作系统磁盘上创建两个分区不使用 c: （系统分区） 存储业务的任何数据。  
  
    > [!NOTE]
    >  如果未列出（例如，串行高级技术附件 (SATA) 的硬盘）硬盘驱动器上，你必须加载该硬盘设备驱动程序。 获取设备的驱动程序来自制造商并将其保存到可移动媒体（例如软盘或 U 盘）。 将可移动媒体连接到你的计算机，然后单击**加载驱动程序**。  
  
     如果你需要删除和/或创建分区，请参阅以下步骤：  
  
    1.  若要删除分区，选择分区、单击**驱动器选项（高级）**，然后单击**删除**。 你可以删除系统分区后，使用任何一步中的说明创建新分区**b**或步骤**c**。  
  
        > [!NOTE]
        >  单击后**驱动器选项（高级）**，，将不会再显示选项。 这种情况下跳过驱动器的选项是指步骤的一部分。  
  
    2.  若要从未分区的空间，创建分区，单击你想要分区、单击硬盘**驱动器选项（高级）**，单击**新建**，然后在**大小**文本框中，键入你想要创建分区大小。 例如，如果你使用的 120 千兆字节 (GB) 的推荐的分区大小，键入**122880**，然后单击**应用**。 创建分区后，单击**下一步**。 分区格式化之前会继续安装。  
  
    3.  若要创建的分区使用所有未分区的空间，请单击硬盘分区、单击你希望**驱动器选项（高级）**，单击**新建**，然后单击**应用**以接受默认分区大小。 创建分区后，单击**下一步**。 分区格式化之前会继续安装。  
  
        > [!IMPORTANT]
        >  完成此步骤后，不能操作系统移动到另一个分区。  
  
 在安装期间，临时文件复制到你大约有 30 分钟的计算机上安装文件夹中。 Windows Server Essentials 操作系统安装后，重新启动你的计算机。 现在，现在可以配置 Windows Server Essentials 操作系统。  
  
###  <a name="BKMK_Step2Configure"></a>第 2 步：配置 Windows Server Essentials 操作系统  
  
> [!IMPORTANT]
>  如果要迁移到 Windows Server Essentials 从以前版本的 Windows 小型企业服务器，你必须遵循不同的进程。 有关迁移安装的信息，请参阅：  
>   
>  -   [从 Windows SBS 2003 迁移](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
> -   [从 Windows SBS 2008 迁移](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 在安装此阶段，提示你回答几个有关你的组织的问题。 此信息用于配置操作系统。  
  
> [!IMPORTANT]
>  在开始之前此步骤，请确保，本地网络适配器已连接到路由器或开关处于打开状态，且正常工作。  
  
 **预计时间：**约为 30 分钟  
  
##### <a name="to-configure-the-operating-system"></a>若要配置操作系统  
  
1.  在**验证日期和时间设置**页上，单击**更改系统日期和时间设置**选择服务器的日期、时间和时区的区域设置。 当你完成后，请单击**下一步**。  
  
    > [!IMPORTANT]
    >  如果你在虚拟机上安装 Windows Server Essentials，请确保你选择主机操作系统正在使用的相同的时区的区域设置。 如果时区的区域设置而有所不同，可能会失败服务器安装。  
  
2.  在**选择服务器安装模式**页上，执行下列操作之一：  
  
    1.  选择**全新安装**来设置 Windows Server Essentials 服务器软件的完整的新安装。  
  
    2.  选择**Server 迁移**来安装 Windows Server Essentials 和到现有的 Windows 域加入该服务器。  
  
3.  在**个性化服务器**页上，输入你的组织的名称、内部域名和服务器名称。  
  
    > [!IMPORTANT]
    >  服务器名称必须网络上的一个唯一的名称。 完成此步骤后，你无法更改服务器名称或内部域名。  
  
4.  单击**下一步**。  
  
5.  在**提供你的管理员帐户信息**页上，键入新管理员帐户信息。  
  
    > [!CAUTION]
    >  请命名的网络管理员帐户，管理员或网络管理员联系。 系统将这些帐户名称保留用于。  
  
6.  在**提供你标准用户帐户信息**页上，键入新的标准用户帐户的信息，然后单击**下一步**。  
  
7.  在**使服务器自动保持最新**页上，选择你想要接收 Windows 更新服务器，然后单击**下一步**。  
  
8.  **更新并准备服务器**页面显示的最终安装过程进度。 这将时间才能完成，并计算机将重新启动几次足球。  
  
9. 最后一个服务器后重启，则**服务器已准备好使用**页面显示时。 单击**关闭**。  
  
10. 单击仪表板磁贴**开始**屏幕上，并在仪表板上完成**设置我服务器**任务上**家庭**页面。 立即在 Windows Server Essentials 安装完成后，你应该完成这些任务。  
  
> [!NOTE]
>  安装完成后，你将自动登录到与新管理员帐户，在安装过程中添加了服务器。 内置的管理员帐户密码已设置为新的管理员帐户和密码相同，然后禁用了内置的管理员帐户。  
  
 无需输入产品密钥，你可以为一个有限的时间（也称为试用期）使用新安装的服务器。 评估到期后，你必须输入产品密钥来激活服务器或扩展试用期。 你可以扩展试用期 a 最大的两次。 当你达到的最大的数天试用期允许时，必须先使用产品密钥激活服务器。  
  
### <a name="customize-windows-server-essentials"></a>自定义 Windows Server Essentials  
 **家庭**网页 Windows Server Essentials 仪表板中的链接到**设置**应该后立即你完成任务，安装您的服务器。 通过执行这些任务，你可以帮助保护存储在服务器的信息，并启用适用于 Windows Server Essentials 功能。  
  
 如果您不选择执行的任务，用户可能没有某些网络功能的访问权限。 若要稍后返回到这些任务，返回到 Windows Server Essentials 仪表板**家庭**页面。  
  
 下表中的设置的任务列表定义可能会出现的项目。  
  
|任务|描述
|----------|-----------------|  
|获取其他 Microsoft 产品的更新|单击此任务中访问运行的工具，可使您明确指定你希望使用 Microsoft 更新自动获取更新，Windows Server Essentials 和其他 Microsoft 产品，如 Office 的链接。  
|添加用户帐户|单击以查看有关添加用户帐户的简短信息此任务。 若要运行的链接**添加用户帐户向导**提供。 有关详细信息，请参阅[添加用户帐户](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。  
|添加服务器文件夹|单击以查看有关添加服务器文件夹简要信息此任务。 若要运行的链接**添加文件夹向导**提供。 此外提供了是关于使用服务器文件夹联机帮助主题的链接。 有关详细信息，请参阅[添加或移动服务器文件夹](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。 
|设置服务器备份|单击以查看有关使用服务器备份，以保护你的数据简要信息此任务。 若要运行的链接**设置服务器备份向导**提供。 有关详细信息，请参阅[设置或自定义服务器备份](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)。 
|随时随地访问设置|单击以查看有关的任何地方访问功能的简短信息 Windows Server Essentials 在此任务。 链接**任何地方访问设置**提供了页。 有关详细信息，请参阅[管理任何地方访问](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)。 
|电子邮件通知设置|单击以查看有关电子邮件通知简要信息此任务。 若要运行的链接**设置电子邮件通知的通知**工具仅提供。 有关详细信息，请参阅[设置电子邮件通知的通知](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)。  
|将介质服务器设置|单击以查看有关使用媒体服务器共享音乐、视频和映像文件简要信息此任务。 链接**媒体设置**提供了页。 此外提供了是联机帮助主题以了解有关媒体服务器的详细信息的链接。 有关详细信息，请参阅[管理数字媒体](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)。 
|连接两台计算机|单击以查看大致了解如何连接到服务器的网络计算机此任务。 有关详细信息，请参阅[将计算机连接到服务器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。
  
## <a name="see-also"></a>请参阅  
  
-   [安装 Windows Server Essentials](Install-Windows-Server-Essentials.md)

