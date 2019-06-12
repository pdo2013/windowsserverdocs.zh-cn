---
title: 使用 Windows Server Essentials 中的远程 Web 访问
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: b78d6abbaf287fc56336ff8d16127ce249b97519
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435912"
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>使用 Windows Server Essentials 中的远程 Web 访问

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
  远程 Web 访问是一项功能的 Windows 服务器 Essentials，可用于访问文件/文件夹和 web 浏览器通过网络上的计算机从任何位置连接到 Internet。 
  
  远程 Web 访问帮助你在离开时与你的 Windows Server Essentials 网络保持连接。 当您登录到远程 Web 访问时，可以连接到 Windows Server Essentials 网络上计算机、 打开仪表板以管理 Windows Server Essentials 网络，和访问所有服务器上的共享文件夹和媒体文件。  
  
 本主题包含以下各节：  
  

-   [连接到远程 Web 访问](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [共享文件和文件夹](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [从移动设备连接](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a> 连接到远程 Web 访问  
  
-   [登录到远程 Web 访问](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [远程访问您的计算机](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [连接到远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [共享文件和文件夹](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [从移动设备连接](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a> 连接到远程 Web 访问  
  
-   [登录到远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [远程访问您的计算机](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a> 登录到远程 Web 访问  
 当您从登录到远程 Web 访问在本地或远程计算机时，您可以在网络上运行 Windows Server Essentials 和计算机的服务器上访问资源。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>若要从网络计算机登录到远程 Web 访问  
  
1.  打开 Web 浏览器中，键入**https://***< 你的服务器名称\>***/远程**中地址栏中，然后按 Enter。  
  
    > [!NOTE]
    >  请确保在 https 中包含 s。  
  
2.  在远程 Web 访问登录页上，在文本框中，键入你的用户名和密码，然后单击箭头。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>从远程计算机登录到远程 Web 访问  
  
1.  打开 Web 浏览器中，键入**https://***< 你的域名\>***/远程**中地址栏中，然后按 Enter。  
  
    > [!NOTE]
    >  你可以从网络管理员处获得你的域名信息。 请确保在 https 中包含 s。  
  
2.  在远程 Web 访问登录页上，在文本框中，键入你的用户名和密码，然后单击箭头。  
  
###  <a name="BKMK_1.5"></a> 远程访问您的计算机  
 当你离开办公室时，可以使用 Web 浏览器登录到远程 Web 访问站点远程访问 Windows Server Essentials 仪表板、 共享的文件夹和网络上的计算机上。  
  
 当你连接到仪表板时，可以管理 Windows Server Essentials，就像你在办公室时一样。 可以执行所有常用的管理任务，例如添加用户帐户、添加共享文件夹、设置共享文件夹访问等。 当你在网络上连接到计算机时，可以访问这些计算机的桌面，就像在办公室中坐在它们的前面一样。  
  
 **“状态”** 列显示你是否可以连接到网络上的计算机，并可以包含以下值：  
  
-   **已推出**  
  
     计算机已打开，并且可用于远程连接。 如果第三方防火墙阻止该连接，则即使你看到此状态，仍可能无法连接到此计算机。  
  
-   **脱机或处于睡眠状态**  
  
     计算机处于关闭状态，或处于睡眠或休眠模式。 若计算机已脱机或处于睡眠状态，则将实时更新其状态，以便你知道该计算何时可用。  
  
-   **不支持的操作系统**  
  
     计算机上的操作系统不支持远程桌面。 如果有更改，则在服务器上更新此状态可能需要多达 6 个小时。  
  
-   **连接已禁用**  
  
     计算机连接受到防火墙阻止，或者计算机上的远程桌面已由组策略禁用。 如果有更改，则在服务器上更新此状态可能需要多达 6 个小时。  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>连接到你的网络上的计算机  
 在 **“设备”** 选项卡上，单击计算机的名称。 可以只选择状态为 **“可用”** 的计算机。  
  
#### <a name="to-connect-to-the-server-dashboard"></a>连接到服务器仪表板  
 在 **“设备”** 选项卡上，单击你的服务器的名称。 可以只选择状态为 **“可用”** 的计算机。 必须能够在你的服务器上提供管理员用户帐户和密码以使用仪表板。  
  
##  <a name="BKMK_SharedFolders"></a> 共享文件和文件夹  
  

-   [上传和下载远程 Web 访问中的文件](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [创建、 重命名、 移动、 删除或复制远程 Web 访问中的文件和文件夹](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [上传和下载远程 Web 访问中的文件](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [创建、 重命名、 移动、 删除或复制远程 Web 访问中的文件和文件夹](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a> 上传和下载远程 Web 访问中的文件  
 在远程 Web 访问的 **“共享文件夹”** 选项卡上，你可以执行以下操作：  
  
-   从你的计算机中将文件上载（发送）到 Windows Server Essentials。  
  
    > [!NOTE]
    >  你可以只将文件而不是文件夹上载到远程 Web 访问。 如果你希望服务器上的 **“共享文件夹”** 中具有与在你计算机上相同的文件和文件夹层次结构，则必须在远程 Web 访问中的服务器上创建文件夹，然后将文件上载到你创建的文件夹中。 有关创建服务器文件夹的信息，请参阅 [Add or move a server folder](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
-   将文件和文件夹从 Windows Server Essentials 下载（接收）到你的计算机。  
  
-   在 Windows Server Essentials 的共享文件夹中创建一个文件夹。  
  

-   移动、删除和重命名 Windows Server Essentials 上的文件和文件夹。 有关详细信息，请参阅[创建、 重命名、 移动、 删除或复制文件和文件夹在远程 Web 访问](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

-   移动、删除和重命名 Windows Server Essentials 上的文件和文件夹。 有关详细信息，请参阅[创建、 重命名、 移动、 删除或复制文件和文件夹在远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

  
#### <a name="upload-files"></a>上载文件  
  
###### <a name="to-upload-files"></a>上载文件  
  
1. 在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2. 从文件和文件夹的共享文件夹列表中，单击要将文件上载到的文件夹，然后单击 **“上载”** 。  
  
3. 如果尚未加载标准上载工具，则单击 **“使用标准上载方法”** 。  
  
4. 单击“浏览”   查找计算机上的文件。  
  
5. 浏览计算机上的文件夹，以查找要上载的文件，然后单击 **“打开”** 。  
  
6. 对于每个要上载的文件，请重复步骤 2 和 3。  
  
7. 在你已添加所有要上载的文件后，请单击 **“上载”** 。  
  
   轻松文件上载工具简化了运行 Windows Server Essentials 的服务器上的文件上载的过程。 通过使用拖放功能，你可以将尽可能多的文件添加到轻松文件上载工具，然后将这些文件上载到服务器上的共享文件夹。  
  
> [!NOTE]
>  在与 HTML5 兼容的 Web 浏览器中，支持本地上载多个文件。 仅在 Web 浏览器不支持 HTML5 时需要此工具。  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>使用轻松文件上载工具上载文件  
  
1.  在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2.  从文件和文件夹的共享文件夹列表中，单击要将文件上载到的文件夹，然后单击 **“上载”** 。 如果要上载到的文件夹不存在，则单击 **“新文件夹”** ，在对话框中键入新文件夹的名称，然后单击 **“确定”** 。  
  
3.  你可能需要运行 Windows Server 解决方案加载项。 如果是这样，则单击在屏幕顶部的黄色条带，单击运行 **“加载项”** ，然后在对话框中单击 **“运行”** 。  
  
4.  如果尚未加载轻松文件上载工具，则单击 **“使用轻松文件上载工具”** 。  
  
5.  你可以从 Windows 资源管理器将文件拖放到轻松文件上载工具，或单击 **“浏览以选择文件”** 。  
  
6.  当你完成将文件添加到你想要将其上载到的所选文件夹时，请单击 **“上载”** 。  
  
7.  当文件上载成功时，单击 **“关闭”** 。  
  
#### <a name="download-files-or-folders"></a>下载文件或文件夹  
  
###### <a name="to-download-a-single-file"></a>下载单个文件  
  
1. 在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2. 从共享文件夹的文件列表中，单击你想要下载到你的家庭计算机的文件旁边的复选框。  
  
3. 单击 **“下载”** 以开始下载。  
  
4. 在 **“文件下载”** 对话框上，单击 **“保存”** 以将文件保存到你的计算机。  
  
5. 在 **“另存为”** 对话框中，选择要保存该文件的位置，然后单击 **“保存”** 。 下载单个文件前，不会将它压缩。  
  
   下载多个文件或文件夹有两个选项。 选择适合你需要的选项：  
  
> [!NOTE]
>  仅当要将多个文件或文件夹下载到你的计算机时，这些选项才可用。  
  
- **自解压可执行文件 (.exe)**  
  
  > [!NOTE]
  >   本部分适用于运行 Windows Server Essentials 的服务器。  
  
   自解压可执行文件是你可以下载的文件，此文件将解压缩（可执行）程序与压缩的文件组合在一起。 当你运行可执行程序时，它会自动解压缩压缩的文件（自解压）。 这是分发压缩数据的常用方式，无需担心收件人是否具有正确的解压缩实用工具。  
  
  > [!NOTE]
  >  此选项支持 Unicode 字符。  
  
- **Windows 压缩文件夹 (.zip)**  
  
   压缩文件能够创建小于原始文件的文件压缩版本。 该压缩版本的文件具有 .zip 文件扩展名。 通过压缩大小能减少最多的文件类型是面向文本的文件类型，例如 .txt、.doc、.xls 和使用非压缩文件类型（如 .bmp）的图形文件。 某些图形文件（如 .jpg 和 .gif 文件）已经使用过压缩，因此通过压缩，文件的大小减小得非常少。 此外，与大部分是文本的文档相比，包含大量图形的 Word 文档无法减少得一样多。  
  
  > [!NOTE]
  >  此选项的 Windows Server Essentials 中的国际文件名称提供有限的支持。  
  
  开始实际下载之前，将创建 exe 或 zip 文件。 这可能需要几分钟，具体取决于文件的数量和要下载文件的总大小。 创建下载文件后，下载文件的操作发生在后台。 这允许你在下载过程完成时可以继续工作。  
  
###### <a name="to-download-multiple-files-or-folders"></a>下载多个文件或文件夹  
  
1.  在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2.  从共享文件夹的文件列表中，单击要下载到你的家庭计算机的文件或文件夹旁边的复选框。  
  
3.  单击 **“下载”** 以开始下载。  
  
4.  在 **“选择下载格式”** 对话框中，单击以选择你喜欢的下载格式选项，然后单击 **“确定”** 。 将使用你选定的格式选项准备压缩文件。  
  
5.  在 **“文件下载”** 对话框上，单击 **“保存”** 以将文件保存到你的计算机。  
  
6.  在 **“另存为”** 对话框中，选择要保存该文件的位置，然后单击 **“保存”** 。  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>检索下载到你的计算机的压缩文件  
  
> [!NOTE]
>   本部分适用于运行 Windows Server Essentials 的服务器。  
  
 如果你选择要下载的多个文件或文件夹，则可以接收自解压缩的压缩可执行文件 (.exe) 或压缩 (.zip) 文件。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>从压缩 (.exe) 文件中检索文件  
  
1.  在你的计算机上，双击压缩文件，以将其打开。  
  
2.  按照说明将文件解压缩到你的计算机上的文件夹中。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>从压缩 (.zip) 文件中检索文件  
  
1.  在你的计算机上，双击压缩文件，以将其打开。  
  
2.  选择要检索的文件，然后将这些文件拖动到将要存储它们的计算机上的某个文件夹。  
  
    > [!NOTE]
    >  如果使用第三方文件压缩程序，请按照该程序的过程从压缩文件中提取你的文件。  
  
###  <a name="BKMK_2"></a> 创建、 重命名、 移动、 删除或复制远程 Web 访问中的文件和文件夹  
 可以使用远程 Web 访问在现有的共享文件夹中创建新文件夹，用于对服务器上的文件和文件夹进行重命名、移动、复制以及删除。  
  
> [!NOTE]
>  若要在运行 Windows Server Essentials 的服务器上添加新的共享文件夹，必须使用仪表板。 若要从远程 Web 访问连接到服务器控制台，在 **“计算机”** 选项卡上，单击服务器名称，单击 **“连接”** ，然后按照登录到服务器的说明进行操作。 有关如何创建共享文件夹的信息，请参阅 [Add or move a server folder](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
##### <a name="to-create-a-new-folder"></a>创建新文件夹  
  
1.  在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2.  在任务栏中，单击 **“新文件夹”** 。  
  
3.  键入文件夹的名称，然后单击 **“确定”** 。  
  
##### <a name="to-rename-a-file-or-folder"></a>重命名文件或文件夹  
  
1.  在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2.  右键单击你想要重命名的文件或文件夹，然后单击 **“重命名”** 。  
  
3.  在文本框中键入新名称，然后单击 **“确定”** 。  
  
##### <a name="to-move-files-or-folders"></a>移动文件或文件夹  
  
1.  在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2.  选中你要移动的文件或文件夹旁边的复选框，右键单击其中一个选定的文件或文件夹，然后单击 **“剪切”** 。  
  
3.  右键单击要将文件或文件夹移动到的文件夹，然后单击 **“粘贴”** 。  
  
##### <a name="to-delete-a-file-or-folder"></a>删除文件或文件夹  
  
1.  在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2.  选中要删除的文件或文件夹旁边的复选框，右键单击其中一个选定的文件或文件夹，然后单击 **“删除”** 。  
  
3.  若要确认要删除所选文件和文件夹，请单击 **“是”** 。  
  
##### <a name="to-copy-files-or-folders"></a>复制文件或文件夹  
  
1.  在远程 Web 访问中，单击 **“共享文件夹”** 选项卡，然后单击共享文件夹链接。 显示该共享文件夹中的文件和文件夹列表。  
  
2.  选中要复制的文件或文件夹旁边的复选框，右键单击其中一个选定的文件或文件夹，然后单击 **“复制”** 。  
  
3.  右键单击要将文件或文件夹复制到的文件夹，然后单击 **“粘贴”** 。  
  
##  <a name="BKMK_ConnectMobile"></a> 从移动设备连接  
  

-   [从移动设备使用远程 Web 访问](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [支持的移动设备 Web 浏览器](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [从移动设备使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [支持的移动设备 Web 浏览器](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a> 从移动设备使用远程 Web 访问  
 你可以从你的智能手机登录到远程 Web 访问，以查看服务器上共享文件夹中的文件和文件夹。  
  
> [!NOTE]
>  还可以从 [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) 下载并使用适用于 Windows Server Essentials 的 My Server 应用来访问存储在服务器上的共享文件夹和媒体文件。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>从移动设备登录到远程 Web 访问  
  
1.  打开 Web 浏览器并键入**https://***<你的域名\>***/远程**在地址栏中。  请确保在 https 中包含 s。  
  
2.  在远程 Web 访问登录页上，在文本框中，键入你的用户名和密码，然后单击箭头。 你已登录到远程 Web 访问的移动版本。  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>切换到远程 Web 访问的桌面版  
  
1.  打开 Web 浏览器并键入**https://***<你的域名\>***/远程**在地址栏中。  请确保在 https 中包含 s。  
  
2.  在远程 Web 访问登录页上，键入你的用户名和密码的文本框中，单击**查看桌面版本**，然后单击箭头。 你已登录到远程 Web 访问的桌面版本。  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>返回到远程 Web 访问的移动版本  
  
1. 注销。  
  
2. 打开 Web 浏览器并键入**https://***<你的域名\>***/远程/m**在地址栏中。 请确保在 https 中包含 s。  
  
3. 显示远程 Web 访问的移动版本。 在远程 Web 访问登录页上，在文本框中，键入你的用户名和密码，然后单击箭头。 登录到远程 Web 访问的移动版本。  
  
   你可以搜索服务器上的共享文件夹中的文件和文件夹。  
  
###  <a name="BKMK_9"></a> 支持的移动设备 Web 浏览器  
 受移动设备支持的 Web 浏览器包括：  
  
-   Internet Explorer 移动 6.0 或更高版本  
  
-   Safari  
  
-   Blackberry  
  
-   Symbian 6.0 或更高版本  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>请参阅  
  
-   [管理远程 Web 访问](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [可以通过远程方式](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [可以通过远程方式](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

