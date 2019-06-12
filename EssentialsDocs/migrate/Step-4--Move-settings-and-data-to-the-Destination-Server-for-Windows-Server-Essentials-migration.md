---
title: 步骤 4：将设置和数据移到目标服务器以进行 Windows Server Essentials 迁移
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: fa6ab8e2108e569b7cef6bfbf0d20af4fa31016d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432577"
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>步骤 4：将设置和数据移到目标服务器以进行 Windows Server Essentials 迁移

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本节提供从源服务器迁移数据和设置的相关信息。 将设置和数据移到目标服务器，如下所示：：  
  
-   [将数据复制到目标服务器](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [配置网络](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [将获得允许的计算机映射到用户帐户](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a> 将数据复制到目标服务器  
 在将数据从源服务器复制到目标服务器之前，请执行以下任务：  
  
-   在源服务器上查看共享文件夹列表（包括每个文件夹的权限）。 在目标服务器上创建或自定义这些文件夹，使其与要从源服务器迁移的文件夹结构相匹配。  
  
-   查看每个文件夹的大小，并确保目标服务器具有足够的存储空间。  
  
-   使源服务器上的共享文件夹对所有用户只读，以便在将文件复制到目标服务器时，不会在硬盘驱动器上发生写入操作。  
  
-   “客户端计算机备份”  文件夹不能迁移到目标服务器。 在服务器迁移之前，确保所有客户端计算机正常运行。 完成服务器迁移后，建议配置和启动客户端计算机备份，以确保对所有重要客户端计算机的数据进行备份。  
  
-   **文件历史记录备份**文件夹不能直接迁移到目标服务器，由于 Windows Server Essentials 中的文件夹结构和备份元数据更改。 但是，可以为特定计算机上的特定用户迁移“文件历史记录备份”  文件夹。 若要执行此操作，应在用于该用户和计算机的“文件历史记录备份”  文件夹中找到“数据”  文件夹，然后将该“数据”  文件夹复制到目标服务器上的“文件历史记录备份”  文件夹中。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>将数据从源服务器复制到目标服务器  
  
1. 以域管理员身份登录到目标服务器，然后打开命令提示符窗口或 Windows PowerShell 命令提示。  
  
2. 如果使用命令提示符窗口，则键入以下命令，然后按 ENTER：  
  
   `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
    其中：  
  
   - \<SourceServerName\>是源服务器的名称  
  
   - \<SharedSourceFolderName\>是源服务器上的共享文件夹的名称  
  
   - \<PathOfTheDestination\>是你想要移动的文件夹的绝对路径  
  
   - \<SharedDestinationFolderName\>是数据将复制到目标服务器上的文件夹  
  
     例如  `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`。  
  
3. 如果使用 Windows PowerShell，则键入以下命令，然后按 Enter 键。  
  
    `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4. 对每个要从源服务器迁移的共享文件夹重复此过程。  
  
##  <a name="BKMK_Network"></a> 配置网络  
  
#### <a name="to-configure-the-network"></a>配置网络  
  
1. 在目标服务器上，打开仪表板。  
  
2. 在仪表板“主页”  页面上，单击“设置”  ，单击“设置随处访问”  ，然后选择“单击以配置随处访问”  选项。  
  
3. 此时会出现“设置随处访问”向导。 完成向导中的说明，配置你的路由器名和域名。  
  
   如果路由器不支持 UPnP 框架，或如果 UPnP 框架已禁用，则路由器名称旁边可能会出现一个黄色的警告图标。 请确保下列端口为打开状态，并且已定向到目标服务器的 IP 地址：  
  
-   端口 80：HTTP Web 流量  
  
-   端口 443:HTTPS Web 流量  
  
> [!NOTE]
>  如果想要在目标服务器上配置公用域名，你必须从源服务器上释放该域名，以避免动态 DNS 更新的竞争。  
  
##  <a name="BKMK_MapPermittedComputers"></a> 将获得允许的计算机映射到用户帐户  
 从早期版本的 Windows Small Business Server 或 Windows Server Essentials 迁移的每个用户帐户都必须映射到一台或多台计算机。  
  
#### <a name="to-map-user-accounts-to-computers"></a>将用户帐户映射到计算机  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击“用户”  。  
  
3.  在用户帐户列表中，右键单击用户帐户，然后单击“查看帐户属性”  。  
  
4.  单击“随处访问”  选项卡，然后单击“允许远程 Web 访问和访问 Web 服务应用程序”  。  
  
5.  依次单击“共享文件夹”  、“计算机”  、“主页链接”  ，然后单击“应用”  。  
  
6.  单击“计算机访问”  选项卡，然后单击要允许访问的计算机的名称。  
  
7.  为每个用户帐户重复步骤 3、4、5 和 6。  
  
> [!NOTE]
>  不必更改客户端计算机的配置。 它将进行自动配置。  
  
> [!NOTE]
>  完成迁移后，如果你在目标服务器上创建第一个新的用户帐户时遇到问题，则请删除你已添加的用户帐户，然后再次创建它。  
  
## <a name="next-steps"></a>后续步骤  
 已将设置和数据移动到目标服务器中。 现在，转到[步骤 5:启用在目标服务器以进行 Windows Server Essentials 迁移的文件夹重定向](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

