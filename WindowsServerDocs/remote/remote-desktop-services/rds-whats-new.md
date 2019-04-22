---
title: 什么是远程桌面服务中的新增功能
description: 提供了新功能 Windows Server 2016 中的 RDS 的说明。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/11/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: ad13fdce251c1f84bac725e9f1ee266c6aae5e13
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816308"
---
# <a name="whats-new-in-remote-desktop-services"></a>什么是远程桌面服务中的新增功能

远程桌面服务 (RDS) 基于 Windows Server 2016 是虚拟化平台，使范围广泛的客户方案。 改进整体 RDS 解决方案中的包含所完成的远程桌面团队和其他技术合作伙伴在 Microsoft 工作。 以下方案和技术是新的或改进 Windows Server 2016 中。

此外请务必查看我们来自 Ignite 2016 会话：[利用 Windows Server 2016 中的 RDS 改进](https://channel9.msdn.com/Events/Ignite/2016/BRK3098)。 在此视频中，产品团队可以检查的所有远程桌面服务，包括 vGPU 支持中新增和改进功能。 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>应用程序兼容性-Windows Server 2016 和 Windows 10
Windows Server 2016 的 Windows 10 的同一个基础上构建，不仅具有相同的外观和感觉摇身一变桌面，但也可以运行许多相同的应用程序。 Windows Server 2016 配对 （如下所示） 的图形功能提供一个环境，用于所有用户可以高效工作。 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL 数据库-为高度可用环境的新数据库
RD 连接代理就能够在共享的 SQL 数据库，如 Azure SQL 数据库中存储的所有部署信息 （如连接状态和每个主机的用户映射）。 听众的 SQL Server Always On 可用性组部署手动、 获取到 Azure SQL 数据库的连接字符串和开始使用高度可用环境。

其他信息：[使用 Azure SQL DB 用于远程桌面连接代理高可用性环境](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>图形-解决图形需求跨各种方案
得益于 HYPER-V 的离散设备分配，你现在可以直接向 VM 中以供其 GPU 要求的应用程序主机计算机上映射 Gpu。 RemoteFX vGPU，包括支持 OpenGL 4.4、 OpenCL 1.1、 4 k 解析和 Windows Server 虚拟机中也已进行改进。

其他信息：[离散设备分配](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>RD 连接代理-改进登录风暴期间处理的连接
通过改进的连接的处理，RD 连接代理现在就可以处理超过 10,000 个并发登录请求，有时会出现在"登录风暴"过程。 改进的 RD 连接代理还使部署能够更简单的快速将服务器添加回该环境的维护。

其他信息：[改进了远程桌面连接代理性能](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10-内置于协议的新功能
RDP 10 现在使用 H.264/AVC 444 编解码器，适当地优化跨视频和文本。 此版本中，也支持笔远程处理。 使用这些功能，远程会话开始感觉甚至更像本地会话。  

其他信息：[Windows 10 和 Windows Server 2016 中的 RDP 10 AVC/H.264 改进](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>个人会话桌面-提供对任何最终用户的独立桌面
个人会话桌面是为你在云中托管你自己个人桌面的新方法。 管理权限和专用的会话主机中删除托管用户希望管理起来桌面环境的复杂性是其自己。

其他信息：[个人会话桌面](rds-personal-session-desktops.md)
