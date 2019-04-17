---
title: 网络文件系统概述
description: 介绍了什么是网络文件系统。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: bad0692ffd0773f61ac62bd140a421cb02c68acf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099136"
---
# 网络文件系统概述

>适用于： Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主题介绍的网络文件系统角色服务和附带 Windows Server 中的文件和存储服务服务器角色的功能。 网络文件系统 (NFS) 提供了一种文件共享解决方案为拥有异类环境，包括 Windows 和非 Windows 计算机的企业。

## 功能说明

使用 NFS 协议，可以在运行 Windows 和其他非 Windows 操作系统的系统，如 Linux 或 UNIX 计算机之间传输文件。

Windows Server 中的 NFS NFS 和 NFS 客户端包括服务器。 运行 Windows Server 的计算机可以使用 NFS 服务器充当 NFS 文件服务器的其他非 Windows 客户端计算机。 NFS 客户端允许访问的文件存储在非 Windows NFS 服务器上运行 Windows Server 的基于 Windows 的计算机。

## Windows 和 Windows Server 版本

Windows 支持多个版本的 NFS 客户端和服务器，具体取决于操作系统版本和系列。 

| 操作系统 | NFS 服务器版本 |NFS 客户端版本|
| ----------------- | ------------------- | ----------------- |
| Windows 7、 Windows 8.1、 Windows 10 | 不适用 | NFSv2 NFSv3 |
| Windows Server 2008、 Windows Server 2008 R2 | NFSv2 NFSv3 | NFSv2 NFSv3 |
| Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2016、 Windows Server 2019 | NFSv2，NFSv3，NFSv4.1  | NFSv2 NFSv3 |

## 实际应用程序

下面是你可以使用 NFS 一些方法：

- 使用 Windows NFS 文件服务器提供多协议访问相同的文件共享通过 SMB 和 NFS 协议从多平台客户端。
- 部署 Windows NFS 文件服务器提供对 NFS 文件共享的计算机访问的非 Windows 客户端在主要非 Windows 操作系统环境中。
- 可以通过 SMB 和 NFS 协议进行访问的文件共享上存储的数据迁移从一个操作系统到另一个应用程序。

## 新增功能和更改的功能

网络文件系统中的新的和更改功能包括对 NFS 版本 4.1 支持和改进的部署和管理性。 是新的或更改 Windows Server 2012 中的功能有关的信息，请参阅下表中：

