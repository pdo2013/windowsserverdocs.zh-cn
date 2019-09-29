---
title: 为工作站创建 Windows 10 企业版虚拟桌面
description: 了解如何创建适用于工作站的 Windows Server 2016 桌面
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: cd08caef8228a4d20c6d5f4a40fe5bd90aacbe40
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395535"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>为工作站创建 Windows 10 企业版虚拟桌面
MultiPoint Services 中的此可选配置主要用于以下情况：对于每个用户，基本应用程序需要其自己的客户端操作系统实例。 示例包括无法在 Windows Server 上安装的应用程序，以及不会在同一台主机上运行多个实例的应用程序。  
  
> [!NOTE]  
> 这些虚拟桌面（也称为 VDI）比默认的 MultiPoint 服务桌面会话需要更多的资源，因此我们建议尽可能使用默认的 MultiPoint 服务会话。  
  
## <a name="prerequisites"></a>先决条件  
若要准备创建工作站虚拟桌面，请确保 MultiPoint 服务系统满足以下要求：      
  
|硬件|要求|         |
|------------|----------------|----------------| 
|CPU （多媒体）|每个虚拟机1个核心或线程|  
|固态硬盘（SSD）|容量 > = 每个工作站 20 gb + 40 GB 用于 MultiPoint 服务主机操作系统<br /><br />随机读取\/写入 IOPS > = 每个工作站3K|  
|RAM|每个工作站 2GB + 2GB 用于 Windows MultiPoint Server 主机操作系统|  
|图形|DX11|  
|BIOS|BIOS CPU 设置配置为启用虚拟化–第二级地址转换（SLAT）|  
  
