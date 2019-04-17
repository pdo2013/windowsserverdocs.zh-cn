---
title: "管理 Windows Server Essentials 中的数字媒体"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6906c1dafc6d4131e07c008b9db47ebebe9b7770
ms.sourcegitcommit: 5c8d8acc59d61756377c9c4e459a47a9ab555b39
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2018
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的数字媒体

>适用于：Windows Server 2012 R2 Essentials，Windows Server 2012 Essentials

以下主题讨论媒体流式传输的服务器，功能，并介绍了如何设置并使用你的网络上流式传输的媒体。  
  
> [!NOTE]
>  默认情况下，媒体流式传输不的功能适用于 Windows Server Essentials 和 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色。 若要添加的媒体流式传输功能在这些版本中，[下载并安装 Windows Server Essentials 媒体包](https://www.microsoft.com/download/details.aspx?id=40837)从 Microsoft 下载中心提供。  
  
-   [数字媒体概述](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [管理媒体服务器使用仪表板](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [媒体流式传输的工作原理](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [将媒体流式传输打开或关闭](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [添加到服务器的数字媒体文件](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [允许或限制向媒体库在服务器上的访问权限](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [重命名媒体库](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [停止共享其数字媒体](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [启用了服务器消息块 (SMB) 协议用于访问共享的文件服务器上的媒体设备](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [常见的处理器以及他们所支持的视频的配置文件](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [媒体文件类型的已知的问题](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>数字媒体概述  
 数字媒体指音频、视频和照片已编码（数字压缩）的内容。 编码内容包括音频和视频将输入转换为数字媒体文件，如 Windows Media 文件。 数字媒体编码后，它可以轻松地处理、进行分发，和播放的计算机，并通过计算机网络轻松传输它。  
  
 数字媒体类型的示例包括：Windows Media 音频 (WMA)、Windows Media 视频 (WMV)、MP3、JPEG 和 AVI。 有关 Windows Media Player 支持的数字媒体类型的信息，请参阅[文件类型的 Windows Media Player 支持](https://support.microsoft.com/kb/316992)。  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>为什么要将流式传输我数字媒体？  
 许多人将音乐、视频和照片存储在 Windows Server Essentials 中的共享文件夹。 可能有的有时您需要执行以下操作：  
  
-   **观看视频**。 服务器可以用于存储和流式传输较大视频集锦和录制的电视节目到你的计算机或其他播放设备在你的网络。 你可以通过使用 Windows Media Player 流式传输到 Xbox 360 或计算机的视频。  
  
-   **播放音乐**。 当你打开的共享媒体**音乐**共享的文件夹中，您可以从支持 Windows Media 连接的设备访问你的音乐。 你不需要启用或配置流式传输从任何用户帐户**音乐**共享的文件夹后共享处于打开状态。  
  
-   **显示照片幻灯片放映**。 你可以将存储在你数字照片**照片**共享你的服务器上的文件夹，并从任何计算机或已连接到你的家庭或 office 电视 Xbox 360 访问它们。 你可以观看照片幻灯片放映，这就好像在到大相框关闭你的电视节目。  
  
###  <a name="BKMK_1.5"></a>共享张受版权保护的媒体  
  Windows Server 软件包不支持共享张受版权保护的媒体。 这包括通过在线音乐应用商店购买的音乐。  
  
 仅在计算机或你用于购买它的设备上，可以重新播放 Copy-protected 媒体。 复制保护将阻止你播放媒体多台计算机或设备上，即使你将媒体复制到你的服务器，并从该处播放它。 但是，你可以在 Windows Server Essentials 上存储的版权保护的媒体，并继续在计算机或你用于购买它的设备上播放媒体。  
  
##  <a name="BKMK_2"></a>管理媒体服务器使用仪表板  
  Windows Server 软件包便可以通过使用 Windows Server Essentials 仪表板中执行常见的管理任务。 **媒体**选项卡上的服务器**设置**仪表板中的页面将提供以下：  
  
|部分|功能|  
|-------------|-------------------|  
|媒体服务器|**打开 / 关闭**按钮可将媒体流式传输打开或关闭。|  
|流式传输的视频质量|下拉箭头可选择流式传输从服务器播放的视频的质量的视频。|  
|媒体库|显示在媒体库的名称。 库的默认名称称为**数字媒体库**，这创建当打开将媒体流式传输。 若要更改的媒体库名称，请参阅[重命名媒体库](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)。 你可以单击**自定义**自定义的共享媒体库到的文件夹。|  
  
 有关详细信息，请参阅[允许限制对服务器上的媒体库的访问或](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)和[共享张受版权保护的媒体](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5)。  
  
##  <a name="BKMK_3"></a>媒体流式传输的工作原理  
 媒体流式传输 Windows Server Essentials 中的新功能可使其联网计算机可能和某些网络数字媒体设备播放服务器存储的数字媒体文件。  
  
 当打开 media 服务器时，在媒体库中共享的内容将适用于都能够接收来自你服务器的流媒体网络上的设备上玩游戏功能。 你可以流式传输数字媒体文件的大多数的类型。 一些常见的可以流式传输的文件类型：  
  
-   Windows Media 格式 (.asf，.wma、.wmv、.wm)  
  
-   音频 Visual 交错 (.avi)  
  
-   移动照片专家组 (.mpeg、.mpg、.mp3)  
  
-   对于 Windows (.wav) 音频  
  
-   CD 音频轨道 (.cda)  
  
 若要播放的文件，只需找到一首歌曲，视频或图片在共享文件夹中，双击该文件，并内容将将从服务器流式传输到你的计算机和播放。 有关如何查找并玩服务器存储数字媒体文件的信息，请参阅[播放数字媒体](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。  
  
 若要将媒体流式传输，你将需要以下硬件：  
  
-   有线或无线专用网络  
  
-   在你的网络或称为数字媒体接收器（有时称为联网数字媒体播放器）的设备上的任一另一台计算机。 数字媒体接收器是硬件设备连接到有线或无线网络，你可以通过使用你的电脑控制？即使你的计算机是在另一个房间中。  
  
 有关详细信息，请参阅[将媒体流式传输打开或关闭](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)。  
  
##  <a name="BKMK_4"></a>将媒体流式传输打开或关闭  
 Windows Media Center（包括 Xbox 360）和电子的其他个人设备，你可以共享音乐、视频和图片从流式传输文件由 Windows Server Essentials 向计算机、手机、电视、数字媒体接收器、扩展如任何受支持的数字媒体接收器 (DMR)。  
  
 Windows Server Essentials 与兼容的数字媒体设备的当前列表，请参阅[Windows 兼容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。  
  
### <a name="enabling-media-sharing"></a>启用共享媒体  
 若要共享存储在 Windows Server Essentials 媒体，你需要将媒体流式传输打开。 默认情况下，打开了媒体流式传输。  
  
####  <a name="BKMK_2.5"></a>若要将媒体流式传输打开或关闭  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击**设置**，单击**媒体**，然后执行下列情况之一：  
  
    -   单击**打开**好开始分享的所有文件存储在服务器媒体库中。  
  
    -   单击**关闭**要停止共享的所有文件存储在服务器媒体库中。  
  
3.  如果你希望共享媒体库中的其他文件夹，请单击**自定义**，然后选择**是**为你希望包含在媒体库中的每个共享的文件夹。  
  
4.  单击**确定**以保存所做的更改。  
  
 有关 Windows Media Player 支持的数字媒体类型的信息，请参阅[文件类型的 Windows Media Player 支持](https://support.microsoft.com/kb/316992)。  
  
 有关详细信息，请参阅[允许限制对服务器上的媒体库的访问或](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)。  
  
##  <a name="BKMK_5"></a>添加到服务器的数字媒体文件  
 通过访问直接，服务器或使用远程网站访问的站点若要登录到仪表板中，服务器管理员可以向媒体库中的共享文件夹添加数字媒体。 其他用户可以通过使用到服务器添加的媒体文件**共享文件夹**启动栏使用远程网站访问的网站，或通过使用我的服务器应用适用于 Windows Phone 上进行连接。 有关播放媒体的信息，请参阅[播放数字媒体](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。  
  
> [!NOTE]
>  您可以使用我的服务器应用适用于 Windows Phone 服务器上载媒体文件。 你可以下载我服务器应用从[Windows Phone 应用商店](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)。 有关适用于 Windows phone 的我服务器应用的详细信息，请参阅博客文章[我服务器的手机应用，Windows Server essentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx)。  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>若要添加的服务器上的共享文件夹数字媒体文件  
  
1.  使用以下方法之一登录到服务器：  
  
    1.  登录到远程 Web 访问有关的信息，请参阅[登录到远程访问 Web](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)。  
  
    2.  有关使用启动栏日志记录的信息，请参阅[启动栏概述](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)。  
  
2.  搜索，并单击你要添加的媒体的文件夹。  
  
3.  复制和粘贴或拖放所需的媒体文件添加到相应的共享文件夹服务器上。  
  
##  <a name="BKMK_6"></a>允许或限制向媒体库在服务器上的访问权限  
  
-   当你打开 media 共享时，它将创建四个预定义的文件夹：音乐、图片、视频和录制的电视节目。 这些文件夹的任何 preexists 在服务器上，如果重复作为的共享文件夹以用于共享媒体使用现有的文件夹。 将保留所有现有文件夹的媒体内容和用户权限，并共享所有网络用户使用。  
  
-   打开媒体库共享共享文件夹之前，你应了解媒体库共享绕过任何类型的用户帐户访问你的共享文件夹的设置。 例如，假设媒体的共享的库在打开**照片**共享文件夹，并设置**照片**到共享的文件夹**拒绝访问**为用户帐户名为胡继。 胡继仍可以流式传输从任何数字媒体**视频**到任何支持的数字媒体播放器或 DMR 共享的文件夹。 如果你不希望在这种方式流式传输的数字媒体，请将文件存储在未安装媒体库共享打开一个文件夹。  
  
-   如果你打开媒体库共享共享文件夹，所有受支持的数字媒体播放器或可以访问你的 Windows Server Essentials 网络的 DMR 还可以访问你的共享文件夹中的数字媒体。 例如，如果你有无线网络，并且你不安全它，你的无线网络的范围内的任何人都可能可以访问数字媒体该文件夹中。 打开媒体库共享之前，请确保你保护你的无线网络。 有关详细信息，请参阅有关你的无线接入点的文档。  
  
##  <a name="BKMK_8"></a>重命名媒体库  
 媒体库中的默认名称是**数字媒体服务器**。 当你打开流式传输在 Windows Server Essentials 中的媒体创建。 媒体流式传输的车的详细信息，请参阅[将媒体流式传输打开或关闭](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5)。 你可以通过使用服务器仪表板，可随时修改媒体库名称。  
  
#### <a name="to-rename-the-media-library"></a>若要重命名媒体库  
  
1.  打开服务器仪表板。 若要启动栏中打开仪表板，请单击**仪表板**、键入服务器密码，然后单击的箭头即可登录。  
  
2.  在服务器仪表板上，单击**设置**。  
  
3.  在**设置**页上，单击**媒体**选项卡。  
  
4.  在**媒体库**部分**媒体设置**页上，单击媒体库的名称。  
  
5.  在**更改媒体库名称**对话框中，键入媒体库的新名称，然后单击**确定**。  
  
##  <a name="BKMK_9"></a>停止共享其数字媒体  
 服务器管理员可以停止共享其存储在运行 Windows Server Essentials 服务器上的共享文件夹的数字媒体。  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>若要停止共享其共享文件夹中的媒体  
  
1.  打开服务器仪表板。  
  
2.  仪表板上**家庭版**页上，单击**设置**，单击**设置媒体服务器**，然后单击**单击媒体服务器设置**。  
  
3.  在**媒体**设置页上，你可以选择以下选项之一：  
  
    -   单击**关闭**要停止共享的所有文件存储在服务器上。  
  
    -   单击**自定义**，然后选择**否**你想要停止共享的特定文件夹。  
  
4.  单击**应用**或**确定**以保存所做的更改。  
  
##  <a name="BKMK_10"></a>启用了服务器消息块 (SMB) 协议用于访问共享的文件服务器上的媒体设备  
 使用网络文件和共享访问权限而不是 DLNA（适用于流式传输 media）服务器消息块 (SMB) 的设备需要来宾帐户处于激活状态。 这网络，若要查看共享的文件夹，无需身份验证的内容上允许任何设备或用户。  
  
> [!CAUTION]
>  来宾帐户启用时，任何人都可以在默认情况下访问服务器上的共享的资源。  
  
##  <a name="BKMK_CommonProcessors"></a>常见的处理器以及他们所支持的视频的配置文件  
 将媒体传输在 Windows Server Essentials 服务器中，你可以使用运行的 Windows 7 或 Windows 8 操作系统或其他网络的设备（如数字媒体播放器）或 Media Center 扩展器（例如 Xbox 360) 的计算机。 当你离开你的网络，请使用远程访问 Web Media Player 播放存储在服务器的文件。  
  
 你将需要 200 KBps 和 10 MBps 之间的数据传输速率。 你需要使用你的计算机和设备能够识别和播放媒体格式。 并非所有设备都支持的同一媒体格式，因此必须为你的计算机和设备玩你所拥有的媒体文件的方法。  
  
  Windows Server 软件包包含代码转换支持，以确定计算机或设备的功能，然后动态将不受支持的视频文件转换到受支持。 一般情况下，如果 Windows Media Player 12 可以运行的 Windows 7 或 Windows 8 计算机上播放的内容，将网络连接的设备上播放内容服务器上。  
  
 代码转换为选择的格式和高比特率属于强烈 server 处理器的性能。 处理器性能被标识为 Windows 体验指数的一部分。 若要确定性能分数的服务器，请执行以下任一操作：  
  
-   网络在计算机上运行的 Windows 7 或 Windows 8 具有处理器相同服务器，请转到**“控制面板”**，单击**性能信息和工具**，然后查看上的信息**率和提高计算机的性能**页面。  
  
-   请联系制造商的处理器。  
  
 为了获得最佳的用户体验，选择视频流式传输适用于你的服务器处理器的分辨率质量。 服务器将自动调整比特率对某个受这些设置：  
  
-   **低**小于 3.6 处理器分数是否。  
  
-   **中等**是否大于 3.6 和小于 4.2 处理器分数。  
  
-   **高**是否大于 4.2 和小于 6.0 处理器分数。  
  
-   **最佳**是否大于 6.0 处理器分数。  
  
 如果你选择的视频流需要更多处理电量高于服务器具有的分辨率，你可能会遇到的缓冲区，以及从服务器媒体流式传输时停止。  
  
> [!NOTE]
>  若要将流式传输高清视频通过远程 Web 访问，你需要分数为至少 6.0 的处理器。  
  
##  <a name="BKMK_KnownIssues"></a>媒体文件类型的已知的问题  
 在远程访问 Web 媒体流式传输功能使用 Windows Media Player 12 网络共享服务。 远程访问 Web 媒体流式传输支持音频、视频和照片文件类型受支持的 Windows Media Player 12 和 Silverlight 4。  
  
 下表列出了受远程访问 Web 媒体流式传输的文件类型（格式）。 如果有多媒体文件类型服务器在表格中未包含，无法将它们流式传输通过远程访问 Web 媒体流式传输。  
  
|文件类型|文件扩展名|  
|---------------|-------------------------|  
|但是，3GPP 文件|.3gp、.3gpp、.3g2 和.3gp2|  
|音频数据传输流 (ADTS) 音频文件|.adts 和.adt|  
|AU 音频文件|.au 和.snd|  
|音频交换文件格式 (AIFF) 音频文件|.aif、.aifc 和.aiff|  
|AVCHD 文件|.m2ts、.m2t 和.mts|  
|CD 音频磁盘|.cda|  
|DVD 视频磁盘|.vob|  
|JPEG 图片文件|.jpg|  
|MP3 音频文件|.mp3 和.m3u|  
|MPEG 视频文件|.mpeg、.mpg、.m1v、.mpa、.mpe、.mp4、.mp4v、.m4v、.m4a 和.mov|  
|Windows 音频和视频文件|.avi 和.wav|  
|Windows Media 音频和视频文件|.asf、.asx、.wax、.wm、.wma、.wmd、.wmp、.wmv、.wmx、.wpl 和.wvx|  
  
> [!NOTE]
>  如果你无法使用此表所示的文件类型，该文件可能还进行编码的编解码器的 Windows Media Player 不支持。  
  
 有关支持的文件格式的其他信息，请参阅[文件类型的 Windows Media Player 支持](https://go.microsoft.com/fwlink/p/?LinkID=196118)和[支持媒体格式、协议和日志字段](https://go.microsoft.com/fwlink/p/?LinkId=203339)为 Silverlight。  
  
## <a name="see-also"></a>请参阅  
  
-   [播放数字媒体](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
