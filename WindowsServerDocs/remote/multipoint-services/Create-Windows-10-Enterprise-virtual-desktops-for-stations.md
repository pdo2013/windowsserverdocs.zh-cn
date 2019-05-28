---
title: 为工作站创建 Windows 10 企业版虚拟桌面
description: 了解如何创建 Windows Server 2016 桌面工作站
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: 0aa81ef3633adf27a25b45b3b7c00082d83bf0bb
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034623"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>为工作站创建 Windows 10 企业版虚拟桌面
MultiPoint Services 中此可选配置主要适用于其中的基本应用程序需要它自己的客户端操作系统的实例为每个用户的情况。 示例包括不能在 Windows Server 上安装的应用程序和应用程序将在同一台主机计算机上运行多个实例。  
  
> [!NOTE]  
> 这些虚拟桌面，也称为 VDI，是比默认 MultiPoint 服务桌面会话的更多占用大量资源，因此，我们建议使用默认 MultiPoint 服务会话在可能的情况。  
  
## <a name="prerequisites"></a>先决条件  
若要准备将创建虚拟桌面工作站时，请确保你的 MultiPoint 服务系统满足以下要求：      
  
|硬件|要求|         |
|------------|----------------|----------------| 
|CPU （多媒体）|1 个核心或每个虚拟机的线程|  
|固态硬盘 (SSD)|容量 > = 20 GB，每个工作站 + 40 GB 的 MultiPoint 服务承载操作系统<br /><br />随机读取\/写入 IOPS > = 3 K 每工作站|  
|RAM|每个工作站的 2GB + Windows MultiPoint Server 主机操作系统的 2 GB|  
|显卡|DX11|  
|BIOS|配置为启用虚拟化 – 第二个二级地址转换 (SLAT) 的 BIOS CPU 设置|  
  
-   **工作站**-设置你的 MultiPoint 服务系统的工作站。 有关详细信息，请参阅[附加其他工作站到 MultiPoint 服务](Attach-additional-stations-to-your-MultiPoint-services-computer.md)。  
  
-   **域**-在域环境中，Windows MultiPoint Server 计算机是否已添加到域，并且域用户具有已添加到 MultiPoint 服务主机操作系统上的本地管理员组。  
  
## <a name="procedures"></a>过程  
使用下面的过程，以：  
  