-   **工作站**-为 MultiPoint 服务系统设置工作站。 有关详细信息，请参阅[将其他工作站附加到 MultiPoint 服务](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。  
  
-   **域**-在域环境中，Windows MultiPoint 服务器计算机已添加到域中，并且域用户已添加到 MultiPoint Services 主机操作系统上的本地管理员组中。  
  
## <a name="procedures"></a>过程  
使用下面的过程，以：  
  
-   [为虚拟机创建模板](#create-a-template-for-virtual-desktops)  
  
-   [从模板创建虚拟桌面](#create-virtual-machine-desktops-from-the-template)  
  
-   [复制现有虚拟桌面模板](#copy-an-existing-virtual-desktop-template)  
  
### <a name="create-a-template-for-virtual-desktops"></a>为虚拟机创建模板  
为虚拟机创建模板之前，必须先在 MultiPoint Server 中启用虚拟桌面功能。  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>启用虚拟桌面功能  
  
1.  使用本地管理员帐户登录到 MultiPoint Server 主机操作系统，或使用域帐户（该帐户是本地管理员组的成员）登录到 MultiPoint Server 主机操作系统。  
  
2.  从 "**开始**" 屏幕中，打开 "MultiPoint 管理器"。  
  
3.  单击 "**虚拟桌面**" 选项卡，单击 "**启用虚拟桌面**"，然后单击 **"确定"** ，并等待系统重新启动。  
  
下一步是创建虚拟桌面模板。 你实际上是创建一个虚拟硬盘（VHD）文件，你可以将其用作模板，以便为 MultiPoint 管理器创建工作站虚拟桌面。 你可以使用适用于 Windows 的物理安装媒体或。作为模板源的 ISO 映像文件。 你还可以使用。Windows 安装的 VHD。 请注意，若要使用物理安装光盘，必须先插入光盘，然后再启动向导。  
  
##### <a name="to-create-a-virtual-desktop-template"></a>创建虚拟桌面模板  
  
1.  使用本地管理员帐户登录到 MultiPoint Server 主机操作系统，或者使用域中的域帐户登录到本地 Administrators 组的成员。  
  
2.  从 "**开始**" 屏幕中，打开 "MultiPoint 管理器"。  
  
3.  单击 "**虚拟机**" 选项卡。  
  
4.   将 Windows 10 企业版 .iso 文件复制到本地 SSD。  
  
5.  在 "虚拟桌面" 选项卡上，单击 "**创建虚拟桌面模板"。**   
  
6.  在 "**前缀**" 中，输入用于标识模板的前缀以及使用该模板创建的虚拟机。 默认前缀为主机计算机名称。  
  
    前缀用于为模板和虚拟桌面工作站命名。 模板将 <*前缀*>-t。 虚拟桌面工作站的名称为 <*前缀*>-*n*，其中*n*是工作站标识符。  
  
7.  输入用于模板的本地管理员帐户的用户名和密码。 在域中，输入将添加到本地管理员组的域帐户的凭据。 此帐户可用于登录到模板以及从模板创建的所有虚拟桌面工作站。  
  
8. 单击 **"确定"** ，并等待模板创建完成。  
  
9. 新模板将在 "**虚拟机**" 选项卡上列出。模板将关闭。  
  
下一步是在虚拟机上用所需的软件和设置来配置模板。 在从模板创建任何虚拟桌面之前，必须执行此操作。  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>自定义虚拟桌面模板  
  
1.  使用本地管理员帐户或在域中使用本地管理员组中的域帐户登录到 MultiPoint server 主机操作系统。  
  
2.  从 "**开始**" 屏幕中，打开 "MultiPoint 管理器"。  
  
3.  单击 "**虚拟机**" 选项卡。  
  
4.  选择要自定义的模板，单击 "**自定义模板**"，然后单击 **"确定"** 。  
  
    > [!NOTE]  
    > 仅提供尚未用于创建虚拟桌面工作站的模板。 如果要更新已在使用的模板，则必须使用 "[复制现有虚拟桌面模板](#copy-an-existing-virtual-desktop-template)" 中所述的 "**导入模板**" 任务创建模板的副本。  
  
    该模板将在 Hyper-v **VM 连接**窗口中打开，并使用内置的管理员帐户执行自动登录。  
  
5.  此时，你可以安装应用程序和软件更新、更改设置，以及更新管理员配置文件。 对模板内置管理员配置文件所做的所有更改都将复制到从模板创建的虚拟桌面工作站中的默认用户配置文件。  
  
    如果要通过域连接工作站，则建议你在自定义期间创建一个本地用户帐户并将其添加到本地管理员组。  
  
    > [!NOTE]  
    > 如果在自定义模板时系统重启，则在系统重新启动后，使用内置管理员帐户自动登录可能会失败。 若要解决此问题，请使用创建的本地管理员帐户手动登录，更改内置管理员帐户的密码，注销，然后使用内置的管理员帐户和新密码重新登录。 （你将需要删除使用本地管理员帐户登录时创建的配置文件。）  
  
6.  完成配置系统后，双击管理员桌面上的**CompleteCustomization**快捷方式以运行 Sysprep，然后关闭模板。 在自定义期间，Sysprep 工具会删除所有唯一的系统信息，以便准备要映像的 Windows 安装。  
  
### <a name="create-virtual-machine-desktops-from-the-template"></a>从模板创建虚拟机桌面  
通过虚拟桌面模板的配置方式，你可以开始创建虚拟桌面。 将为附加到 MultiPoint 服务器计算机的每个工作站创建一个虚拟桌面。 用户下次登录到工作站时，将看到虚拟桌面，而不是以前显示的基于会话的桌面。  
  
> [!NOTE]  
> 此过程仅在 MultiPoint Server 处于*工作站模式*时有效。 如果系统处于*控制台模式*，则可以从 MultiPoint 管理器切换到工作站模式。 如果使用默认的 MultiPoint 设置，还可以通过重新启动计算机来启动工作站模式。 默认情况下，MultiPoint Server 计算机始终在工作站模式下启动  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>为工作站创建虚拟桌面  
  
1.  使用本地管理员帐户从远程工作站登录到 Windows MultiPoint server （例如，通过使用远程桌面连接从 Windows 计算机登录），或在域中，使用本地管理员组中的域帐户登录到 Windows MultiPoint server。  
  
    > [!NOTE]  
    > 或者，你可以使用本地工作站登录到服务器。 但是，当你创建工作站虚拟桌面时，你必须注销用于创建虚拟桌面的工作站，以便将另一工作站连接到新的虚拟机。  
  
2.  从 "**开始**" 屏幕中，打开 "MultiPoint 管理器"。  
  
3.  如果计算机处于控制台模式，请切换到工作站模式：  
  
    1.  在 "**主页**" 选项卡上，单击 "**切换到工作站模式**"。  
  
    2.  当计算机重新启动时，以管理员身份登录。  
  
4.  单击 "**虚拟机**" 选项卡。  
  
5.  选择要与工作站一起使用的虚拟桌面模板，单击 "**创建虚拟桌面工作站**"，然后单击 **"确定"** 。  
  
任务完成后，每个本地工作站都将连接到基于虚拟机的虚拟机。  
  
> [!NOTE]  
> 如果用户帐户登录到任何本地工作站，你将需要注销会话，以使工作站能够连接到新创建的工作站虚拟桌面。  
  
### <a name="copy-an-existing-virtual-desktop-template"></a>复制现有虚拟桌面模板  
使用以下过程创建可自定义和使用的现有虚拟桌面模板的副本。 这在下列情况下可能很有用：  
  
-   若要将主模板从网络共享复制到 MultiPoint Server 主机计算机上，以便可以从主模板创建虚拟桌面工作站。  
  
-   若要创建当前正在使用的模板的副本，以便您可以进行其他自定义。  
  
##### <a name="to-import-a-virtual-desktop-template"></a>导入虚拟桌面模板  
  
1.  以管理员身份登录到 MultiPoint 服务器。  
  
2.  从 "**开始**" 屏幕中，打开 "MultiPoint 管理器"。  
  
3.  单击 "**虚拟机**" 选项卡。  
  
4.  单击 "**导入虚拟桌面模板**"，并使用 "**浏览**" 选择要导入的 .vhd 文件（模板）。 导入模板时，将生成原始 .vhd 的副本。 默认情况下，MultiPoint 服务将 .vhd 文件存储在 C：\\Users\\公共\\文档\\\-\\\\中。  
  
5.  为新模板输入前缀，然后单击 **"确定"** 。  
  
6.  如果要对本地模板进行进一步的自定义，则可以通过在前缀末尾递增版本号来更改前缀名称。 或者，如果您要导入主模板，则可能需要将主模板的版本添加到默认前缀名称的末尾。  
  
7.  任务完成后，可以自定义模板，也可以使用它来创建工作站。  
