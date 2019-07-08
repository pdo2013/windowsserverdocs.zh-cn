---
title: 远程桌面服务中的新增功能
description: 介绍 Windows Server 2016 中的 RDS 新功能。
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63711797"
---
# <a name="whats-new-in-remote-desktop-services"></a>远程桌面服务中的新增功能

构建在 Windows Server 2016 基础之上的远程桌面服务 (RDS) 是一个虚拟化平台，它可以实现多种客户方案。 整体 RDS 解决方案的改进融入了远程桌面团队和 Microsoft 的其他技术合作伙伴的工作。 下面是 Windows Server 2016 中新增或改进的方案和技术。

另外，请务必查看 Ignite 2016 年会议的成果：[利用 Windows Server 2016 中的 RDS 改进](https://channel9.msdn.com/Events/Ignite/2016/BRK3098)。 在此视频中，产品团队评述了远程桌面服务的所有新增功能和改进功能，包括 vGPU 支持。 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>应用程序兼容性 - Windows Server 2016 和 Windows 10
Windows Server 2016 构建在 Windows 10 的相同基础之上，它不仅提供用户预期的桌面外观，而且还能运行 Windows 10 中可运行的许多相同应用程序。 将 Windows Server 2016 与图形功能（如下所述）搭配使用可以提供一个可让所有用户保持高效工作的环境。 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL 数据库 - 适用于高可用性环境的新数据库
RD 连接代理能够在共享的 SQL 数据库（例如 Azure SQL 数据库）中存储所有部署信息（例如连接状态和用户/主机映射）。 可以丢弃 SQL Server Always On 可用性组部署手册，只需获取 Azure SQL 数据库的连接字符串，然后即可开始使用高可用性环境。

其他信息：[将 Azure SQL 数据库用于远程桌面连接代理高可用性环境](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>图形 - 解决不同方案中的图形需求
得益于 Hyper-V 的离散设备分配，现在你可以直接将主机上的 GPU 映射到 VM，供 GPU 消耗型应用程序使用。 RemoteFX vGPU 也已获得改进，包括支持 OpenGL 4.4、OpenCL 1.1、4K 分辨率和 Windows Server 虚拟机。

其他信息：[离散设备分配](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>RD 连接代理 - 改进了登录风暴期间的连接处理
由于连接处理已得到改进，RD 连接代理现在能够处理 10,000 个以上的并发登录请求（有时，在“登录风暴”期间会出现这么多的请求）。 改进的 RD 连接代理还可以快速将服务器添加回到环境中，从而简化了部署的维护。

其他信息：[改进的远程桌面连接代理性能](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 - 协议中内置的新功能
RDP 10 现在使用 H.264/AVC 444 编解码器，可以适当地对视频和文本进行优化。 此版本还支持笔的远程控制。 使用这些功能可使远程会话变得更像是本地会话。  

其他信息：[Windows 10 和 Windows Server 2016 中的 RDP 10 AVC/H.264 改进](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>个人会话桌面 - 为任何最终用户提供单独的桌面
个人会话桌面是在云中托管自己的个人桌面的新方式。 管理特权和专用会话主机消除了在托管环境中让用户随心管理桌面的复杂性。

其他信息：[个人会话桌面](rds-personal-session-desktops.md)
