---
title: "第 4 步： 到达目的地 Server 为 Windows Server Essentials 迁移的设置和数据"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: db1169a5415039498718a7988c711d068945b4b7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>第 4 步： 到达目的地 Server 为 Windows Server Essentials 迁移的设置和数据

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

此部分中提供源服务器有关迁移数据和设置的信息。 移动设置和数据到目标服务器，如下所示：  
  
-   [复制到目标 Server 的数据](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [配置的网络](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [允许用户帐户的计算机的地图](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>复制到目标 Server 的数据  
 你将数据从源服务器复制到目标服务器之前，请执行以下任务：  
  
-   查看源在服务器上，包括权限的每个文件夹的共享文件夹的列表。 创建或自定义匹配要从源 Server 迁移文件夹结构目的地服务器上的文件夹。  
  
-   检查每个文件夹的大小，并确保目标服务器具有足够的存储空间。  
  
-   源服务器上的共享的文件夹仅阅读面向所有用户以使没有书写可能需要在硬盘上的位置时，你将文件复制到目标服务器。  
  
-   **客户端计算机备份**文件夹无法迁移到目的地的服务器。 Server 迁移之前，确保所有客户端计算机正常。 Server 迁移后，建议您将配置并开始客户端计算机备份，以确保备份你的所有重要的客户端计算机数据。  
  
-   **文件历史记录备份**文件夹无法直接迁移到由于文件夹结构和备份元数据中的更改，Windows Server Essentials 目标服务器。 但是，就可以迁移**文件历史记录备份**特定计算机上特定用户的文件夹。 若要执行此操作，您应找到**数据**中的文件夹**文件历史记录备份**文件夹该用户和计算机，然后复制**数据**到的文件夹**文件历史记录备份**目标服务器上的文件夹。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>若要将数据从源服务器复制到目标服务器  
  
1.  登录到目标服务器以管理员身份域，，然后打开 Command Prompt 窗口或 Windows PowerShell command prompt。  
  
2.  如果你使用的命令提示符窗口中，键入以下命令，并按 enter 键：  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     位置：  
  
    -   \ < SourceServerName\ > 是源服务器的名称  
  
    -   \ < SharedSourceFolderName\ > 是源服务器上的共享文件夹的名称  
  
    -   \ < PathOfTheDestination\ > 是你需要将文件夹移动绝对路径  
  
    -   \ < SharedDestinationFolderName\ > 是数据将复制到目标服务器上的文件夹  
  
     例如， `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`。  
  
3.  如果你使用 Windows PowerShell，键入以下命令，，然后按 ENTER。  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  对于每个要从源 Server 迁移的共享文件夹重复该过程。  
  
##  <a name="BKMK_Network"></a>配置的网络  
  
#### <a name="to-configure-the-network"></a>若要配置网络  
  
1.  在目标服务器上，打开仪表板。  
  
2.  仪表板上**家庭版**页上，单击**设置**，单击**设置任何地方访问**，然后选择**单击配置任何地方访问**选项。  
  
3.  安装任何地方访问向导将显示。 完成向导中的说明来配置你的路由器并域名。  
  
 如果你的路由器中不支持 UPnP 框架，或者 UPnP 框架已禁用，黄色警告图标可能会显示路由器名称旁边。 确保打开以下端口，他们将定向到目的地服务器的 IP 地址：  
  
-   端口 80: HTTP Web 通信  
  
-   端口 443: HTTPS Web 通信  
  
> [!NOTE]
>  如果你想要配置公众域名目标服务器上，你必须从源服务器以避免场竞赛，动态 DNS 更新发布的域名。  
  
##  <a name="BKMK_MapPermittedComputers"></a>允许用户帐户的计算机的地图  
 从以前版本的 Windows 小型企业服务器或 Windows Server Essentials 迁移每个用户帐户必须将映射到一个或多个计算机。  
  
#### <a name="to-map-user-accounts-to-computers"></a>将映射到计算机的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，右键单击用户帐户时，，，然后单击**查看帐户属性**。  
  
4.  单击**任何地方访问**选项卡，然后单击**允许远程网站访问和 web 服务的应用程序访问**。  
  
5.  单击**共享文件夹**，单击**计算机**，单击**主页链接**，然后单击**应用**。  
  
6.  单击**计算机的访问权限**选项卡，然后单击你希望允许访问计算机的名称。  
  
7.  每个用户帐户重复步骤 3 平板电脑、4、5 和 6。  
  
> [!NOTE]
>  你不需要更改的配置的客户端计算机。 自动配置。  
  
> [!NOTE]
>  完成迁移，如果你遇到问题，当您创建目标服务器上的第一个新的用户帐户后，你添加的用户帐户中删除并重新创建。  
  
## <a name="next-steps"></a>后续步骤  
 已经到目标服务器移动你的设置和数据。 现在转到[第 5 步： 启用 Windows Server Essentials 的目标 Server 迁移文件夹重定向](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

