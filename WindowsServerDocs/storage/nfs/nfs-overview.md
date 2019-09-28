---
title: 网络文件系统概述
description: 说明什么是网络文件系统。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 72f71bc6605103f8240bcd531da3a5b58d470181
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403050"
---
# <a name="network-file-system-overview"></a>网络文件系统概述

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍 Windows Server 中的文件和存储服务服务器角色随附的网络文件系统角色服务和功能。 网络文件系统（NFS）为具有异构环境（包括 Windows 和非 Windows 计算机）的企业提供文件共享解决方案。

## <a name="feature-description"></a>功能说明

使用 NFS 协议，你可以在运行 Windows 的计算机与其他非 Windows 操作系统（如 Linux 或 UNIX）之间传输文件。

Windows Server 中的 NFS 包括 NFS 服务器和 NFS 客户端。 运行 Windows Server 的计算机可以使用 NFS 服务器作为其他非 Windows 客户端计算机的 NFS 文件服务器。 NFS 客户端允许运行 Windows Server 的基于 Windows 的计算机访问非 Windows NFS 服务器上存储的文件。

## <a name="windows-and-windows-server-versions"></a>Windows 和 Windows Server 版本

Windows 支持多个版本的 NFS 客户端和服务器，具体取决于操作系统版本和系列。 

| 操作系统 | NFS 服务器版本 |NFS 客户端版本|
| ----------------- | ------------------- | ----------------- |
| Windows 7、Windows 8.1、Windows 10 | 不可用 | NFSv2、NFSv3 |
| Windows Server 2008、Windows Server 2008 R2 | NFSv2、NFSv3 | NFSv2、NFSv3 |
| Windows Server 2012，Windows Server 2012 R2，Windows Server 2016，Windows Server 2019 | NFSv2、NFSv3、NFSv 4。1  | NFSv2、NFSv3 |

## <a name="practical-applications"></a>实际应用程序

可以通过以下方法使用 NFS：

- 使用 Windows NFS 文件服务器为多平台客户端上的 SMB 和 NFS 协议提供对同一文件共享的多协议访问。
- 在主要非 Windows 操作系统环境中部署 Windows NFS 文件服务器，以提供对 NFS 文件共享的非 Windows 客户端计算机的访问权限。
- 通过将数据存储在可通过 SMB 和 NFS 协议访问的文件共享上，将应用程序从一个操作系统迁移到另一个操作系统。

## <a name="new-and-changed-functionality"></a>新增功能和更改的功能

网络文件系统中的新功能和更改的功能包括对 NFS 版本4.1 的支持以及改进的部署和可管理性。 有关 Windows Server 2012 中新增或更改的功能的信息，请参阅下表：

