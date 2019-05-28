---
title: 网络文件系统概述
description: 介绍什么是网络文件系统。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9b0d339df588c784f8fe46f7dd0e6ce2975d0c48
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034651"
---
# <a name="network-file-system-overview"></a>网络文件系统概述

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍网络文件系统角色服务和 Windows Server 中的文件和存储服务服务器角色中包含的功能。 网络文件系统 (NFS) 提供了一种文件共享拥有包括 Windows 和非 Windows 计算机的异类环境的企业解决方案。

## <a name="feature-description"></a>功能说明

使用 NFS 协议时，可以运行 Windows 和其他非 Windows 操作系统的系统，如 Linux 或 UNIX 的计算机之间传输文件。

Windows Server 中的 NFS 的 NFS 和 nfs 客户端包括服务器。 运行 Windows Server 的计算机可以使用 nfs 服务器充当其他非 Windows 客户端计算机的 NFS 文件服务器。 Nfs 客户端允许访问文件存储在非 Windows NFS 服务器上运行 Windows Server 的基于 Windows 的计算机。

## <a name="windows-and-windows-server-versions"></a>Windows 和 Windows Server 版本

Windows 支持多个版本的 NFS 客户端和服务器，具体取决于操作系统版本和系列。 

| 操作系统 | NFS 服务器版本 |NFS 客户端版本|
| ----------------- | ------------------- | ----------------- |
| Windows 7，Windows 8.1，Windows 10 | 不可用 | NFSv2, NFSv3 |
| Windows Server 2008、 Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012 中，Windows Server 2012 R2、 Windows Server 2016 中，Windows Server 2019 | NFSv2, NFSv3, NFSv4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>实际应用程序

下面是可以使用 NFS 的一些方法：

- 使用 Windows NFS 文件服务器以提供多协议对访问相同的文件共享通过 SMB 和 NFS 协议从多平台客户端。
- 部署 Windows NFS 文件服务器在主要的非 Windows 操作系统环境提供非 Windows 客户端计算机访问 NFS 文件共享中。
- 通过可通过 SMB 和 NFS 协议访问文件共享上存储的数据，可将应用程序一个操作系统迁移到另一个。

## <a name="new-and-changed-functionality"></a>新增功能和更改的功能

网络文件系统中的新功能和更改功能包括支持 nfs 版本 4.1 和改进的部署和可管理性。 有关 Windows Server 2012 中新增或更改的功能的信息，请查看下表：