|特性/功能|新功能或更新功能|说明|
|---|---|---|
|[NFS 版本 4.1](#nfs-version-4.1)|新增|而提高了的安全性、 性能和互操作性相较于 NFS 版本 3。|
|[NFS 基础结构](#nfs-infrastructure)|已更新|改进了部署和可管理性，并提高安全。|
|[NFS 版本 3 连续可用性](#nfs-version-3-continuous-availability)|已更新|提高了 NFS 版本 3 客户端上的连续可用性。|
|[部署和管理性改进](#deployment-and-manageability-improvements)|已更新|可以轻松地部署和管理 NFS 使用新的 Windows PowerShell cmdlet 和新的 WMI 提供程序。|

## NFS 版本 4.1

NFS 版本 4.1 实现所有所需的方面，除了[RFC 5661](https://tools.ietf.org/html/rfc5661)的可选方面的一些：

- **伪文件系统**，将物理和逻辑命名空间和与 NFS 版本 3 和 NFS 版本 2 兼容的文件系统。 导出的文件系统，这是伪文件系统的一部分提供别名。
- **复合 Rpc**组合相关操作，并减少闲聊。
- **会话和会话中继**支持只是一个语义，并允许连续可用性和更好的性能，同时利用 NFS 4.1 客户端和 NFS 服务器之间的多个网络。

## NFS 基础结构

下方详细介绍 Windows Server 2012 中的整体 NFS 基础结构改进：

- **远程过程调用 (RPC) / 外部数据表示 (XDR)** 传输基础结构，由 WinSock 网络协议，是适用于 NFS 这两个服务器和 NFS 客户端。 这将替代传输设备接口 (TDI)、 提供更好地支持，并提供更好的可扩展性和接收方缩放 (RSS)。
- **RPC 端口多路复用器**功能防火墙友好 （若要管理较少端口） 和简化了部署 NFS。
- **自动调整缓存和线程池**的资源管理功能的新 RPC/XDR 基础架构是动态的自动调整缓存，线程池，具体取决于工作负荷。 这将完全删除涉及的猜测时优化参数，立即部署 NFS 提供最佳性能。
- Kerberos 隐私 (Krb5p) 支持以及现有 krb5 和 krb5i 身份验证选项添加**新的 Kerberos 隐私实现和身份验证选项**。
- **标识映射 Windows PowerShell 模块**cmdlet 更加轻松地管理身份映射、 配置 Active Directory 轻型目录服务 (AD LDS)，并设置 UNIX 和 Linux 密码和平面文件。
- **卷装入点**可以访问安装在使用 NFS 版本 4.1 NFS 共享的卷。
- 该**端口复用**功能支持的 RPC 端口多路复用器 （端口 2049年），后者是防火墙友好和简化 NFS 部署。

## NFS 版本 3 连续可用性

NFS 版本 3 客户端可以快速且透明计划进行故障转移更多的可用性和减少停机时间。 故障转移过程 NFS 版本 3 客户端是速度更快，因为：

- 群集基础结构现在允许每个网络名称而不是为每个共享，从而显著提高了资源的故障转移时间的一个资源的一个资源。
- NFS 服务器中的故障转移路径已经过优化，更好的性能。
- NFS 服务器中的通配符注册不再是必需的并且更微调进行故障转移。
- 网络状态监视器 (NSM) 通知发出后故障转移过程中，并且客户端不再需要等待 TCP 超时以重新连接到故障转移服务器。

请注意 NFS 服务器支持透明故障转移仅手动启动时，通常在计划维护期间。 如果未计划故障转移发生，NFS 客户端会失去连接。 NFS 服务器还没有任何与恢复密钥筛选器的集成。 这意味着，如果本地应用或 SMB 会话会尝试访问 NFS 客户端访问计划的故障转移后立即同一个文件，NFS 客户端可能会丢失 （不会成功透明故障转移） 及其连接。

## 部署和管理性改进

部署和管理 NFS 已改进方式如下：

- 超过四十新的 Windows PowerShell cmdlet 更加轻松地配置和管理 NFS 文件共享。 有关详细信息，请参阅[Windows PowerShell 中的 NFS Cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。
- 标识映射改进了本地平面文件映射应用商店和新的 Windows PowerShell cmdlet 配置身份映射。
- 易于使用服务器管理器图形用户界面。
- 新的 WMI 版本 2 提供程序仅适用于更易于管理。
- RPC 端口多路复用器 （端口 2049年） 是防火墙友好，并简化了部署 NFS。

## 服务器管理器信息

在服务器管理器-或更高版本的[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -使用添加角色和功能向导添加 NFS 角色服务的服务器 (下的文件和 iSCSI 服务角色)。 有关安装功能的一般信息，请参阅[安装或卸载角色、 角色服务或功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>)。 NFS 工具的服务器包括的网络文件系统 MMC 管理单元中的服务来管理 nfs 服务器和客户端 NFS 组件。 使用的管理单元，你可以管理 NFS 组件在计算机上安装的服务器。 NFS 服务器还包含多个 Windows 命令行管理工具：

- **装载**本地装载远程 NFS 共享 （也称为导出），并将其映射到的 Windows 客户端计算机上的本地驱动器号。
- **Nfsadmin**管理和配置设置的服务器 NFS 客户端 NFS 组件。
- **Nfsshare**配置为共享使用用于 NFS 服务器的文件夹的 NFS 共享设置。
- **Nfsstat**显示或重置的调用 nfs 服务器接收的统计信息。
- **执行 Showmount**显示已装载的文件系统 nfs 导出的服务器。
- **卸载**删除 NFS 装载的驱动器。

NFS Windows Server 2012 中的引入了 NFS 模块 Windows PowerShell 的具有多个新 cmdlet 专门用于 NFS。 这些 cmdlet，可以方便地自动执行 NFS 管理任务。 有关详细信息，请参阅[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。

## 其他信息

下表中为评估 NFS 提供了其他资源。

|内容类型|参考|
|---|---|
|部署|[部署网络文件系统](deploy-nfs.md)|
|操作|[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|相关技术|[Windows Server 中的存储](../storage.md)|