|特性/功能|新功能或更新功能|描述|
|---|---|---|
|[NFS 版本4。1](#nfs-version-41)|新增|与 NFS 版本3相比，安全性、性能和互操作性更高。|
|[NFS 基础结构](#nfs-infrastructure)|已更新|提高部署和可管理性，并提高安全性。|
|[NFS 版本3连续可用性](#nfs-version-3-continuous-availability)|已更新|提高 NFS 版本3客户端的持续可用性。|
|[部署和可管理性改进](#deployment-and-manageability-improvements)|已更新|使你能够使用新的 Windows PowerShell cmdlet 和新的 WMI 提供程序轻松部署和管理 NFS。|

## <a name="nfs-version-41"></a>NFS 版本4。1

NFS 版本4.1 除了提供一些可选方面之外，还实现了[RFC 5661](https://tools.ietf.org/html/rfc5661)的所有必需方面：

- **伪文件系统**，一种分隔物理和逻辑命名空间并与 nfs 版本3和 nfs 版本2兼容的文件系统。 为导出的文件系统提供别名，该文件系统是伪文件系统的一部分。
- **复合 rpc**合并了相关操作并减少了交互成本。
- **会话和会话中继**只启用了一个语义，可在 nfs 4.1 客户端和 nfs 服务器之间使用多个网络时，实现持续的可用性和更好的性能。

## <a name="nfs-infrastructure"></a>NFS 基础结构

下面详细介绍了对 Windows Server 2012 中的总体 NFS 基础结构的改进：

- **远程过程调用（RPC）/External 数据表示（XDR）** 传输基础结构由 WinSock 网络协议提供支持，可用于 nfs 服务器和 Nfs 客户端。 这替代了传输设备接口（TDI），提供了更好的支持，并提供了更好的可伸缩性和接收方缩放（RSS）。
- **RPC 端口多路复**用器功能适合于防火墙（管理更少的端口）并简化 NFS 的部署。
- **自动优化缓存和线程池**是新的 RPC/XDR 基础结构的资源管理功能，它是动态的，可根据工作负荷自动优化缓存和线程池。 这完全消除了优化参数时所涉及的猜测，并在部署 NFS 后提供最佳性能。
- **新增了 kerberos 隐私实现和身份验证选项**，同时添加了 kerberos 隐私（Krb5p）支持以及现有的 krb5.conf 和 krb5i 身份验证选项。
- 通过**标识映射，Windows PowerShell 模块**cmdlet 可以更轻松地管理标识映射、配置 Active Directory 轻型目录服务（AD LDS），以及设置 UNIX 和 Linux 密码和平面文件。
- **卷装入点**允许你使用 nfs 版本4.1 访问在 nfs 共享下装载的卷。
- **端口多路复**用功能支持 RPC 端口多路复用器（端口2049），这是防火墙友好的，可简化 NFS 部署。

## <a name="nfs-version-3-continuous-availability"></a>NFS 版本3连续可用性

NFS 版本3客户端可以具有快速、透明的计划内故障转移，并提供更高的可用性和缩短停机时间。 NFS 版本3客户端的故障转移过程更快，因为：

- 群集基础结构现在允许每个网络名称有一个资源，而不是每个共享一个资源，这大大缩短了资源的故障转移时间。
- 对 NFS 服务器内的故障转移路径进行优化，以获得更好的性能。
- 不再需要 NFS 服务器中的通配符注册，并且故障转移更精细。
- 网络状态监视器（NSM）通知将在故障转移过程后发出，客户端不再需要等待 TCP 超时重新连接到故障转移的服务器。

请注意，NFS 服务器仅当手动启动时，且通常是在计划的维护期间支持透明的故障转移。 如果发生计划外故障转移，NFS 客户端会失去连接。 NFS 服务器还不与 Resume 密钥筛选器集成。 这意味着，如果本地应用或 SMB 会话尝试在一个计划的故障转移之后立即访问一个 NFS 客户端同时正在访问的文件，则该 NFS 客户端可能会失去其连接（透明故障转移将失败）。

## <a name="deployment-and-manageability-improvements"></a>部署和可管理性改进

部署和管理 NFS 的方式已通过以下方式改进：

- 超过40个新的 Windows PowerShell cmdlet 可让你更轻松地配置和管理 NFS 文件共享。 有关详细信息，请参阅[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。
- 使用本地平面文件映射存储和用于配置标识映射的新 Windows PowerShell cmdlet 改进标识映射。
- 服务器管理器图形用户界面更易于使用。
- 新的 WMI 版本2提供程序可用于更轻松地进行管理。
- RPC 端口多路复用器（端口2049）易于防火墙并简化 NFS 的部署。

## <a name="server-manager-information"></a>服务器管理器信息

在服务器管理器或更高版本的[Windows 管理中心](../../manage/windows-admin-center/understand/windows-admin-center.md)中-使用 "添加角色和功能向导" 添加 "NFS 服务器" 角色服务（在 "文件和 iSCSI 服务" 角色下）。 有关安装功能的详细信息，请参阅 [“安装或卸载角色、角色服务或功能”](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>)。 NFS 服务器工具包括用于网络文件系统的服务 MMC 管理单元，用于管理 NFS 服务器和 NFS 客户端组件。 使用管理单元，你可以管理安装在计算机上的 NFS 服务器组件。 NFS 服务器还包含多个 Windows 命令行管理工具：

- **装载**在本地装载远程 NFS 共享（也称为导出），并将其映射到 Windows 客户端计算机上的本地驱动器号。
- **Nfsadmin**管理 nfs 服务器和 Nfs 客户端组件的配置设置。
- **Nfsshare**为使用 nfs 服务器共享的文件夹配置 NFS 共享设置。
- **Nfsstat**显示或重置 NFS 服务器收到的调用的统计信息。
- **Showmount**显示 nfs 服务器导出的已装载文件系统。
- **卸载**删除 NFS 装载的驱动器。

Windows Server 2012 中的 NFS 为适用于 Windows PowerShell 的 nfs 模块引入了专门用于 NFS 的多个新 cmdlet。 这些 cmdlet 提供了一种简单的方法来自动执行 NFS 管理任务。 有关详细信息，请参阅[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。

## <a name="additional-information"></a>其他信息

下表提供了用于评估 NFS 的其他资源。

|内容类型|参考资料|
|---|---|
|部署|[部署网络文件系统](deploy-nfs.md)|
|操作|[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|相关技术|[Windows Server 中的存储](../storage.md)|