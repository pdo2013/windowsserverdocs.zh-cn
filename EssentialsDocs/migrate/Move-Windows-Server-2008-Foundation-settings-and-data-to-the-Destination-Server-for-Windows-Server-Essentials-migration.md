---
title: "移动到目标服务器以进行 Windows Server Essentials 迁移的 Windows Server 2008 基础设置和数据"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ff7d040-ebd1-421c-80db-765deacedd4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 80e70ba954d981985caa5329379661d978456c7f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-server-2008-foundation-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>移动到目标服务器以进行 Windows Server Essentials 迁移的 Windows Server 2008 基础设置和数据

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

移动设置和数据到目标服务器，如下所示： 

1.  [复制到目标服务器（可选）的数据](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [导入的 Active Directory 用户帐户添加到 Windows Server Essentials 仪表板（可选）](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [从源服务器 DHCP 服务器角色转为路由器](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MoveDHCP)  
  
4.  [配置的网络](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
5.  [允许用户帐户的计算机的地图](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>复制到目标服务器（可选）的数据  
 你将数据从源服务器复制到目标服务器之前，请执行以下任务：  
  
-   查看源在服务器上，包括权限的每个文件夹的共享文件夹的列表。 创建或自定义匹配要从源 Server 迁移文件夹结构目的地服务器上的文件夹。  
  
-   检查每个文件夹的大小，并确保目标服务器具有足够的存储空间。  
  
-   请源服务器上的共享的文件夹 Read-only 为时将文件复制到目标服务器，以便可以不采取任何写字用的所有用户都放置在驱动器上。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>若要将数据从源服务器复制到目标服务器  
  
1.  登录到目标服务器以管理员身份域，，然后打开命令窗口。  
  
2.  在命令提示符下，键入以下命令，，然后按 ENTER:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     位置：
     - \ < SourceServerName\ > 是源服务器的名称
     - \ < SharedSourceFolderName\ > 是源服务器上的共享文件夹的名称
     - \ < DestinationServerName\ > 是目标服务器上的名称
     - \ < SharedDestinationFolderName\ > 是数据将复制到目标服务器上的共享的文件夹。  
  
3.  对于每个要从源 Server 迁移的共享文件夹，重复上述步骤。  
  
##  <a name="BKMK_ImportADaccounts"></a>导入的 Active Directory 用户帐户添加到 Windows Server Essentials 仪表板（可选）  
 默认情况下，创建源服务器上的所有用户帐户自动都迁移到在 Windows Server Essentials 仪表板中。 但是，如果所有属性不都满足迁移要求，将无法自动迁移的 Active Directory 的用户帐户。 你可以使用下面的 Windows PowerShell cmdlet 导入的 Active Directory 的用户。  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>若要将 Active Directory 的用户帐户导入到 Windows Server Essentials 仪表板  
  
1.  域管理员身份登录到目标服务器上。  
  
2.  以 administrator 身份打开 Windows PowerShell。  
  
3.  运行以下 cmdlet，其中`[AD username]`是你想要导入的 Active Directory 用户帐户的名称：  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="BKMK_MoveDHCP"></a>从源服务器 DHCP 服务器角色转为路由器  
 如果你的源服务器运行的 DHCP 角色，执行以下步骤来进入路由器 DHCP 角色。  
  
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>若要将从源服务器 DHCP 角色移动到路由器  
  
1.  关闭 DHCP 服务源在服务器上，如下所示：  
  
    1.  在源服务器上，单击**开始**，单击**管理工具**，然后单击**服务**。  
  
    2.  在当前运行的服务的列表中，右键单击**DHCP 服务器**，然后单击**属性**。  
  
    3.  对于**开始键入**、选择**禁用**。  
  
    4.  停止的服务。  
  
2.  打开你的路由器上 DHCP 角色。  
  
    1.  按照你的路由器相关文档中的说明，若要启用 DHCP 角色在路由器上。  
  
    2.  若要确保 IP 地址发出的源服务器保持不变，按照你的路由器相关文档中的说明配置为源服务器上的 DHCP 范围相同在路由器上 DHCP 范围。  
  
        > [!IMPORTANT]
        >  如果你尚未设置在路由器上的静态 IP 或 DHCP 预订目标服务器，并 DHCP 范围不源服务器相同，则可能路由器将新的 IP 地址发出的目标服务器。 如果发生这种情况，重置端口转发规则路由器转发给目标服务器的新 IP 地址的。  
  
##  <a name="BKMK_Network"></a>配置的网络  
 后将 DHCP 角色移动到路由器上，将网络设置配置目标服务器上。  
  
#### <a name="to-configure-the-network"></a>若要配置网络  
  
1.  在目标服务器上，打开仪表板。  
  
2.  仪表板上**家庭版**页上，单击**设置**，单击**设置任何地方访问**，然后选择**单击配置任何地方访问**选项。  
  
3.  完成向导中的说明来配置你的路由器并域名。  
  
 如果你的路由器中不支持 UPnP 框架，或者 UPnP 框架已禁用，黄色警告图标可能会显示路由器名称旁边。 确保打开以下端口，他们将定向到目的地服务器的 IP 地址：  
  
-   端口 80: HTTP Web 通信  
  
-   端口 443: HTTPS Web 通信  
  
##  <a name="BKMK_MapPermittedComputers"></a>允许用户帐户的计算机的地图  
 在 Windows Server Essentials 用户必须显式分配到计算机时，它将显示在远程 Web 访问。 从 Windows Server 2008 基础迁移每个用户帐户必须将映射到一个或多个计算机。  
  
#### <a name="to-map-user-accounts-to-computers"></a>将映射到计算机的用户帐户  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，右键单击用户帐户时，，，然后单击**查看帐户属性**。  
  
4.  单击**任何地方访问**选项卡，然后单击**允许远程网站访问和 web 服务的应用程序访问**。  
  
5.  选择**共享文件夹**、选择**计算机**、选择**主页链接**，然后单击**应用**。  
  
6.  单击**计算机的访问权限**选项卡，然后单击你希望允许访问计算机的名称。  
  
7.  每个用户帐户重复步骤 3 平板电脑、4、5 和 6。  
  
> [!NOTE]
>  你不需要更改的配置的客户端计算机。 自动配置。  
  
> [!NOTE]
>  完成迁移，如果你遇到问题，当您创建目标服务器上的第一个新的用户帐户后，你添加的用户帐户中删除并重新创建。
