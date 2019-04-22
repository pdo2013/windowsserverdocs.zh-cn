---
title: 步骤 2：Windows Server Essentials 安装为新副本域控制器
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816458"
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>步骤 2：Windows Server Essentials 安装为新副本域控制器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本部分介绍如何使用来安装 Windows Server Essentials 和 Windows Server 2012 R2 Standard （已启用 Windows Server Essentials 体验角色） 作为域控制器。  
  
 对于具有最多 25 个用户和 50 台设备的环境，你可以按照此指南从早期版本的 Windows SBS 迁移到 Windows Server Essentials 中的步骤。 对于环境中使用最多 100 个用户和 200 台设备，你可以按照相同的指南迁移到 Windows Server 2012 R2 Standard 和 Datacenter 版本安装了 Windows Server Essentials 体验角色。 本文档将对这两种方案进行介绍。  
  
> [!IMPORTANT]
>  如果迁移到 Windows Server Essentials 时，以下错误消息添加到事件日志在 21 天宽限期内直到从网络删除源服务器的每一天。 在 21 天的宽限期之后，源服务器将关闭。 <br> **FSMO 角色检查检测到不符合授权策略的环境中的条件。管理服务器必须包含主域控制器以及域命名主机 Active Directory 角色。请立即将 Active Directory 角色移到管理服务器。此服务器将自动关机如果首次检测这种情况的时间从 21 天内没有纠正该问题**。   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>在目标服务器上安装 Windows Server Essentials 或 Windows Server 2012 R2 标准版  
  
1.  按照中的说明已启用 Windows Server Essentials 体验角色安装 Windows Server Essentials 或 Windows Server 2012 R2 Standard[安装和配置 Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  
  
    > [!NOTE]
    >  如果启动了“配置 Windows Server Essentials”向导，请取消它。  
  
2.  从源服务器传送 FSMO 角色。  
  
    > [!NOTE]
    >  如果 Windows Server Essentials 域中唯一的域控制器，FSMO 角色将自动移动到源服务器降级时运行 Windows Server Essentials 的服务器。  
  
3.  打开服务器管理器，然后运行“添加角色和功能”向导。  
  
4.  如果未安装，请添加 Windows Server Essentials 体验角色。  
  
5.  安装 Windows Server Essentials 体验角色后，配置 Windows Server Essentials 任务将出现在通知区域。 单击该任务以启动“配置 Windows Server Essentials”向导。  
  
6.  按照说明完成 Windows Server Essentials 的配置。 在运行该向导之前，请执行以下操作：  
  
    -   更改服务器名称（如果需要），因为完成“配置 Windows Server Essentials”向导之后，无法再更改该名称。  
  
    -   请确保服务器的时间和设置正确。  
  
7.  验证安装，如下所示：  
  
    1.  打开“仪表板”。  
  
    2.  单击“用户”  选项卡，并验证是否列出了 Active Directory 中的用户帐户。  
  
### <a name="transfer-the-operations-master-roles"></a>传送操作主机角色  
 操作主机 （也称为灵活单主机操作或 FSMO） 角色必须是源服务器内传送到目标服务器的目标服务器上安装 Windows Server Essentials 的 21 天。  
  
##### <a name="to-transfer-the-operations-master-roles"></a>传送操作主机角色  
  
1.  在目标服务器上，以管理员身份打开命令提示符窗口。 请参阅 [以管理员身份打开“命令提示符”窗口](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)。  
  
2.  在命令提示符下，键入 **NETDOM QUERY FSMO**，然后按 ENTER。  
  
3.  在命令提示符下，键入 **ntdsutil**，然后按 ENTER。  
  
4.  在 **ntdsutil** 命令提示符下，输入以下命令：  
  
    1.  键入 **activate instance NTDS**，然后按 Enter 键。  
  
    2.  键入 **roles**，然后按 ENTER。  
  
    3.  键入 **connection**，然后按 ENTER。  
  
    4.  类型**连接到服务器** *< 服务器名\>* (其中 *< 服务器名\>* 是目标服务器的名称)，然后按 ENTER。  
  
    5.  在命令提示符处键入 **q**，然后按 ENTER。  
  
        1.  键入 **transfer PDC**，按 ENTER，然后在“角色传送确认”对话框中单击“是”。  
  
        2.  键入 **transfer infrastructure master**，按 ENTER，然后在“角色传送确认”对话框中单击“是”。  
  
        3.  键入 **transfer naming master**，按 ENTER，然后在“角色传送确认”  对话框中单击“是”  。  
  
        4.  键入 **transfer RID master**，按 ENTER，然后在“角色传送确认”对话框中单击“是”。  
  
        5.  键入 **transfer schema master**，按 ENTER，然后在“角色传送确认”  对话框中单击“是”  。  
  
    6.  键入 **q**，然后一直按 ENTER，直到返回命令提示符为止。  
  
> [!NOTE]
>  你可以从网络上的任意服务器中，确认操作主机角色已传送到目标服务器。 以管理员身份打开命令提示符窗口（有关详细信息，请参阅 [以管理员身份打开命令提示符窗口](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)）。 键入 **netdom query fsmo**，然后按 ENTER。  
  
## <a name="next-steps"></a>后续步骤  
 为新副本域控制器，你已安装 Windows Server Essentials。 现在，转到[步骤 3:将计算机加入到新的 Windows Server Essentials 服务器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  
  
若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

