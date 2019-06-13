---
title: 使用 My Server 应用连接到 Windows Server Essentials
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e40b57f-6917-43ef-92e0-030baa9d2b99
9author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3c7c5114bbd3aa479e2b6afe807fee2ec8d3d5bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435883"
---
# <a name="use-the-my-server-app-to-connect-to-windows-server-essentials"></a>使用 My Server 应用连接到 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

适用于 Windows Server Essentials 的 My Server 应用可以连接到资源，并从笔记本电脑、 PC 或 Surface 设备在 Windows Server Essentials 服务器上执行少量的管理任务。 在 My Server 中，你可以管理用户、设备和警报，并使用服务器上的共享文件。 脱机时，你可以继续处理你最近在 My Server 中访问的文件，并且在下次连接时，你的脱机更改将自动与服务器同步。  
  
 如果你的服务器正在运行 Windows Server Essentials 操作系统，则使用原始的 My Server 应用。 如果你的服务器正在运行 Windows Server Essentials 操作系统，则必须使用更新的 My Server 2012 R2 应用。 这两个应用都可以从 [Windows 应用](https://windows.microsoft.com/windows-8/apps)下载。  
  
> [!TIP]
> 
>  若要从 Windows Phone 访问 Windows Server Essentials 中的资源，请使用 My Server 手机应用 （适用于 Windows Server Essentials) 或 My Server 2012 R2 手机应用 （适用于 Windows Server Essentials)。 有关 My Server 手机应用的信息，请参阅[为 Windows Phone 使用 My Server 应用](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)。 有关 My Server 2012 R2 手机应用的信息，请参阅博客文章 [My Server 2012 R2 Windows 和 Windows Phone 应用](http://blogs.technet.com/b/sbs/archive/2013/11/19/my-server-2012-r2-windows-and-windows-phone-apps.aspx)。  
  
## <a name="in-this-topic"></a>本主题内容  
  
-   [什么是 My Server 2012 R2 中的新增功能](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_WhatsNew)  
  
-   [操作系统要求](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_OS)  
  
-   [安装 My Server](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Install)  
  
-   [使用我的服务器](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_UseServer)  
  
-   [My Server 的功能](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Features)  
  
-   [如何从本地网络连接到你的服务器](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_ConnectServer)  

>  若要从 Windows Phone 访问 Windows Server Essentials 中的资源，请使用 My Server 手机应用 （适用于 Windows Server Essentials) 或 My Server 2012 R2 手机应用 （适用于 Windows Server Essentials)。 有关 My Server 手机应用的信息，请参阅[为 Windows Phone 使用 My Server 应用](../use/Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)。 有关 My Server 2012 R2 手机应用的信息，请参阅博客文章 [My Server 2012 R2 Windows 和 Windows Phone 应用](http://blogs.technet.com/b/sbs/archive/2013/11/19/my-server-2012-r2-windows-and-windows-phone-apps.aspx)。  
  
## <a name="in-this-topic"></a>本主题内容  
  
-   [什么是 My Server 2012 R2 中的新增功能](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_WhatsNew)  
  
-   [操作系统要求](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_OS)  
  
-   [安装 My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Install)  
  
-   [使用我的服务器](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_UseServer)  
  
-   [My Server 的功能](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_Features)  
  
-   [如何从本地网络连接到你的服务器](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md#BKMK_ConnectServer)  

  
##  <a name="BKMK_WhatsNew"></a> 什么是 My Server 2012 R2 中的新增功能  
 除了 My Server 中提供的功能，My Server 2012 R2 与 Windows Server Essentials 的客户提供以下新功能：  
  
-   **访问 SharePoint Online 团队网站和库**-利用 Microsoft Office 365 集成与 Windows Server Essentials 访问 SharePoint Online 团队网站并打开 My Server 2012 R2 中 SharePoint Online 库中的文档。  
  
-   **远程桌面连接到服务器和客户端计算机**-使用**远程连接**My Server 2012 R2 以通过远程桌面连接到你的 Windows Server Essentials 服务器和客户端计算机中。  
  
##  <a name="BKMK_OS"></a> 操作系统要求  
 若要在笔记本电脑、 PC 或 Surface 设备上使用 My Server 或 My Server 2012 R2，计算机必须具有以下操作系统之一：Windows 8.1、 Windows RT 8.1、 8 或 Windows rt。  
  
##  <a name="BKMK_Install"></a> 安装 My Server  
 你可以从 [Windows 应用](https://windows.microsoft.com/windows-8/apps) 应用商店下载适当的 My Server 应用。 如果您的计算机具有 Windows 8 或 Windows RT 操作系统，并且你想要使用的服务器名称连接到您的 intranet 上的服务器，还需要从服务器中安装证书。  
  
#### <a name="to-install-my-server-or-my-server-2012-r2-on-your-windows-based-laptop-pc-or-surface-device"></a>在基于 Windows 的笔记本电脑、PC 或 Surface 设备上安装 My Server 或 My Server 2012 R2  
  
1.  通过 [Windows 应用](https://windows.microsoft.com/windows-8/apps) 将适当的 My Server 应用安装到基于 Windows 的笔记本电脑、PC 或 Surface 设备上。  
  
    |服务器操作系统|下载|  
    |-----------------------------|--------------|  
    | Windows Server Essentials|[My Server](https://apps.microsoft.com/webpdp/app/19d94f81-db21-4234-8b49-806694dbbaea)|  
    | Windows Server Essentials|[My Server 2012 R2](https://apps.microsoft.com/webpdp/app/67e86695-bda3-4f32-96c4-2e20e56f1cf3)|  
  
2.  如果你想要能够使用服务器名称连接到 Windows Server Essentials 服务器通过 intranet，并且您的设备具有 Windows 8 或 Windows RT 操作系统，请在计算机上安装默认的 CAROOT.cer 证书，通过执行以下 p过程。 Windows 8.1 和 Windows RT 8.1 中不需要手动安装证书。  
  
###  <a name="BKMK_InstallCert"></a> 若要在 Windows 8、 Windows 8.1 或 Windows RT 设备上安装 My Server 的证书  
  
1.  在计算机上打开 Web 浏览器，然后从服务器下载默认证书 CAROOT.cer。 若要执行此操作，请键入以下地址，用你的服务器名称（例如，**marketing.contoso.com**）替代 <*servername*>：  
  
     **http://<servername\>/connect/default.aspx?get=caroot.cer**  
  
2.  安装证书：  
  
    1.  在“开始”  页上，打开“文件资源管理器”  。  
  
    2.  确保显示隐藏项和文件扩展名：在“查看”  选项卡的“显示/隐藏  组中，选中“隐藏项”  和“文件扩展名”  复选框。  
  
    3.  导航到包含刚下载的 CAROOT.cer 文件的文件夹。  
  
    4.  右键单击 CAROOT.cer 文件，然后单击“打开”  。  
  
    5.  在“证书”  对话框中，单击“安装证书”  。  
  
    6.  选择“本地计算机”  作为安装位置，然后单击“下一步”  。  
  
    7.  在“证书存储”  向导页上，选择“将所有的证书放入下列存储”  ，然后使用“浏览”  选择“受信任的根证书颁发机构”  存储。 然后单击 **“完成”** 。  
  
##  <a name="BKMK_UseServer"></a> 使用我的服务器  
 若要开始使用 My Server 或 My Server 2012 R2 应用，请打开应用，然后简要了解它的功能。  
  
#### <a name="to-open-my-server-or-my-server-2012-r2"></a>打开 My Server 或 My Server 2012 R2  
  
1.  从打开 My Server （适用于 Windows Server Essentials) 或 （对于 Windows Server Essentials) 的 My Server 2012 R2**启动**Windows 设备的页面。  
  
2.  登录到 Windows Server Essentials 服务器在服务器上使用你的帐户。 若要识别服务器，请采取以下方式：  
  
    -   在非本地环境中，使用服务器的域名-例如， **marketing.contoso.com**。  
  
    -   如果你要通过 intranet 连接的本地，在前面的示例中使用的服务器名称-，服务器名称是**市场营销**。 （如果使用 Windows 8 或 Windows RT 的计算机连接到本地，并且想要使用此方法，您必须安装 CAROOT.cer 证书从服务器。 有关详细信息，请参阅[若要在 Windows 8、 Windows 8.1 或 Windows RT 设备上安装 My Server 的证书](#BKMK_InstallCert)。)  
  
###  <a name="BKMK_Features"></a> My Server 的功能  
 下表介绍了 My Server 和 My Server 2012 R2 应用的功能。 你在服务器上的帐户类型决定你能够看到的内容和能够执行的操作。 所有用户都可以使用共享资源、自定义“最近”  显示内容、确定将最近使用的文件缓存多长时间以供脱机访问，以及启用或禁用通过付费连接进行下载。 Windows Server Essentials 服务器上的管理员还可以管理警报、 设备和用户。 标准用户帐户查看警报和连接设备的能力较为有限，具体取决于用户帐户中的属性；下表中注明了各项功能的要求。  
  
### <a name="features-of-the-my-server-and-my-server-2012-r2-apps-for-windows-server-essentials"></a>适用于 Windows Server Essentials 的 My Server 和 My Server 2012 R2 应用的功能  
  
|功能|描述|  
|-----------------|-----------------|  
|管理警报|-（仅限管理员） 解决警报的服务器上，或忽略不需要操作的警报。 打开或关闭通知（“权限”  设置、“通知”  选项）<br />-（标准用户帐户） 查看网络运行状况警报。<br />     **注意：** 用户可以在 My Server 中查看警报**用户可以查看网络运行状况警报**必须在选择设置**常规**用户帐户的设置。 有关详细信息，请参阅 [Manage user accounts using the Dashboard](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)。|  
|管理设备|（仅限管理员）<br /><br /> -当连接到 Windows Server Essentials 服务器，查看有关每个连接的计算机中的详细信息**设备**视图。 脱机设备以阴影表示。<br />-启动和停止已连接的计算机的备份。<br />-在 My Server 中打开或关闭通知。 （“权限”  设置、“通知”  选项）<br /><br /> 所有用户：<br /><br /> -查看您的用户帐户有权访问的客户端计算机。 （“设备”  屏幕）<br />-监视这些计算机的警报。 （“警报”  屏幕）<br />-(My Server 2012 R2 中仅） 连接到使用远程 Web 访问这些计算机。 （“设备”  屏幕、“远程连接”  按钮）|  
|使用“远程桌面”连接计算机|(My Server 2012 R2 仅)使用 Windows Server Essentials 服务器或客户端计算机打开远程桌面会话。 （“设备”  屏幕、“远程连接”  按钮）<br /><br /> **注意：** 若要启用此功能，请从“Windows 应用”将[远程桌面应用](https://apps.microsoft.com/webpdp/app/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)下载并安装到你的计算机上。 标准用户帐户可以连接到它们有权登录的设备。 若要使用户能够登录某台计算机，请将该计算机添加到用户帐户的“计算机访问”  选项卡中。 有关详细信息，请参阅 [Assign user accounts permission to log on to specific network computers](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)。|  
|管理用户|（仅限管理员）更改用户帐户的密码。 结束用户会话在服务器上。 （“用户”  设置）|  
|使用共享文件|<ul><li>上传和下载文件中的共享文件 （共享文件夹的服务器有权访问），你的个人共享，或从 Windows 8.1 的设备、 SkyDrive 或网络存储。 创建文件夹。 对服务器进行添加（上载）、编辑和删除文件。</li><li>上载或下载文件时查看传输状态。 取消传输。 解决文件冲突。</li><li>在本地计算机、服务器、SkyDrive 或网络存储上无缝使用文件和文件夹。 文件列表显示你最近在计算机上、SkyDrive 或网络存储中使用的文件夹，以及服务器上的共享文件夹，并允许你浏览其中任何位置的文件夹。</li><li>在服务器上搜索文件夹和文件；单击文件进行下载，并使用默认应用将其打开。 在脱机模式下，搜索仅限于脱机文件。</li><li>共享图片、音乐和视频。 单击要在 Windows 8 图片、 音乐或视频播放器中打开的文件。 或使用“打开”  或“打开方式”  在其他应用中打开文件。 与通常一样，你可以将选定应用设置为用于该媒体类型的默认应用。<br /><br />     **注意：** 默认情况下，媒体流式处理功能将不可用在 Windows Server Essentials 和 Windows Server 2012 R2 中安装了 Windows Server Essentials 体验角色。 有关详细信息，请参阅[管理数字媒体](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)。<br /><br /> <ul><li>**图片** - 在“图片”  视图中，点击图片将其打开。 再次点击图片返回 My Server 中的缩略图视图。</li><li>**音乐**-在**音乐**视图中，查看唱片集或多首歌曲在服务器上共享。 点击某个项目在音乐播放器中将其打开。</li><li>**视频**-单击缩略图中的**视频**视图，以打开视频播放器。</li></ul></li></ul>|  
|使用 SharePoint Online 库|(My Server 2012 R2 仅)处理你的团队的 SharePoint Online 库中的文件。 打开团队网站。 （SharePoint Online 部分：打开团队网站，然后深化到要打开的文档库或文件。 当您打开一个文件时，您必须登录到 Office 365 与您的网络帐户关联的 Microsoft 联机帐户。）<br /><br /> **注意：** 若要使用此功能，服务器必须与 Office 365 集成、 Office 365 订阅必须包括 SharePoint Online 和您在服务器上的用户帐户必须具有与之关联的 Microsoft Online Services 帐户。 有关将 Office 365 和 SharePoint Online 集成与 Windows Server Essentials 的信息，请参阅[服务的 Windows Server Essentials 集成概述-第 1 部分](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)和[的服务集成概述Windows Server Essentials-第 2 部分](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)。|  
|自定义“最近”  显示内容|“最近”  列表使你可以快速访问你现在处理的文件。 你可以进行以下更改：<br /><br /> -设置的最新的文件历史记录，若要显示的天数。 （“最近”  设置：“‘最近’中的保存天数”  ；默认值 = 7 天）<br />-清除**最近**显示，如果不再需要处理你一直使用的文件。 这不会影响缓存；这些文件仍可脱机使用。 （“最近”  设置：“清除”  按钮）<br />     **更改暂存文件夹路径**如果不需要的文件的脱机访问，使用**清除所有**中**脱机**设置从缓存中删除的文件。|  
|脱机工作|默认情况下，你在过去 7 天内访问的文件均可供脱机使用。 脱机更改将在你连接到服务器时立即与服务器同步。<br /><br /> 你可以对脱机配置进行以下更改：<br /><br /> -更改为缓存的工作的天数。 (**脱机**My Server 中的设置**文件**My Server 2012 R2 中的设置：“缓存期”  ；默认值 = 7 天）<br />-允许通过付费网络传输的文件或关闭该功能。 此功能默认为关闭状态，以防止通过付费网络传输文件而产生昂贵的费用。 （“脱机”  或“文件”  设置：“打开或关闭通过付费网络自动传输文件”  ；默认值 =“关闭”  ）<br />-你有时可能需要清除缓存。 在“最近”  或“文件”  设置中查看缓存大小；然后使用“清除”  或“全部清除”  来清除缓存。<br /><br /> **更改暂存文件夹路径**若要查明哪些文件将可供脱机使用，请在任一共享文件夹中，选择“脱机缓存”筛选器而非“全部”  。<br /><br /> **注意：** 默认情况下，缓存存储的文件历史记录与“最近”  列表显示的记录相同。 请注意，如果你清除了缓存，或者缓存的设置与“最近”  显示内容不匹配，则“最近”  列表中的某些文件可能无法脱机使用。|  
|后台同步|每当你登录 My Server 时，以及在连接到服务器的状态下进行更改时，文件便会与服务器进行同步。 为了保持性能，在大多数文件夹上将不会自动执行资源密集型后台同步。 为了避免产生昂贵的传输费用，在 3G 网络上不执行后台同步。<br /><br /> -解决文件冲突，显示在**冲突**区域中的**传输状态**页。 执行文件传输时，主屏幕将显示“传输状态”  和“警报”  超级按钮。|  
  
##  <a name="BKMK_ConnectServer"></a> 如何从本地网络连接到你的服务器  
 若要在适用于 Windows Phone、Windows 8 和 Windows 8.1 的 Windows Server Essentials 中成功地使用 My Server 应用，你必须首先在设备上安装服务器证书。 该证书使你可以将设备连接到在本地网络中运行 Windows Server Essentials 的服务器。 使用以下过程连接到本地网络中的服务器，并在运行 Windows 8 或 Windows 8.1 的 Windows Phone 或计算机上安装服务器证书。  
  
#### <a name="to-connect-to-your-server-from-your-windows-phone"></a>从 Windows Phone 连接到你的服务器  
  
1.  在运行 Windows 8 或 Windows 8.1 的 Windows Phone 上打开 Internet Explorer。  
  
2.  在地址栏中，键入**http://<yourservername\>/connect/default.aspx?get=caroot.cer**，然后按 Enter。  
  
3.  当系统提示你安装 caroot.cer 证书时，单击“安装”  。  
  
4.  等待证书安装完成。  
  
    > [!NOTE]
    >  证书安装完成后，便可以使用服务器名称和网络凭据登录适用于 Windows Phone 的 My Server 应用。  
  
#### <a name="to-connect-to-your-server-in-your-local-network-from-a-computer-running-windows-8-or-windows-81"></a>从运行 Windows 8 或 Windows 8.1 的计算机连接到本地网络中的服务器  
  
1.  在运行 Windows 8 或 Windows 8.1 的计算机上打开 Internet Explorer。  
  
2.  在地址栏中，键入**http://<yourservername\>/connect/default.aspx?get=caroot.cer**，然后按 Enter。  
  
3.  当系统提示你安装 caroot.cer 证书时，单击“打开”  。  
  
4.  在“证书”  对话框中，单击“安装证书”  。  
  
5.  在“证书导入”向导中，选择“本地计算机”  作为存储位置。  
  
6.  选择“将所有的证书放入下列存储”  ，然后浏览到“受信任的根证书颁发机构”  位置。  
  
7.  按照说明完成向导。  
  
    > [!NOTE]
    >  证书安装完成后，便可以使用服务器名称和网络凭据登录适用于 Windows 8 或 Windows 8.1 的 My Server 应用。  
  
## <a name="see-also"></a>请参阅  
  
-   [适用于 Windows Server Essentials-第 1 部分的服务集成概述](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [适用于 Windows Server Essentials-第 2 部分的服务集成概述](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [管理随处访问](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [可以通过远程方式](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [可以通过远程方式](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