|特性/功能|新功能或更新功能|描述|
|---|---|---|
|[NFS 版本 4.1](#nfs-version-41)|新增|增强的安全性、 性能和互操作性相比 NFS 版本 3。|
|[NFS 基础结构](#nfs-infrastructure)|已更新|改进了部署和可管理性，并可提高安全性。|
|[NFS 版本 3 连续可用性](#nfs-version-3-continuous-availability)|已更新|改进了 NFS 版本 3 客户端上的连续可用性。|
|[部署和可管理性方面的改进](#deployment-and-manageability-improvements)|已更新|可以轻松部署和管理 NFS 与新的 Windows PowerShell cmdlet 和新的 WMI 提供程序。|

## <a name="nfs-version-41"></a>NFS 版本 4.1

NFS 版本 4.1 的实现所有所需的方面，除了一些可选方面[RFC 5661](https://tools.ietf.org/html/rfc5661):

- **虚拟文件系统**，将分离物理和逻辑命名空间，并与 NFS 版本 3 和 NFS 版本 2 兼容的文件系统。 对于导出的文件系统，这是虚拟文件系统的一部分提供别名。
- **复合 Rpc**合并相关操作和减少的通信频率。
- **会话和会话中继**启用只是一个语义，并且允许连续可用性和更好的性能，同时利用 NFS 4.1 客户端和 NFS 服务器之间的多个网络。

## <a name="nfs-infrastructure"></a>NFS 基础结构

下面详细介绍了对 Windows Server 2012 中的整体 NFS 基础结构的改进：

- **远程过程调用 (RPC) / 外部数据表示 (XDR)** WinSock 网络协议，提供支持的传输协议基础结构是可用于这两个 nfs 服务器和 nfs 客户端。 这将替代传输设备接口 (TDI)，提供更好的支持，并提供更好的可伸缩性和接收方缩放 (RSS)。
- **RPC 端口多路复用器**功能是受防火墙影响 （若要管理较少端口），并简化了部署的 NFS。
- **自动优化的缓存和线程池**是新 RPC/XDR 基础结构的管理功能，是动态的自动优化的缓存和线程池根据工作负荷的资源。 优化参数，从而提供最佳性能，一旦 NFS 部署时，这完全删除涉及的猜测。
- **新的 Kerberos 隐私实现和身份验证选项**添加了 Kerberos 隐私 (Krb5p) 支持以及现有 krb5 和 krb5i 身份验证选项。
- **标识映射 Windows PowerShell 模块**cmdlet 使其更轻松地管理标识映射、 配置 Active Directory 轻型目录服务 (AD LDS) 中，并设置 UNIX 和 Linux passwd 和平面文件。
- **卷装入点**使您能够访问安装在与 NFS 版本 4.1 的 NFS 共享下的卷。
- **端口多路复用**功能支持 RPC 端口多路复用器 （端口 2049年），这是受防火墙影响，简化了 NFS 的部署。

## <a name="nfs-version-3-continuous-availability"></a>NFS 版本 3 连续可用性

NFS 版本 3 客户端能否具有快速和透明计划的故障转移具有更多可用性和减少了停机时间。 故障转移过程的 NFS 版本 3 客户端会提高，因为：

- 聚类分析的基础结构现在允许每个网络名称，而不是每个共享，从而显著提高了资源的故障转移时间的一个资源的一个资源。
- NFS 服务器中的故障转移路径进行优化，更好的性能。
- 在 NFS 服务器中的通配符注册不再是必需的并且更细微调整故障转移。
- 网络状态监视器 (NSM) 通知发出后故障转移过程，并且客户端不再需要的 TCP 超时等待重新连接到故障转移服务器。

请注意，NFS 服务器仅当手动启动时，且通常是在计划的维护期间支持透明的故障转移。 如果发生计划外故障转移，NFS 客户端会失去连接。 Nfs 服务器还没有与恢复密钥筛选器的任何集成。 这意味着，如果本地应用或 SMB 会话尝试在一个计划的故障转移之后立即访问一个 NFS 客户端同时正在访问的文件，则该 NFS 客户端可能会失去其连接（透明故障转移将失败）。

## <a name="deployment-and-manageability-improvements"></a>部署和可管理性方面的改进

通过以下方式改进了部署和管理 NFS:

- 超过四十新的 Windows PowerShell cmdlet，使其易于配置和管理 NFS 文件共享。 有关详细信息，请参阅[Windows PowerShell 中的 NFS Cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。
- 标识映射得到改进本地平面文件映射应用商店和新的 Windows PowerShell cmdlet 配置标识映射。
- 服务器管理器图形用户界面是易于使用。
- 新的 WMI 版本 2 提供程序是可用于更轻松地管理。
- RPC 端口多路复用器 （端口 2049年） 是受防火墙影响，并简化了部署的 NFS。

## <a name="server-manager-information"></a>服务器管理器信息

在服务器管理器-或更高版本[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -使用添加角色和功能向导来添加 NFS 服务角色服务的服务器 (在文件和 iSCSI 服务角色)。 有关安装功能的详细信息，请参阅 [“安装或卸载角色、角色服务或功能”](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>)。 服务器 NFS 工具包括网络文件系统 MMC 管理单元中的服务来管理 nfs 服务器和 NFS 组件的客户端。 使用管理单元，可以管理的计算机上安装 NFS 组件的服务器。 Nfs 服务器还包含几个 Windows 命令行管理工具：

- **装载**本地装载一个远程的 NFS 共享 （也称为导出），并将其映射到 Windows 客户端计算机上的本地驱动器号。
- **Nfsadmin**管理和配置设置的服务器的 NFS 客户端 NFS 组件。
- **Nfsshare**配置使用 nfs 服务器共享文件夹的 NFS 共享设置。
- **Nfsstat**显示或重置的调用中的 NFS 服务器接收的统计信息。
- **Showmount**显示 nfs 服务器导出的已装载的文件系统。
- **Umount**删除 NFS 装载的驱动器。

Windows Server 2012 中的 NFS 引入了 NFS 模块用于 Windows PowerShell 具有几个新 cmdlet 专为 NFS。 这些 cmdlet 可以方便地自动执行 NFS 管理任务。 有关详细信息，请参阅[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。

## <a name="additional-information"></a>其他信息

下表提供有关评估 NFS 的其他资源。

|内容类型|参考|
|---|---|
|部署|[部署网络文件系统](deploy-nfs.md)|
|操作|[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|相关技术|[Windows Server 中存储](../storage.md)|