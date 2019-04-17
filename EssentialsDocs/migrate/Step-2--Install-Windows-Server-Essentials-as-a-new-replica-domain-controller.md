---
title: "第 2 步： 安装 Windows Server Essentials 为新的副本域控制器"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 757012b7d1a57a001e3b55cdc0604b63852a3d3c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>第 2 步： 安装 Windows Server Essentials 为新的副本域控制器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

此部分中介绍了如何为域控制器安装 （具有 Windows Server Essentials 体验角色已启用） Windows Server Essentials 和 Windows Server 2012 R2 标准。  
  
 对于环境 25 最多的用户和 50 设备，可以按照本指南将从以前版本的 Windows SBS 迁移到 Windows Server Essentials 中的步骤操作。 对于环境与高达 100 用户和 200 设备，可以按照相同的指南安装了 Windows Server Essentials 体验角色迁移到标准型和 Datacenter 版本的 Windows Server 2012 R2。 本文档中介绍了这两种情况。  
  
> [!IMPORTANT]
>  如果迁移到 Windows Server Essentials，以下错误消息将添加到事件日志每一天期间 21 天的宽限期直到源服务器删除你的网络。 21 日宽限期到期后，源服务器将关闭。 <br> **FSMO 角色检查检测到一个情况，你不符合许可策略的环境中。按住管理服务器必须主域控制器和域名主机 Active Directory 角色。请现在向管理服务器移动 Active Directory 角色。此服务器将自动关闭如果该问题不更正 21 天从首先检测到这种情况的时间内**。   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>安装 Windows Server Essentials 或 Windows Server 2012 R2 Standard 目标服务器上的  
  
1.  安装 Windows Server Essentials 或 Windows Server 2012 R2 Standard 与 Windows Server Essentials 体验角色中的说明启用[安装和配置 Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  
  
    > [!NOTE]
    >  如果配置 Windows Server Essentials 向导启动，请将其取消。  
  
2.  从你的源服务器传输域控制器。  
  
    > [!NOTE]
    >  如果 Windows Server Essentials 是在域中的唯一域控制器，到服务器时降级源服务器运行 Windows Server Essentials 自动移动 FSMO 角色。  
  
3.  打开服务器管理器并运行添加角色和功能的向导。  
  
4.  如果未安装，添加了 Windows Server Essentials 体验角色。  
  
5.  安装 Windows Server Essentials 体验角色后，将配置 Windows Server Essentials 任务出现在通知区域。 单击以启动配置 Windows Server Essentials 向导中的任务。  
  
6.  按照说明完成 Windows Server Essentials 配置。 运行向导之前，请执行以下操作：  
  
    -   更改服务器名称如果需要，因为您已完成配置 Windows Server Essentials 向导后，无法更改该名称。  
  
    -   确保服务器的时间和设置正确无误。  
  
7.  验证安装，如下所示：  
  
    1.  打开的面板。  
  
    2.  单击**用户**选项卡，然后确认列出了你的 Active Directory 中的用户帐户。  
  
### <a name="transfer-the-operations-master-roles"></a>传输操作主机角色  
 操作 （也称为灵活单个主操作或 FSMO） 母版角色必须从传输源服务器到目标服务器 21 安装后天内 Windows Server Essentials 目标服务器上。  
  
##### <a name="to-transfer-the-operations-master-roles"></a>若要传输操作主机角色  
  
1.  在目标服务器上，以 administrator 身份打开 Command Prompt 窗口。 请参阅[打开 Command Prompt 窗口以管理员身份](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)。  
  
2.  在命令提示符下，键入**NETDOM 查询 FSMO**，然后按 ENTER。  
  
3.  在命令提示符下，键入**ntdsutil**，然后按 ENTER。  
  
4.  在**ntdsutil**命令提示符下，则输入以下命令：  
  
    1.  键入**激活实例 NTDS**，然后按 ENTER。  
  
    2.  键入**角色**，然后按 ENTER。  
  
    3.  键入**连接**，然后按 ENTER。  
  
    4.  键入**连接到服务器** *< ServerName\ >* (其中*< ServerName\ >*目标服务器的名称)，然后按 ENTER。  
  
    5.  在命令提示符下，键入**q**，然后按 ENTER。  
  
        1.  键入**传输 PDC**，按 enter 键，然后单击**是**中**角色传输确认**对话框。  
  
        2.  键入**传输基础主机**，按 enter 键，然后单击**是**中**角色传输确认**对话框。  
  
        3.  键入**传输命名主机**，按 enter 键，然后单击**是**中**角色传输确认**对话框。  
  
        4.  键入**传输 RID 主机**，按 enter 键，然后单击**是**中**角色传输确认**对话框。  
  
        5.  键入**传输架构主机**，按 enter 键，然后单击**是**中**角色传输确认**对话框。  
  
    6.  键入**q**，直到你返回到命令提示符，然后按 ENTER。  
  
> [!NOTE]
>  从该网络上的任何服务器，可验证操作主机角色有已传送到目标服务器。 打开 Command Prompt 窗口以管理员身份登录 (有关详细信息，请参阅[打开 Command Prompt 窗口以管理员身份](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx))。 键入**netdom 查询 fsmo**，然后按 ENTER。  
  
## <a name="next-steps"></a>后续步骤  
 您作为新的副本域控制器安装了 Windows Server Essentials。 现在转到[第 3 步： 到新的 Windows Server Essentials 服务器加入计算机](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  
  
若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