-   [创建虚拟桌面模板](#create-a-template-for-virtual-desktops)  
  
-   [从模板创建虚拟桌面](#create-virtual-machine-desktops-from-the-template)  
  
-   [复制现有的虚拟桌面模板](#copy-an-existing-virtual-desktop-template)  
  
### <a name="create-a-template-for-virtual-desktops"></a>创建虚拟桌面模板  
可以创建你的虚拟桌面模板之前，必须启用 MultiPoint Server 中的虚拟桌面功能。  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>若要启用虚拟桌面功能  
  
1.  登录到 MultiPoint Server 主机操作系统使用本地管理员帐户，或在域中，使用域帐户是本地 Administrators 组的成员。  
  
2.  从**启动**屏幕上，打开 MultiPoint 管理器。  
  
3.  单击**虚拟桌面**选项卡上，单击**启用虚拟桌面**，然后单击**确定**，并等待系统重新启动。  
  
下一步是创建虚拟桌面模板。 按字面意思创建可以用作模板来创建工作站的 MultiPoint 管理器的虚拟桌面的虚拟硬盘 (VHD) 文件。 可以为 Windows 使用物理安装媒体或。ISO 映像到作为源的模板文件。 此外可以使用。Windows 安装的 VHD。 请注意，若要使用的物理安装光盘，您必须插入光盘之前启动向导。  
  
##### <a name="to-create-a-virtual-desktop-template"></a>若要创建虚拟桌面模板  
  
1.  登录到 MultiPoint Server 主机操作系统使用本地管理员帐户，或在域中，是本地 Administrators 组的成员的域帐户。  
  
2.  从**启动**屏幕上，打开 MultiPoint 管理器。  
  
3.  单击**虚拟桌面**选项卡。  
  
4.   将 Windows 10 企业版.iso 文件复制到本地 SSD。  
  
5.  在虚拟桌面选项卡上单击**创建虚拟桌面模板。**   
  
6.  在中**前缀**，输入要用于标识该模板并使用模板创建虚拟桌面的前缀。 默认的前缀是主机计算机名称。  
  
    前缀用于为模板和虚拟桌面工作站命名。 该模板将在 <*前缀*>-t。 将虚拟桌面工作站被命名为 <*前缀*>-*n*，其中*n*是工作站标识符。  
  
7.  输入用户名和密码用于进行模板的本地管理员帐户。 在域中，输入将添加到本地 Administrators 组的域帐户的凭据。 可以使用此帐户登录到该模板并从模板创建的所有虚拟桌面工作站。  
  
8. 单击**确定**，并等待模板创建完成。  
  
9. 上会列出新的模板**虚拟桌面**选项卡。该模板将关闭状态。  
  
下一步是使用该软件和虚拟机的所需的设置配置的模板。 从模板创建任何虚拟桌面之前必须执行此操作。  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>若要自定义虚拟桌面模板  
  
1.  登录到 MultiPoint server 主机操作系统使用本地管理员帐户，或在具有本地管理员组中的域帐户的域中。  
  
2.  从**启动**屏幕上，打开 MultiPoint 管理器。  
  
3.  单击**虚拟桌面**选项卡。  
  
4.  选择你想要自定义，请单击模板**自定义模板**，然后单击**确定**。  
  
    > [!NOTE]  
    > 可以使用仅具有尚未用于创建虚拟桌面工作站模板。 如果你想要更新已在使用模板，您必须通过使用进行模板的副本**导入模板**更高版本，介绍的任务中[复制现有的虚拟桌面模板](#copy-an-existing-virtual-desktop-template)。  
  
    该模板将在 HYPER-V 中打开**VM 连接**使用内置管理员帐户执行窗口中，并自动登录。  
  
5.  此时可以安装应用程序和软件更新、 更改设置，并更新管理员配置文件。 对模板的内置管理员配置文件所做的所有更改将都复制到默认用户配置文件中从模板创建虚拟桌面工作站。  
  
    如果要通过域连接你的工作站，我们建议你创建本地用户帐户和自定义期间将其添加到本地管理员组。  
  
    > [!NOTE]  
    > 如果要自定义模板时，重新启动系统，系统重新启动后，使用内置管理员帐户自动登录可能会失败。 来避开此问题，请手动日志上使用你创建的本地管理员帐户更改内置 Administrator 帐户的密码，先注销，然后再次登录，使用内置管理员帐户和新密码。 （您需要删除您使用本地管理员帐户登录时创建的配置文件。）  
  
6.  完成配置您的系统后，双击**CompleteCustomization**上运行 Sysprep，然后关闭该模板管理员的桌面快捷方式。 自定义期间，Sysprep 工具中删除所有唯一的系统信息来准备要映像的 Windows 安装。  
  
### <a name="create-virtual-machine-desktops-from-the-template"></a>从模板创建虚拟机桌面  
使用的虚拟桌面模板配置所需桌面是，您就可以开始创建虚拟桌面的方式。 将创建连接到 MultiPoint Server 计算机的每个工作站的虚拟桌面。 下次用户登录到工作站，他们将看到虚拟桌面而不是基于会话的桌面之前显示。  
  
> [!NOTE]  
> 此过程只适用于 MultiPoint Server 是在*工作站模式下*。 如果系统处于*控制台模式*，您可以从 MultiPoint 管理器切换到工作站模式。 如果使用默认 MultiPoint 设置，您还可以通过重新启动计算机启动工作站模式。 默认情况下，MultiPoint Server 计算机始终在工作站模式下启动  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>若要创建为你的工作站的虚拟桌面  
  
1.  登录到远程工作站 （例如，从通过使用远程桌面连接的 Windows 计算机） 从 Windows MultiPoint server 使用的本地管理员帐户，或者，在域中，域帐户的本地管理员组中。  
  
    > [!NOTE]  
    > 或者，你可以登录到使用本地工作站的服务器。 但是，在创建虚拟桌面工作站时，你需要注销你用于创建虚拟桌面以便将其他工作站连接到新的虚拟桌面工作站。  
  
2.  从**启动**屏幕上，打开 MultiPoint 管理器。  
  
3.  如果计算机是在控制台模式下，切换到工作站模式下：  
  
    1.  上**主页**选项卡上，单击**切换到工作站模式**。  
  
    2.  在计算机重新启动时，请以管理员身份登录。  
  
4.  单击**虚拟桌面**选项卡。  
  
5.  选择你想要使用的工作站，请单击虚拟桌面模板**创建虚拟桌面工作站**，然后单击**确定**。  
  
完成任务后，每个本地工作站将连接到基于虚拟机的虚拟桌面。  
  
> [!NOTE]  
> 如果用户帐户登录到任意本地工作站，将需要注销会话后，若要获取连接到一个新创建的工作站虚拟桌面工作站。  
  
### <a name="copy-an-existing-virtual-desktop-template"></a>复制现有的虚拟桌面模板  
使用以下过程创建一个现有的虚拟桌面模板，可以自定义和使用的副本。 这可以是以下情况下有用：  
  
-   若要从网络共享到 MultiPoint Server 主机计算机上复制的主模板，以便可以从主模板创建虚拟桌面工作站。  
  
-   若要创建正在使用中，以便可以进行其他自定义模板的副本。  
  
##### <a name="to-import-a-virtual-desktop-template"></a>导入虚拟桌面模板  
  
1.  以管理员身份登录到 MultiPoint server。  
  
2.  从**启动**屏幕上，打开 MultiPoint 管理器。  
  
3.  单击**虚拟桌面**选项卡。  
  
4.  单击**导入虚拟桌面模板**，并使用**浏览**以选择你想要导入的.vhd 文件 （模板）。 导入模板，将原始.vhd 的生成副本。 默认情况下，MultiPoint 服务将.vhd 文件存储在 c:\\用户\\公共\\文档\\超\-V\\虚拟硬盘\\文件夹。  
  
5.  对于新的模板中，输入一个前缀，然后单击**确定**。  
  
6.  如果您要做进一步的本地模板的自定义设置，可能通过递增版本号末尾的前缀来更改的前缀名称。 或者，如果要导入的主模板，您可能想要将版本的主模板添加到默认的前缀名称末尾。  
  
7.  完成任务后，可以自定义模板，或使用它，因为它是要创建工作站。  
