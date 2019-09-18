---
title: 创建远程桌面服务集合
description: 了解如何将 RDSH 和 RemoteApp 程序添加到 RDS 部署中。
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
ms.openlocfilehash: ca43a37bff28a035d9f7292da830a23ca29d23bc
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871035"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>创建供桌面和应用运行的远程桌面服务集合

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

使用以下步骤创建远程桌面服务会话集合。 会话集合包含你希望提供给用户的应用和桌面。 创建集合后，发布它，以便用户可以访问它。

在创建集合之前，需确定需要哪种集合：共用桌面会话还是个人桌面会话。 

- **为基于会话的虚拟化使用共用桌面会话**：利用 Windows Server 的计算能力提供经济高效的多会话环境，以驱动用户的日常工作负载
- **使用个人桌面会话创建虚拟桌面基础结构 (VDI)** ：利用 Windows 客户端为用户提供他们在 Windows 桌面体验中所预期的高性能、应用兼容性和顺手性。
 
使用共用会话时，多个用户访问共享的资源池，而使用个人桌面会话时，将从池中向用户分配其自己的桌面。 共用会话提供较低的总成本，而个人会话使用户能够自定义其桌面体验。

如果需要共享图形密集型托管应用程序，可以将个人会话桌面与为图形加速配置的 RemoteFX vGPU 组合起来。 或者，也可以将个人会话桌面与新的离散设备分配 (DDA) 功能相结合，为需要加速图形的托管应用程序提供支持。 有关详细信息，请参阅[哪种图形虚拟化技术适合你](rds-graphics-virtualization.md)。


无论选择哪种类型的集合，都将使用 RemoteApps 填充这些集合 - 用户可以从任何受支持的设备访问这些程序和资源，并且像在本地运行程序那样使用它们。

## <a name="create-a-pooled-desktop-session-collection"></a>创建一个共用桌面会话集合

1.  在“服务器管理器”中，单击“远程桌面服务”>“集合”>“任务”>“创建会话集合”  。  
2.  输入集合名称，例如“ContosoAps”  。  
3.  选择创建的 RD 会话主机服务器（例如，Contoso-Shr1）。  
4.  接受默认“用户组”  。  
5.  输入为该集合的用户配置文件磁盘创建的文件共享的位置（例如，\Contoso-Cb1\UserDisksr  ）。   
6.  单击“创建”  。 创建集合后，单击“关闭”  。  


## <a name="create-a-personal-desktop-session-collection"></a>创建个人桌面会话集合

使用 New-RDSessionCollection cmdlet 创建个人会话桌面集合。 以下三个参数提供个人会话桌面所需的配置信息：

- **-PersonalUnmanaged** - 指定可用于将用户分配到个人会话主机服务器的会话集合类型。 如果未指定此参数，则会以传统 RD 会话主机集合的形式创建集合，其中用户在登录时将分配到下一个可用的会话主机。
- **-GrantAdministrativePrivilege** - 如果使用了 -PersonalUnmanaged  ，则此参数会指定要为分配到会话主机的用户授予管理特权。 如果不使用此参数，则只会为用户授予标准用户权限。
- **-AutoAssignUser** - 如果使用了 -PersonalUnmanaged  ，则此参数指定要将通过 RD 连接代理进行连接的新用户自动分配到某个未分配的会话主机。 如果集合中没有任何未分配的会话主机，则用户会看到错误消息。 如果不使用此参数，则必须在用户登录之前[手动将用户分配到会话主机](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host)。

可以使用 PowerShell cmdlet 来管理个人桌面会话集合。 有关详细信息，请参阅[管理个人桌面会话集合](rds-manage-personal-collection.md)。

## <a name="publish-remoteapp-programs"></a>发布 RemoteApp 程序
使用以下步骤在集合中发布应用和资源：

1.  在“服务器管理器”中，选择新集合 (ContosoApps  )。  
2.  在 RemoteApp 程序下，单击“发布 RemoteApp 程序”  。  
3. 选择要发布的程序，然后单击“发布”  。  
