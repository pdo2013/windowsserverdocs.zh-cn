---
title: "在 Windows Server Essentials 使用 Web 远程访问"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac384b545fe61b4a832debdc8aedcc81a75c669a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>在 Windows Server Essentials 使用 Web 远程访问

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
  远程访问 Web 可帮助你在离开时保持连接到你的 Windows Server Essentials 网络。 当你登录到远程访问 Web 上时，你可以连接到计算机，Windows Server Essentials 网络上、 打开管理你的 Windows Server Essentials 网络，该仪表板和访问所有共享的文件夹和媒体文件服务器上。  
  
 本主题包含以下部分：  
  

-   [连接到远程 Web 访问](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [共享文件和文件夹](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [从移动设备的连接](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>连接到远程 Web 访问  
  
-   [登录到访问远程 Web](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [远程访问你的电脑](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [连接到远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [共享文件和文件夹](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [从移动设备的连接](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>连接到远程 Web 访问  
  
-   [登录到访问远程 Web](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [远程访问你的电脑](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>登录到访问远程 Web  
 当你从登录到远程访问 Web 本地或远程计算机时，你可以访问您网络中运行 Windows Server Essentials 和计算机的服务器上的资源。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>网络计算机从登录到远程访问 Web  
  
1.  打开 Web 浏览器中，键入**https://***< YourServerName\ >***/远程**在地址栏，然后按 Enter。  
  
    > [!NOTE]
    >  请确保你在 https 包括 s。  
  
2.  在远程访问 Web 登录页面上，在文本框中，键入你的用户名和密码，然后单击键。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>若要远程计算机从登录到远程访问 Web  
  
1.  打开 Web 浏览器中，键入**https://***< YourDomainName\ >***/远程**在地址栏，然后按 Enter。  
  
    > [!NOTE]
    >  你可以从你的网络管理员获取你域名信息。 请确保你在 https 包括 s。  
  
2.  在远程访问 Web 登录页面上，在文本框中，键入你的用户名和密码，然后单击键。  
  
###  <a name="BKMK_1.5"></a>远程访问你的电脑  
 当你离开办公室，可用于你的 Web 浏览器登录到远程访问你的 Windows Server Essentials 仪表板、 共享的文件夹和网络上计算机远程 Web 访问站点上。  
  
 当你连接到仪表板时，你可以像在办公室一样管理 Windows Server Essentials。 你可以执行的所有常用的管理任务，如添加用户帐户添加的共享的文件夹，设置共享的文件夹的访问权限，等等。 当你在你的网络连接到计算机时，你可以像你坐放它们在办公室访问其桌面。  
  
 **状态**列向你显示是否你可以在你的网络连接到计算机，并且可能包括以下值：  
  
-   **可用**  
  
     计算机处于打开状态，并且可以为远程连接。 即使你看到此状态时，你仍然可能无法如果第三方防火墙阻止了该连接连接到此计算机。  
  
-   **离线状态或休眠状态**  
  
     计算机处于关闭状态，或处于睡眠或休眠模式。 如果离线状态或休眠状态的计算机，以便你可以知道计算机可用实时更新的状态。  
  
-   **不受支持的操作系统**  
  
     在计算机上的操作系统不支持远程桌面。 它可能需要最多 6 小时这种状态要更新服务器上，如果进行了更改。  
  
-   **连接已禁用**  
  
     计算机连接已被阻止的防火墙，或在计算机上，或通过组策略禁用远程桌面。 它可能需要最多 6 小时这种状态要更新服务器上，如果进行了更改。  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>若要连接到网络上计算机  
 在**设备**选项卡上，单击计算机的名称。 你可以选择仅有的计算机**可用**状态。  
  
#### <a name="to-connect-to-the-server-dashboard"></a>若要连接到服务器仪表板  
 在**设备**选项卡上，单击服务器的名称。 你可以选择仅有的计算机**可用**状态。 你必须能够提供用于在仪表板中你服务器上的管理员用户帐户和密码。  
  
##  <a name="BKMK_SharedFolders"></a>共享文件和文件夹  
  

-   [上传和下载文件远程访问 Web](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [创建、 重命名、 移动、 删除或复制的文件和文件夹中远程访问 Web](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [上传和下载文件远程访问 Web](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [创建、 重命名、 移动、 删除或复制的文件和文件夹中远程访问 Web](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>上传和下载文件远程访问 Web  
 在远程 Web 访问**共享文件夹**选项卡上，你可以执行以下操作：  
  
-   从你的计算机 （发送） 文件，对 Windows Server Essentials 上载。  
  
    > [!NOTE]
    >  你可以将仅文件和不适用于文件夹上载到远程访问 Web。 如果你想要具有相同的文件和文件夹分层**共享文件夹**服务器上当你在计算机上，你必须远程 Web 访问，在服务器上创建文件夹，然后将上载文件为你创建该文件夹。 有关创建 server 文件夹的信息，请参阅[添加或移动服务器文件夹](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
-   下载 （收到） 的文件和文件夹从 Windows Server Essentials 到计算机。  
  
-   在 Windows Server Essentials 上创建文件夹中的共享文件夹。  
  

-   移动、 删除和重命名文件和 Windows Server Essentials 上的文件夹。 有关详细信息，请参阅[创建、 重命名、 移动、 删除或复制的文件和文件夹远程访问 Web](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

-   移动、 删除和重命名文件和 Windows Server Essentials 上的文件夹。 有关详细信息，请参阅[创建、 重命名、 移动、 删除或复制的文件和文件夹远程访问 Web](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

  
#### <a name="upload-files"></a>将文件上传  
  
###### <a name="to-upload-files"></a>若要将文件上传  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  从共享文件夹的文件和文件夹的列表，请单击你想要上传到文件，然后单击的文件夹**上载**。  
  
3.  如果尚未加载标准上载工具，请单击**使用标准上载方法**。  
  
4.  单击**浏览**以查找你计算机上的文件。  
  
5.  浏览计算机以查找你想要上传，该文件的文件夹，然后单击**打开**。  
  
6.  每个文件，你想要上传，请重复步骤 2 到 3。  
  
7.  当你已添加的所有文件上传，请单击你希望**上载**。  
  
 轻松文件上传工具简化了运行 Windows Server Essentials 服务器上的文件上传的过程。 你可以添加多个文件所需通过拖放的功能，轻松文件上传的工具，然后将它们上载到服务器上的共享文件夹。  
  
> [!NOTE]
>  本地支持 HTML5 与兼容的 web 浏览器中进行上载多个文件。 仅在 web 浏览器不支持 HTML5 时需要此工具。  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>若要使用简单的文件上传工具的文件上传  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  从共享文件夹的文件和文件夹的列表，请单击你想要上传到文件，然后单击的文件夹**上载**。 如果你想要上传不存在的文件夹，请单击**新文件夹**，在对话框中，键入新文件夹的名称，然后单击**确定**。  
  
3.  你可能需要运行 Windows Server 解决方案加载项。 如果是这样，单击屏幕顶部黄色条中、 单击运行**加载项**，然后单击**运行**对话框中。  
  
4.  如果尚未加载轻松文件上传的工具，请单击**使用简单的文件上传工具**。  
  
5.  你可以将文件从 Windows 资源管理器拖放到轻松文件上传的工具，或者单击**浏览中选择文件**。  
  
6.  当你完成添加的文件上传到选定的文件夹中，单击你希望**上载**。  
  
7.  当成功上载文件时、 单击**关闭**。  
  
#### <a name="download-files-or-folders"></a>下载文件或文件夹  
  
###### <a name="to-download-a-single-file"></a>若要下载一个文件  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  从共享文件夹的文件列表中，单击你想要下载到你的家庭计算机的文件旁边的复选框。  
  
3.  单击**下载**开始下载。  
  
4.  在**文件下载**对话框中，单击**保存**将文件保存到你的计算机。  
  
5.  在**另存为**对话框中，选择要保存文件，然后单击位置**保存**。 一个文件不它下载之前进行压缩。  
  
 有两个选项下载多个文件或文件夹。 选择适合你的需求的选项：  
  
> [!NOTE]
>  仅当你将多个文件或文件夹下载到你的计算机，这些选项才可用。  
  
-   **自行解压缩可执行文件 (.exe)**  
  
    > [!NOTE]
    >   本部分适用于运行 Windows Server Essentials 的服务器。  
  
     自解压缩的可执行文件是一个文件，你可以下载组合解压缩 （可执行文件） 程序了用于压缩的文件。 可执行计划运行时，它将自动解压缩 （自行提取） 压缩的文件。 这是常见的方法，而无需担心收件人是否已正确解压缩实用程序分配压缩的数据。  
  
    > [!NOTE]
    >  此选项可支持 Unicode 字符。  
  
-   **Windows 压缩文件夹 (.zip)**  
  
     压缩文件创建的文件，则小于原始文件的压缩的版本。 压缩文件的版本具有.zip 文件扩展名。 最减少压缩的文件类型都面向文本的文件类型，例如.txt、.doc.xls，并图形文件如.bmp 该使用未压缩的文件类型。 某些图形文件.jpg 和.gif 文件，例如已使用压缩，并文件大小减少通过压缩很少。 此外，不为主要文本的文档更减少包含大量图形 Word 文档。  
  
    > [!NOTE]
    >  此选项国际文件名称在 Windows Server Essentials 提供有限的支持。  
  
 实际下载开始之前，将创建 exe 或 zip 文件。 根据我们的号码的文件和下载的文件的总大小，这可能需要几分钟。 创建下载的文件后，在后台下载的文件，会发生。 这允许你同时在下载过程完成继续工作。  
  
###### <a name="to-download-multiple-files-or-folders"></a>若要下载多个文件或文件夹  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  从共享文件夹的文件列表中，单击文件或文件夹你想要下载到你的家庭计算机旁边的复选框。  
  
3.  单击**下载**开始下载。  
  
4.  在**选择下载格式**对话框中，单击以选择下载格式化选项，你愿意，然后单击**确定**。 压缩的文件的格式化选项，你选择在准备好。  
  
5.  在**文件下载**对话框中，单击**保存**将文件保存到你的计算机。  
  
6.  在**另存为**对话框中，选择要保存文件，然后单击位置**保存**。  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>检索压缩的文件下载到你的计算机  
  
> [!NOTE]
>   本部分适用于运行 Windows Server Essentials 的服务器。  
  
 如果你选择多个文件或文件夹以下载，你可以接收自解压缩压缩可执行文件 (.exe) 或压缩的文件 (.zip)。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>若要从压缩 (.exe) 文件检索文件  
  
1.  你在计算机上，双击压缩的文件以打开它。  
  
2.  按照说明来解压缩文件在你的计算机上的文件夹。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>若要从压缩的文件 (.zip) 中检索文件  
  
1.  你在计算机上，双击压缩的文件以打开它。  
  
2.  选择你想要检索，这些文件，然后拖动文件到文件夹计算机上其中你希望将它们存储起来。  
  
    > [!NOTE]
    >  如果您使用第三方文件的压缩程序，请按照从压缩的文件解压缩文件该程序的过程。  
  
###  <a name="BKMK_2"></a>创建、 重命名、 移动、 删除或复制的文件和文件夹中远程访问 Web  
 你可以使用远程访问 Web 在现有的共享文件夹，若要重命名文件和文件夹中进行移动并将复制的文件和文件夹，并删除的文件和您的服务器上的文件夹中创建新文件夹。  
  
> [!NOTE]
>  若要运行的 Windows Server Essentials 服务器上添加新的共享的文件夹，必须使用仪表板。 在远程 Web 访问，从连接到服务器控制台**计算机**选项卡、 单击服务器名称、 单击**连接**，然后按照说明进行操作，并登录到服务器的。 有关如何创建共享的文件夹的信息，请参阅[添加或移动服务器文件夹](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
##### <a name="to-create-a-new-folder"></a>若要创建一个新文件夹  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享的文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  在任务栏中，单击**新文件夹**。  
  
3.  为该文件夹中，键入一个名称，然后单击**确定**。  
  
##### <a name="to-rename-a-file-or-folder"></a>若要重命名文件或文件夹  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享的文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  右键单击文件或文件夹，你想要重命名，然后单击**重命名**。  
  
3.  在文本框中，键入新名称，然后单击**确定**。  
  
##### <a name="to-move-files-or-folders"></a>若要移动的文件或文件夹  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享的文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  选择文件或文件夹你想要移动，右键单击所选的文件或文件夹，其中一个，然后单击旁边的复选框**剪切**。  
  
3.  右键单击你想要移动的文件或文件夹，然后单击该文件夹**粘贴**。  
  
##### <a name="to-delete-a-file-or-folder"></a>若要删除的文件或文件夹  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享的文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  选择文件或文件夹你想要删除，右键单击所选的文件或文件夹，其中一个，然后单击旁边的复选框**删除**。  
  
3.  若要确认你想要删除所选的文件和文件夹，请单击**是**。  
  
##### <a name="to-copy-files-or-folders"></a>若要复制文件或文件夹  
  
1.  在远程网站访问中，单击**共享文件夹**选项卡，然后单击共享的文件夹链接。 显示文件和文件夹在共享文件夹的列表。  
  
2.  选择文件或文件夹复制，右键单击所选的文件或文件夹，其中一个，然后单击你希望旁边的复选框**副本**。  
  
3.  右键单击你想要复制的文件或文件夹，然后单击该文件夹**粘贴**。  
  
##  <a name="BKMK_ConnectMobile"></a>从移动设备的连接  
  

-   [从移动设备使用远程访问 Web](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [移动设备的受支持的 Web 浏览器](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [从移动设备使用远程访问 Web](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [移动设备的受支持的 Web 浏览器](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>从移动设备使用远程访问 Web  
 你可以从登录到远程 Web 访问你的智能手机，以查看在服务器上的共享文件夹的文件和文件夹。  
  
> [!NOTE]
>  你还可以下载并用于 Windows Server Essentials 从我的服务器应用[Windows Phone 商城](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)来访问你的共享的文件夹和媒体文件存储在服务器上。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>若要从移动设备登录到远程访问 Web  
  
1.  打开 Web 浏览器，然后键入**https://***< YourDomainName\ >***/远程**地址栏中。  请确保你在 https 包括 s。  
  
2.  在远程访问 Web 登录页面上，在文本框中，键入你的用户名和密码，然后单击键。 登录到远程 Web 访问移动版本。  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>若要切换到的桌面版本远程访问 Web  
  
1.  打开 Web 浏览器，然后键入**https://***< YourDomainName\ >***/远程**地址栏中。  请确保你在 https 包括 s。  
  
2.  在远程访问 Web 登录页面上，键入你的用户名和密码的文本框中，单击**视图桌面版**，然后单击箭头。 登录到远程网站访问的桌面版本。  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>若要返回到远程 Web 访问移动版  
  
1.  注销。  
  
2.  打开 Web 浏览器，然后键入**https://***< YourDomainName\ >***/远程/m**地址栏中。 请确保你在 https 包括 s。  
  
3.  远程网站访问的移动版本将显示。 在远程访问 Web 登录页面上，在文本框中，键入你的用户名和密码，然后单击键。 登录到远程 Web 访问移动版本。  
  
 你可以搜索服务器上的共享文件夹中文件和文件夹。  
  
###  <a name="BKMK_9"></a>移动设备的受支持的 Web 浏览器  
 适用于移动设备的受支持的 web 浏览器包括：  
  
-   6.0 或更高版本的 Internet Explorer 移动版  
  
-   Safari  
  
-   Blackberry  
  
-   Symbian 6.0 或更高版本  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>请参阅  
  
-   [管理 Web 远程访问](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [远程工作](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [远程工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

