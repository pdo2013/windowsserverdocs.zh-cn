---
title: 管理服务器与 Windows Admin Center
description: 使用 Windows Admin Center (项目 Honolulu) 管理服务器
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 04ade4a14272c7840b5036ca6ad5a3bf3d09bcf9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891148"
---
# <a name="manage-servers-with-windows-admin-center"></a>管理服务器与 Windows Admin Center

>适用于：Windows Admin Center，Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="managing-windows-server-machines"></a>管理 Windows Server 计算机

您可以添加单个服务器运行 Windows Server 2012 或更高版本到 Windows Admin Center 来管理服务器通过一组全面的工具包括证书、 设备、 事件、 过程、 角色和功能、 更新、 虚拟机等。

![服务器连接概述屏幕](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>将服务器添加到 Windows Admin Center

若要将服务器添加到 Windows Admin Center:

1. 单击 **+ 添加**下所有连接。
2. 选择添加**服务器连接**。
3. 键入的服务器的名称，如果系统提示，要使用的凭据。
4. 单击**提交**来完成。

服务器将添加到您在概述页上的连接列表。 单击它以连接到服务器。

> [!NOTE]
> 您还可以添加[故障转移群集](manage-failover-clusters.md)或[超聚合群集](manage-hyper-converged.md)作为 Windows Admin Center 在一个单独的连接。

## <a name="tools"></a>工具

以下工具是可用于服务器连接：

| Tool | 描述 |
| ---- | ----------- |
| [概述](#overview) | 查看服务器详细信息和控制服务器状态 |
| [备份](#backup) | 查看和配置 Azure 备份 |  
| [证书](#certificates) | 查看和修改的证书 |
| [容器](#containers) | 查看容器 |
| [设备](#devices) | 查看和修改设备 |
| [事件](#events) | 查看事件 |
| [文件](#files) | 浏览文件和文件夹 |
| [防火墙](#firewall) | 查看和修改防火墙规则 |
| [已安装的应用](#installed-apps) | 查看和删除已安装的应用 |
| [本地用户和组](#local-users-and-groups) | 查看和修改本地用户和组 |
| [网络](#network) | 查看和修改网络设备 |
| [PowerShell](#powershell) | 通过 PowerShell 与服务器进行交互 |
| [进程](#processes) | 查看和修改正在运行的进程 |
| [Registry](#registry) | 查看和修改注册表项 |
| [远程桌面](#remote-desktop) | 通过远程桌面与服务器进行交互 |
| [角色和功能](#roles-and-features) | 查看和修改角色和功能 |
| [计划的任务](#scheduled-tasks) | 查看和修改计划的任务 |
| [服务](#services) | 查看和修改服务 |
| [存储](#storage) | 查看和修改存储设备 |
| [存储迁移服务](#storage-migration-service) | 将服务器和文件共享迁移到 Azure 或 Windows Server 2019 |
| [存储副本](#storage-replica) | 使用存储副本来管理服务器到服务器存储复制 |
| [系统见解](#system-insights) | 系统 Insights 将提高深入了解你的服务器的正常运行。 |
| [更新](#updates) | 查看已安装并检查新更新 |
| [虚拟机](manage-virtual-machines.md) | 查看和管理虚拟机 |
| [虚拟交换机](#virtual-switches) | 查看和管理虚拟交换机 |

## <a name="overview"></a>概述

**概述**，您可以查看 CPU、 内存和网络性能的当前状态以及作为执行操作和修改目标计算机或服务器上的设置。

### <a name="features"></a>功能

在服务器管理器概述中支持以下功能：

- 查看服务器详细信息
- 查看 CPU 活动
- 查看内存活动
- 查看网络活动
- 重新启动服务器
- 关闭服务器
- 启用服务器上的磁盘指标
- 在服务器上编辑计算机 ID

[**查看反馈和建议的功能的服务器概述**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D)。

## <a name="backup"></a>备份

**备份**，可通过直接向 Microsoft Azure 备份服务器保护 Windows 服务器免受损坏、 攻击或灾难的影响。
[了解有关 Azure 备份的详细信息。](https://aka.ms/windows-admin-center-backup)

[提供 Windows Admin Center 中的备份的反馈](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>功能

在备份过程中支持以下功能：

- 查看 Azure 的备份状态的概述
- 配置备份项和计划
- 启动或停止备份作业
- 查看备份作业历史记录和状态
- 查看恢复点和恢复数据
- 删除备份数据

## <a name="certificates"></a>证书

**证书**允许你管理的计算机或服务器上的证书存储。

### <a name="features"></a>功能

在证书中支持以下功能：

- 浏览和搜索现有证书
- 查看证书详细信息
- 导出证书
- 续订证书
- 请求新证书
- 删除证书

[**查看证书的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D)。

## <a name="containers"></a>容器

**容器**允许您查看 Windows Server 容器主机上的容器。 在正在运行的 Windows Server Core 容器的情况下可以查看事件日志，以及访问容器的 CLI。

[**查看容器的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D)。

## <a name="devices"></a>设备

**设备**允许你管理的计算机或服务器上的已连接的设备。

### <a name="features"></a>功能

在设备中支持以下功能：

- 浏览和搜索设备
- 查看设备详细信息
- 禁用设备
- 更新设备上的驱动程序

[**查看设备的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D)。

## <a name="events"></a>事件

**事件**允许你管理的计算机或服务器上的事件日志。

### <a name="features"></a>功能

在事件中支持以下功能：

- 浏览和搜索事件
- 查看事件详细信息
- 清除事件日志中
- 从日志中导出事件

[**查看事件的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D)。

## <a name="files"></a>文件

**文件**允许你管理的计算机或服务器上文件和文件夹。

### <a name="features"></a>功能

在文件中支持以下功能：

- 浏览文件和文件夹
- 搜索文件或文件夹
- 创建一个新文件夹
- 删除文件或文件夹
- 下载文件或文件夹
- 上传文件或文件夹
- 重命名文件或文件夹
- 提取 zip 文件
- 查看文件或文件夹属性
- 管理文件服务器资源管理器[配额](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)
- 添加、 编辑或删除文件共享
- 修改用户和组权限的文件共享上

[**查看文件的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D)。

## <a name="firewall"></a>防火墙

**防火墙**允许你管理防火墙设置和计算机或服务器上的规则。

### <a name="features"></a>功能

在防火墙中支持以下功能：

- 查看防火墙设置的概述
- 查看传入的防火墙规则
- 查看传出防火墙规则
- 搜索防火墙规则
- 查看防火墙规则详细信息
- 创建新的防火墙规则
- 启用或禁用防火墙规则
- 删除防火墙规则
- 编辑防火墙规则的属性

[**查看防火墙的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D)。

## <a name="installed-apps"></a>已安装的应用

**已安装的应用**可以列出并卸载已安装的应用程序。

[**查看已安装的应用的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D)。

## <a name="local-users-and-groups"></a>本地用户和组

**本地用户和组**，可管理安全组和存在于本地计算机或服务器的用户。

### <a name="features"></a>功能

本地用户和组中支持以下功能：

- 查看和搜索用户和组
- 创建新用户或组
- 管理用户的组成员身份
- 删除用户或组
- 更改用户的密码
- 编辑用户或组的属性

[**查看本地用户和组的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>网络

**网络**允许你管理网络设备和计算机或服务器上的设置。

### <a name="features"></a>功能

在网络中支持以下功能：

- 浏览和搜索现有的网络适配器
- 查看网络适配器的详细信息
- 编辑网络适配器的属性
- 创建[Azure 网络适配器 （预览功能）](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**查看网络的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell**允许您与计算机或通过 PowerShell 会话的服务器进行交互。

### <a name="features"></a>功能

在 PowerShell 中支持以下功能：

- 在服务器上创建交互式 PowerShell 会话
- 断开与服务器上的 PowerShell 会话的连接

[**查看 PowerShell 的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>进程

**进程**允许你管理的计算机或服务器上运行的进程。

### <a name="features"></a>功能

在进程中支持以下功能：

- 浏览和搜索正在运行的进程
- 查看进程详细信息
- 启动进程
- 结束进程
- 创建进程转储
- 查找进程句柄

[**查看进程的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D)。

## <a name="registry"></a>注册表

**注册表**允许您管理注册表项和计算机或服务器上的值。

### <a name="features"></a>功能

在注册表中支持以下功能：

- 浏览注册表项和值
- 添加或修改注册表值
- 删除注册表值

[**为注册表中查看反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D)。

## <a name="remote-desktop"></a>远程桌面

**远程桌面**允许您与计算机或通过交互式桌面会话的服务器进行交互。

### <a name="features"></a>功能

在远程桌面中支持以下功能：

- 启动交互式远程桌面会话
- 断开与远程桌面会话的连接
- Ctrl + Alt + Del 发送到远程桌面会话

[**查看反馈和建议的功能的远程桌面**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D)。

## <a name="roles-and-features"></a>角色和功能

**角色和功能**可以管理角色和服务器上的功能。

### <a name="features"></a>功能

在角色和功能支持以下功能：

- 浏览角色和服务器上的功能的列表
- 查看角色或功能的详细信息
- 安装角色或功能
- 删除角色或功能

[**查看角色和功能的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D)。

## <a name="scheduled-tasks"></a>计划任务

**计划任务**允许你管理的计算机或服务器上的计划的任务。

### <a name="features"></a>功能

在任务计划程序中支持以下功能：

- 浏览任务计划程序库
- 编辑计划的任务
- 启用和禁用计划的任务
- 启动和停止计划的任务
- 创建计划的任务

[**查看计划任务的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D)。

## <a name="services"></a>Services

**服务**允许你管理的计算机或服务器上的服务。

### <a name="features"></a>功能

在服务中支持以下功能：

- 浏览和搜索服务器上的服务
- 查看服务的详细信息
- 启动服务
- 暂停服务
- 编辑服务的属性

[**查看服务的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D)。

## <a name="storage"></a>存储

**存储**允许你管理的计算机或服务器上存储设备。

### <a name="features"></a>功能

在存储中支持以下功能：

- 浏览和搜索服务器上的现有磁盘
- 查看磁盘详细信息
- 创建卷
- 初始化磁盘
- 创建、 附加和分离虚拟硬盘 (VHD)
- 使磁盘脱机
- 格式化卷
- 调整卷的大小
- 编辑卷属性
- 删除卷
- 安装配额管理

[**查看存储的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>存储迁移服务

**存储迁移服务**允许您迁移服务器和文件共享到 Azure 或 Windows Server 2019 — 而无需应用程序或用户进行任何更改。
[获取存储迁移服务的概述](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>存储迁移服务需要 Windows Server 2019。

## <a name="storage-replica"></a>存储副本

使用**存储副本**来管理服务器到服务器存储复制。
[了解有关存储副本的详细信息](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>系统见解

**系统 Insights**引入了预测分析以本机方式在 Windows Server 来帮助向你增加深入了解你的服务器的正常运行。
[获取系统 Insights 概述](http://aka.ms/systeminsights)

>[!NOTE]
>系统 Insights 需要 Windows Server 2019。

## <a name="updates"></a>更新

**更新**允许你管理的计算机或服务器上的 Microsoft 和/或 Windows 更新。

### <a name="features"></a>功能

在更新中支持以下功能：

- 查看可用的 Windows 或 Microsoft 更新
- 查看更新历史记录的列表
- 安装更新
- 联机检查 Microsoft Update 的更新
- 管理[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)集成

[**查看更新的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>虚拟机

请参阅[管理虚拟机与 Windows Admin Center](manage-virtual-machines.md)

## <a name="virtual-switches"></a>虚拟交换机

**虚拟交换机**允许你管理的计算机或服务器上的 HYPER-V 虚拟交换机。

### <a name="features"></a>功能

在虚拟交换机中支持以下功能：

- 浏览和搜索服务器上的虚拟交换机
- 创建新的虚拟交换机
- 重命名虚拟交换机
- 删除现有的虚拟交换机
- 编辑虚拟交换机的属性

[**查看虚拟交换机的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
