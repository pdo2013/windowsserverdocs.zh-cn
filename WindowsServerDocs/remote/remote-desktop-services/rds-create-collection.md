---
title: 创建远程桌面服务集合
description: 了解如何添加和到 RDS 部署 RDSH 和 RemoteApp 程序。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 52806e28c4ef87453995728623efe2954a76dfd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839238"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>创建适用于桌面和应用程序以运行远程桌面服务集合

>适用于：Windows 服务器 （半年频道），Windows Server 2016

使用以下步骤来创建远程桌面服务会话集合。 会话集合中包含的应用和你想要将提供给用户的桌面。 创建集合后，将发布它，以便用户可以访问它。

创建集合之前，需要先确定需要哪种集合： 共用桌面会话或个人桌面会话。 

- **基于会话的虚拟化使用共用桌面会话**:利用 Windows Server 的计算能力，以提供经济高效的多会话环境来驱动用户的日常工作负荷
- **使用个人桌面会话来创建虚拟桌面基础结构 (VDI)**:利用 Windows 客户端提供的高性能、 应用程序兼容性和用户都希望能其 Windows 桌面体验的熟悉程度。
 
使用已入池的会话中，多个用户访问的共享的池的资源，而与个人桌面会话，用户都分配有自己从池内的桌面。 共用的会话提供较低的总成本，而个人会话使用户能够自定义其桌面的体验。

如果需要图形占用承载共享应用程序，您可以将与 RemoteFX vGPU 为图形加速功能配置个人会话桌面。 或者，您可以将个人会话桌面与新的离散设备分配 (DDA) 功能，还需要加速的图形的托管应用程序提供支持。 请查看[图形虚拟化技术最适合您](rds-graphics-virtualization.md)有关详细信息。


无论你选择的集合的类型，将填充 RemoteApps 的计划和资源，用户可以从任何受支持的设备访问和使用，就好像已本地运行该程序使用这些集合。

## <a name="create-a-pooled-desktop-session-collection"></a>创建一个共用桌面会话集合

1.  在服务器管理器中，单击**远程桌面服务 > 集合 > 任务 > 创建会话集合**。  
2.  输入集合的名称，例如**ContosoAps**。  
3.  选择 （例如，Contoso Shr1） 创建的 RD 会话主机服务器。  
4.  接受默认值**用户组**。  
5.  输入为此集合的用户配置文件磁盘创建的文件共享的位置 (例如， **\Contoso-Cb1\UserDisksr**)。   
6.  单击“创建”。 创建集合后，单击**关闭**。  


## <a name="create-a-personal-desktop-session-collection"></a>创建个人桌面会话集合

新建 RDSessionCollection cmdlet 用于创建个人会话桌面集合。 以下三个参数提供适用于个人会话桌面所需的配置信息：

- **-PersonalUnmanaged** -指定的会话集合，可通过该类型将用户分配到个人会话主机服务器。 如果未指定此参数，为传统的 RD 会话主机集合，其中用户在登录时将分配给下一个可用的会话主机创建集合。
- **-GrantAdministrativePrivilege** -如果使用 **-PersonalUnmanaged**，指定分配给会话主机的用户被授予管理权限。 如果不使用此参数，只是标准用户权限将授予用户。
- **-AutoAssignUser** -如果使用 **-PersonalUnmanaged**，指定通过 RD 连接代理连接的新用户将自动分配给未分配的会话主机。 如果在集合中没有未分配的会话主机，用户将看到一条错误消息。 如果不使用此参数，必须[手动将用户分配到会话主机](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host)他们在登录之前。

可以使用 PowerShell cmdlet 来管理您的个人桌面会话集合。 请参阅[管理您的个人桌面会话集合](rds-manage-personal-collection.md)有关详细信息。

## <a name="publish-remoteapp-programs"></a>发布 RemoteApp 程序
使用以下步骤在集合中的发布的应用和资源：

1.  在服务器管理器中，选择新的集合 (**ContosoApps**)。  
2.  在 RemoteApp 程序下，单击**发布 RemoteApp 程序**。  
3. 选择你想要发布，然后单击的程序**发布**。  
