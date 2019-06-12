---
title: 管理 Windows Server Essentials 中的数字媒体
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: 87ec5218455672cbfd2bc1d77244fd263b91362c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433301"
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的数字媒体

>适用于：Windows Server 2012 R2 Essentials、 Windows Server 2012 Essentials

以下主题讨论了服务器的媒体流功能，还介绍了如何设置和使用网络上的媒体流。  
  
> [!NOTE]
>  默认情况下，媒体流式处理功能将不可用在 Windows Server Essentials 和 Windows Server 2012 R2 中安装了 Windows Server Essentials 体验角色。 若要在这些版本中添加媒体流功能，从 Microsoft 下载中心 [下载并安装 Windows Server Essentials 媒体包](https://www.microsoft.com/download/details.aspx?id=40837) 。  
  
-   [数字媒体概述](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [管理媒体服务器使用仪表板](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [媒体流式处理的工作原理](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [打开或关闭媒体流](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [将数字媒体文件添加到服务器](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [允许或限制对服务器上的媒体库的访问](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [重命名媒体库](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [停止共享数字媒体](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [启用使用服务器消息块 (SMB) 协议来访问在服务器上的共享的文件的媒体设备](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [常用的处理器和它们支持的视频配置文件](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [媒体文件类型的已知的问题](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a> 数字媒体概述  
 数字媒体是指已编码（数字压缩）的音频、视频和照片内容。 编码内容包括将音频和视频输入转换为数字媒体文件，如 Windows Media 文件。 对数字媒体进行编码后，它可以通过计算机方便地操作、分发和播放，并且轻松地通过计算机网络传输。  
  
 数字媒体类型的示例包括：Windows Media 音频 (WMA)、Windows Media 视频 (WMV)、MP3、JPEG　和 AVI。 有关 Windows Media Player 支持的数字媒体类型的信息，请参阅 [Windows Media Player 支持的文件类型](https://support.microsoft.com/kb/316992)。  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>为何要流式传输数字媒体？  
 许多人将音乐、 视频和图片存储在 Windows Server Essentials 中的共享文件夹中。 可能有些时候你想要执行以下操作：  
  
-   **观看视频**。 你的服务器可用于存储视频的大集合和录制的电视节目以及将它们流式传输到你的计算机或网络上的其他播放设备。 通过使用 Windows Media Player，可以将视频流媒体传送到 Xbox 360 或计算机。  
  
-   **播放音乐**。 当你打开 **“音乐”** 共享文件夹的媒体共享时，可以从支持 Windows Media Connect 的设备访问你的音乐。 打开共享后，不需要从 **“音乐”** 共享文件夹中启用或配置任何用户帐户以进行流式传输。  
  
-   **显示照片幻灯片**。 可以将你的数字照片存储在服务器上的 **“照片”** 共享文件夹中，然后从任何计算机或连接到你家庭或办公室中的电视的 Xbox 360 访问它们。 可以观看照片幻灯片，就像将你的电视变成大相框一样。  
  
###  <a name="BKMK_1.5"></a> 共享受版权保护的媒体  
  Windows Server Essentials 不支持共享受版权保护的媒体。 这包括通过在线音乐商店购买的音乐。  
  
 仅可以在用于购买它的计算机或设备上播放受版权保护的媒体。 版权保护防止你在多个计算机或设备上播放媒体，即使你将媒体复制到服务器并从那里播放它。 但是，可以在 Windows Server Essentials 上存储受版权保护的媒体，并继续在用来购买它的设备的计算机上播放该媒体。  
  
##  <a name="BKMK_2"></a> 管理媒体服务器使用仪表板  
  Windows Server Essentials，可以通过使用 Windows Server Essentials 仪表板执行常见管理任务。 服务器的 **“媒体”** 选项卡和仪表板的 **“设置”** 页提供了以下内容：  
  
|部分|功能|  
|-------------|-------------------|  
|媒体服务器|**“打开或关闭”** 按钮使你能够打开或关闭媒体流。|  
|视频流质量|下拉箭头使你能够选择从服务器播放的视频的视频流质量。|  
|媒体库|显示媒体库的名称。 此库的默认名称为 **“数字媒体库”** ，它在你打开媒体流时创建。 若要更改媒体库名称，请参阅[重命名媒体库](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)。 你可以单击 **“自定义”** 自定义共享到媒体库中的文件夹。|  
  
 有关详细信息，请参阅 [Allow or restrict access to a media library on the server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) 和 [Sharing copy-protected media](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5)。  
  
##  <a name="BKMK_3"></a> 媒体流式处理的工作原理  
 媒体流式处理 Windows Server Essentials 中的新功能使联网的计算机和某些联网数字媒体设备可以播放存储在服务器的数字媒体文件。  
  
 当你打开媒体服务器时，在能够从你的服务器中接收流媒体的网络中的设备上，媒体库中共享的内容将可用于播放。 可以传输大多数类型的数字媒体文件。 一些较常见的可以流媒体传送的文件类型包括：  
  
- Windows 媒体格式（.asf、.wma、.wmv、.wm）  
  
- 音频 Visual 交错 (.avi)  
  
- 移动图片专家组（.mpeg、.mpg、.mp3）  
  
- 适用于 Windows (.wav) 的音频  
  
- CD 音频曲目 (.cda)  
  
  若要播放文件，只需在共享文件夹中找到一首歌曲、视频或图片，双击该文件，然后将内容从服务器传输到你的计算机并且播放。 有关如何查找和播放存储在服务器的数字媒体文件的信息，请参阅[播放数字媒体](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。  
  
  若要流式传输媒体，你需要以下硬件：  
  
- 有线或无线专用网络  
  
- 你的网络上的另一台计算机或称为数字媒体接收器（有时称为网络数字媒体播放机）的设备。 数字媒体接收器是连接到有线或无线网络，你可以通过使用您的计算机控制的硬件设备吗？ 即使您的计算机是在另一个房间。  
  
  有关详细信息，请参阅[媒体流打开或关闭](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)。  
  
##  <a name="BKMK_4"></a> 打开或关闭媒体流  
 可以通过将 Windows Media center （包括 Xbox 到任何支持的数字媒体接收器 (DMR)，如计算机、 移动电话、 电视、 数字媒体接收器、 扩展程序的文件流共享音乐、 视频和图片从 Windows Server Essentials360) 和其他个人电子设备。  
  
 与 Windows Server Essentials 兼容的数字媒体设备的当前列表，请参阅[Windows 兼容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。  
  
### <a name="enabling-media-sharing"></a>启用媒体共享  
 若要共享存储在 Windows Server Essentials 中的媒体，你需要打开媒体流。 默认情况下，媒体流已关闭。  
  
####  <a name="BKMK_2.5"></a> 若要打开或关闭媒体流  
  
1. 打开 Windows Server Essentials 仪表板。  
  
2. 依次单击 **“设置”** 和 **“媒体”** ，然后执行以下任一操作：  
  
   -   单击 **“打开”** 以开始共享所有存储在服务器上媒体库中的文件。  
  
   -   单击 **“关闭”** 以停止共享所有存储在服务器上媒体库中的文件。  
  
3. 如果你希望共享媒体库中的其他文件夹，则请单击 **“自定义”** ，然后为每个你想要包含在媒体库中的共享文件夹选择 **“是”** 。  
  
4. 单击 **“确定”** 以保存你的更改。  
  
   有关 Windows Media Player 支持的数字媒体类型的信息，请参阅 [Windows Media Player 支持的文件类型](https://support.microsoft.com/kb/316992)。  
  
   有关详细信息，请参阅[允许或限制对服务器上的媒体库的访问](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)。  
  
##  <a name="BKMK_5"></a> 将数字媒体文件添加到服务器  
 通过直接，访问服务器或使用远程 Web 访问站点登录到仪表板，服务器管理员可以将数字媒体添加到媒体库中的共享文件夹。 其他用户可以通过向服务器添加媒体文件**共享文件夹**快速启动板上，通过使用远程 Web 访问站点，或通过用于 Windows Phone 的 My Server 应用的连接。 有关播放媒体的信息，请参阅[播放数字媒体](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。  
  
> [!NOTE]
>  通过使用适用于 Windows Phone 的 My Server 应用，还可以将媒体文件上载到服务器。 你可以从 [Windows Phone 应用商店](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)下载 My Server 应用。 有关适用于 Windows phone 的 My Server 应用的详细信息，请参阅博客文章[适用于 Windows Server Essentials 的 My Server 手机应用](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx)。  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>将数字媒体文件添加到服务器上的共享文件夹  
  
1.  使用以下方法之一登录到服务器：  
  
    1.  有关登录到远程 Web 访问的信息，请参阅[登录到远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)。  
  
    2.  有关使用快速启动板登录的信息，请参阅[快速启动板概述](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)。  
  
2.  搜索并单击要添加的媒体的文件夹。  
  
3.  将你想要添加的媒体文件复制和粘贴或拖放到服务器上相应的共享文件夹中。  
  
##  <a name="BKMK_6"></a> 允许或限制对服务器上的媒体库的访问  
  
-   当你打开媒体共享时，它会创建四个预定义的文件夹：音乐、图片、视频和录制的电视节目。 如果在服务器上预先存在任何这些文件夹，则现有文件夹将作为媒体共享的共享文件夹重复使用。 会保留所有现有文件夹的媒体内容和用户权限，并与所有网络用户共享。  
  
-   在启用共享文件夹的媒体库共享之前，你应该了解媒体库共享绕过任何类型的为共享文件夹设置的用户帐户访问。 例如，假设你为 **“照片”** 共享文件夹启用媒体库共享，并为名为 Bobby 的用户帐户将 **“照片”** 共享文件夹设置为 **“拒绝访问”** 。 Bobby 仍可以从 **“视频”** 共享文件夹将任何数字媒体流式传输到任何支持的数字媒体播放器或 DMR。 如果你有不希望以这种方式流媒体传送的数字媒体，则将文件存储在未启用媒体库共享的文件夹中。  
  
-   如果你启用媒体库共享的共享文件夹，则任何支持的数字媒体播放器或可以访问你的 Windows Server Essentials 网络的 DMR 也可以访问该共享文件夹中的数字媒体。 例如，如果你有无线网络而且你尚未保护它，则你的无线网络范围内的任何人都可能会访问该文件夹中的数字媒体。 在你启用媒体库共享之前，请确保你的无线网络的安全。 有关详细信息，请参阅你的无线访问点的文档。  
  
##  <a name="BKMK_8"></a> 重命名媒体库  
 媒体库的默认名称是 **“数字媒体服务器”** 。 Windows Server Essentials 中的媒体流打开时创建它。 有关启用媒体流的详细信息，请参阅[若要打开或关闭媒体流](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5)。 通过使用服务器仪表板，可以随时修改媒体库的名称。  
  
#### <a name="to-rename-the-media-library"></a>重命名媒体库  
  
1.  打开服务器仪表板。 若要从快速启动板中打开仪表板，请单击 **“仪表板”** ，键入服务器密码，然后单击箭头以登录。  
  
2.  在服务器的仪表板上，单击 **“设置”** 。  
  
3.  在 **“设置”** 页面上，单击 **“媒体”** 选项卡。  
  
4.  在 **“媒体设置”** 页面上的 **“媒体库”** 一节中，单击你的媒体库的名称。  
  
5.  在 **“更改媒体库名称”** 对话框中，键入媒体库的新名称，然后单击 **“确定”** 。  
  
##  <a name="BKMK_9"></a> 停止共享数字媒体  
 服务器管理员可以停止共享存储在运行 Windows Server Essentials 的服务器上的共享文件夹中的数字媒体。  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>停止共享文件夹中的共享媒体  
  
1.  打开服务器仪表板。  
  
2.  在仪表板的 **“主页”** 页上，依次单击 **“设置”** 、 **“设置媒体服务器”** 和 **“单击以设置媒体服务器”** 。  
  
3.  在 **“媒体”** 设置页上，你可以选择以下选项之一：  
  
    -   单击 **“关闭”** 以停止共享所有存储在服务器上的文件。  
  
    -   单击 **“自定义”** ，然后为你要停止共享的特定文件夹选择 **“否”** 。  
  
4.  单击 **“应用”** 或 **“确定”** 以保存你的更改。  
  
##  <a name="BKMK_10"></a> 启用使用服务器消息块 (SMB) 协议来访问在服务器上的共享的文件的媒体设备  
 将服务器消息块 (SMB) 用于网络文件和共享访问而不是使用 DLNA （适用于媒体流）的设备需要激活来宾帐户。 这允许网络上的任何设备或用户无需身份验证即可查看共享文件夹的内容。  
  
> [!CAUTION]
>  默认情况下，启用来宾帐户后，任何人都可以访问服务器上的共享资源。  
  
##  <a name="BKMK_CommonProcessors"></a> 常用的处理器和它们支持的视频配置文件  
 流传输媒体，从 Windows Server Essentials 服务器，可以使用运行 Windows 7 或 Windows 8 操作系统或其他联网的设备 （如数字媒体播放器） 或 Media Center 扩展器 （如 Xbox 360) 的计算机。 当你离开网络时，使用远程 Web 访问 Media Player 播放存储在你服务器上的文件。  
  
 你需要介于 200 KBps 和 10 MBps 之间的数据传输速率。 你需要使用你的计算机和设备可以识别和播放的媒体格式。 并非所有的设备支持相同的媒体格式，因此必须有为你的计算机和设备播放你具有的媒体文件的方法。  
  
  Windows Server Essentials 包含代码转换支持，确定功能的计算机或设备，然后动态不受支持的视频文件将转换为一个受支持。 一般情况下，如果 Windows Media Player 12 可以播放运行 Windows 7 或 Windows 8 的计算机上的内容，则服务器上的内容将在已连接网络的设备上播放。  
  
 为转码选择的格式和比特率很大程度上取决于服务器处理器的性能。 处理器性能被标识为 Windows 体验指数的一部分。 若要确定你的服务器的性能分数，请执行下列任一操作：  
  
- 在运行 Windows 7 或 Windows 8、 具有与你的服务器相同处理器的网络计算机，请转到**Control Panel**，单击**性能信息和工具**，然后上查看信息**速率和提高计算机性能的**页。  
  
- 与处理器的制造商联系。  
  
  为了获得最佳用户体验，选择适用于你的服务器处理器的视频流分辨率质量。 服务器会将比特率自动调整为下列设置之一：  
  
- 如果处理器分数低于 3.6，则为**低** 。  
  
- 如果处理器分数高于 3.6 并低于 4.2，则为**中等** 。  
  
- 如果处理器分数高于 4.2 并低于 6.0，则为**高** 。  
  
- 如果处理器分数高于 6.0，则为**最佳** 。  
  
  如果你选择需要具有比你的服务器更高处理能力的视频流分辨率，则当从该服务器流传输媒体时，你可能会遇到缓冲区并且停止。  
  
> [!NOTE]
>  若要通过远程 Web 访问流传输高清晰度视频，你需要有至少 6.0 分的处理器。  
  
##  <a name="BKMK_KnownIssues"></a> 媒体文件类型的已知的问题  
 远程 Web 访问中的媒体流功能使用 Windows Media Player 12 网络共享服务。 远程 Web 访问媒体流支持音频、视频和图片文件类型，这些文件类型在 Windows Media Player 12 和 Silverlight 4 中受支持。  
  
 下表列出了远程 Web 访问媒体流支持的文件类型（格式）。 如果你的服务器上有该表中不包括的媒体文件类型，则你无法通过远程 Web 访问媒体流来流媒体传送它们。  
  
|文件类型|文件扩展名|  
|---------------|-------------------------|  
|3GPP 文件|.3gp、.3gpp、.3g2 和 .3gp2|  
|音频数据传输流 (ADTS) 音频文件|.adts 和 .adt|  
|AU 音频文件|.au 和 .snd|  
|音频交换文件格式 (AIFF) 音频文件|.aif、.aifc 和 .aiff|  
|AVCHD 文件|.m2ts、.m2t 和 .mts|  
|CD 音频磁盘|.cda|  
|DVD 视频磁盘|.vob|  
|JPEG 图片文件|.jpg|  
|MP3 音频文件|.mp3 和 .m3u|  
|MPEG 视频文件|.mpeg、.mpg、.m1v、.mpa、.mpe、.mp4、.mp4v、.m4v、.m4a 和 .mov|  
|Windows 音频和视频文件|.avi 和 .wav|  
|Windows Media 音频和视频文件|.asf、.asx、.wax、.wm、.wma、.wmd、.wmp、.wmv、.wmx、.wpl 和 .wvx|  
  
> [!NOTE]
>  如果你不能使用此表中列出的文件类型，则文件还可能使用了不受 Windows Media Player 支持的编解码器进行了编码。  
  
 有关受支持的文件格式的其他信息，请参阅 [Windows Media Player 支持的文件类型](https://go.microsoft.com/fwlink/p/?LinkID=196118) 和适用于 Silverlight 的 [受支持的媒体格式、协议和日志字段](https://go.microsoft.com/fwlink/p/?LinkId=203339) 。  
  
## <a name="see-also"></a>请参阅  
  
-   [播放数字媒体](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
