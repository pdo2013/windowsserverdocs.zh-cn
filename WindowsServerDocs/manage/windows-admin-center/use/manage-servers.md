---
title: 通过 Windows 管理中心管理服务器
description: 通过 Windows 管理中心管理服务器 (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 72524fcc71f722daeb8238bc3cffc6d38a611098
ms.sourcegitcommit: 9f955be34c641b58ae8b3000768caa46ad535d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2019
ms.locfileid: "68590585"
---
# <a name="manage-servers-with-windows-admin-center"></a>通过 Windows 管理中心管理服务器

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="managing-windows-server-machines"></a>管理 Windows Server 计算机

你可以将运行 Windows Server 2012 或更高版本的单独服务器添加到 Windows 管理中心, 以使用一组全面的工具 (包括证书、设备、事件、进程、角色和功能、更新、虚拟机等) 来管理服务器。

![服务器连接概述屏幕](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>向 Windows 管理中心添加服务器

若要将服务器添加到 Windows 管理中心:

1. 单击 "所有连接" 下的 " **+ 添加**"。
2. 选择添加**服务器连接**。
3. 键入服务器的名称, 并在出现提示时键入要使用的凭据。
4. 单击 "**提交**" 完成操作。

服务器将添加到 "概述" 页上的 "连接" 列表中。 单击它以连接到服务器。

> [!NOTE]
> 你还可以在 Windows 管理中心中将[故障转移群集](manage-failover-clusters.md)或[超聚合群集](manage-hyper-converged.md)添加为单独的连接。

## <a name="tools"></a>工具

以下工具可用于服务器连接:

| Tool | 描述 |
| ---- | ----------- |
| [概述](#overview) | 查看服务器详细信息和控制服务器状态 |
| [Active Directory](#active-directory-preview) | 管理 Active Directory |
| [Backup](#backup) | 查看和配置 Azure 备份 |  
| [证书](#certificates) | 查看和修改证书 |
| [存放](#containers) | 查看容器 |
| [设备](#devices) | 查看和修改设备 |
| [台](#dhcp) | 查看和管理 DHCP 服务器配置 |
| [DNS](#dns) | 查看和管理 DNS 服务器配置 |
| [事件](#events) | 查看事件 |
| [文件](#files) | 浏览文件和文件夹 |
| [Firewall](#firewall) | 查看和修改防火墙规则 |
| [安装的应用](#installed-apps) | 查看和删除已安装的应用 |
| [本地用户和组](#local-users-and-groups) | 查看和修改本地用户和组 |
| [网桥](#network) | 查看和修改网络设备 |
| [PowerShell](#powershell) | 通过 PowerShell 与服务器交互 |
| [进程](#processes) | 查看和修改正在运行的进程 |
| [Registry](#registry) | 查看和修改注册表项 |
| [远程桌面](#remote-desktop) | 通过远程桌面与服务器交互 |
| [角色和功能](#roles-and-features) | 查看和修改角色和功能 |
| [计划任务](#scheduled-tasks) | 查看和修改计划任务 |
| [服务](#services) | 查看和修改服务 |
| [设置](#settings) | 查看和修改服务 |
| [存储](#storage) | 查看和修改存储设备 |
| [存储迁移服务](#storage-migration-service) | 将服务器和文件共享迁移到 Azure 或 Windows Server 2019 |
| [存储副本](#storage-replica) | 使用存储副本来管理服务器到服务器的存储复制 |
| [系统见解](#system-insights) | 系统见解使你可以更深入地了解服务器的功能。 |
| [程序](#updates) | 查看已安装的并检查是否有新的更新 |
| [虚拟机](manage-virtual-machines.md) | 查看和管理虚拟机 |
| [虚拟交换机](#virtual-switches) | 查看和管理虚拟交换机 |

## <a name="overview"></a>概述

**概述**允许你查看当前的 CPU、内存和网络性能状态, 以及执行操作和修改目标计算机或服务器上的设置。

### <a name="features"></a>功能

服务器管理器概述支持以下功能:

- 查看服务器详细信息
- 查看 CPU 活动
- 查看内存活动
- 查看网络活动
- 重新启动服务器
- 关闭服务器
- 在服务器上启用磁盘指标
- 编辑服务器上的计算机 ID
- 通过超链接查看 BMC IP 地址 (需要 IPMI 兼容的 BMC)。

[**查看服务器概述的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D)。

## <a name="active-directory-preview"></a>Active Directory (预览)

**Active Directory**是在[扩展源](../configure/using-extensions.md)上提供的早期预览版。

### <a name="features"></a>功能

以下 Active Directory 管理可用:

- 创建用户
- 创建组
- 搜索用户、计算机和组
- 在网格中选择的用户、计算机和组的详细信息窗格
- 全局网格操作用户、计算机和组 (禁用/启用、删除)
- 重置用户密码
- 用户对象: 配置组成员身份 & 基本属性
- 计算机对象: 配置对单台计算机的委派
- 组对象: 管理成员身份 (一次添加/删除1个用户)  

[**查看 Active Directory 的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D)。

## <a name="backup"></a>备份

利用**备份**, 你可以通过将服务器直接备份到 Microsoft Azure 来保护 Windows server 免受损坏、攻击或灾难的影响。
[了解有关 Azure 备份的详细信息。](https://aka.ms/windows-admin-center-backup)

[为 Windows 管理中心中的备份提供反馈](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>功能

备份支持以下功能:

- 查看 Azure 备份状态的概述
- 配置备份项和计划
- 启动或停止备份作业
- 查看备份作业历史记录和状态
- 查看恢复点和恢复数据
- 删除备份数据

## <a name="certificates"></a>证书

**证书**允许管理计算机或服务器上的证书存储。

### <a name="features"></a>功能

证书支持以下功能:

- 浏览和搜索现有证书
- 查看证书详细信息
- 导出证书
- 续订证书
- 请求新证书
- 删除证书

[**查看证书的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D)。

## <a name="containers"></a>容器

使用**容器**可以查看 Windows Server 容器主机上的容器。 在运行 Windows Server Core 容器的情况下, 你可以查看事件日志并访问容器的 CLI。

[**查看容器的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D)。

## <a name="devices"></a>设备

使用**设备**, 你可以管理计算机或服务器上的连接设备。

### <a name="features"></a>功能

设备支持以下功能:

- 浏览和搜索设备
- 查看设备详细信息
- 禁用设备
- 更新设备上的驱动程序

[**查看设备的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D)。

## <a name="dhcp"></a>DHCP

**DHCP**允许您管理计算机或服务器上的连接设备。

### <a name="features"></a>功能

- 创建/配置/查看 IPV4 和 IPV6 作用域
- 创建地址排除并配置开始和结束 IP 地址
- 创建地址保留并配置客户端 MAC 地址 (IPV4)、DUID 和 IAID (IPV6)

[**查看 DHCP 的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D)。

## <a name="dns"></a>DNS

**DNS**允许管理计算机或服务器上的连接设备。

### <a name="features"></a>功能

- 查看 DNS 正向查找区域、反向查找区域和 DNS 记录的详细信息
- 创建正向查找区域 (主、辅助或存根), 并配置正向查找区域属性
- 创建主机 (A 或 AAAA)、CNAME 或 MX 类型的 DNS 记录
- 配置 DNS 记录属性
- 创建 IPV4 和 IPV6 反向查找区域 (主节点、辅助节点和存根区域)、配置反向查找区域属性
- 在反向查找区域下创建 DNS 记录的 PTR、CNAME 类型。

[**查看 DHCP 的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D)。

## <a name="events"></a>Events

**事件**可用于管理计算机或服务器上的事件日志。

### <a name="features"></a>功能

事件支持以下功能:

- 浏览和搜索事件
- 查看事件详细信息
- 从日志中清除事件
- 从日志中导出事件

[**查看事件的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D)。

## <a name="files"></a>文件

**文件**可用于管理计算机或服务器上的文件和文件夹。

### <a name="features"></a>功能

文件中支持以下功能:

- 浏览文件和文件夹
- 搜索文件或文件夹
- 创建新文件夹
- 删除文件或文件夹
- 下载文件或文件夹
- 上传文件或文件夹
- 重命名文件或文件夹
- 提取 zip 文件
- 查看文件或文件夹属性
- 添加、编辑或删除文件共享
- 修改文件共享上的用户和组权限

[**查看文件的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D)。

## <a name="firewall"></a>防火墙

**防火墙**允许您管理计算机或服务器上的防火墙设置和规则。

### <a name="features"></a>功能

防火墙支持以下功能:

- 查看防火墙设置概述
- 查看传入防火墙规则
- 查看传出防火墙规则
- 搜索防火墙规则
- 查看防火墙规则详细信息
- 创建新的防火墙规则
- 启用或禁用防火墙规则
- 删除防火墙规则
- 编辑防火墙规则的属性

[**查看适用于防火墙的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D)。

## <a name="installed-apps"></a>安装的应用

**已安装的应用**允许列出和卸载已安装的应用程序。

[**查看已安装应用的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D)。

## <a name="local-users-and-groups"></a>本地用户和组

**本地用户和组**允许您管理计算机或服务器上本地存在的安全组和用户。

### <a name="features"></a>功能

本地用户和组支持以下功能:

- 查看和搜索用户和组
- 创建新用户或组
- 管理用户的组成员身份
- 删除用户或组
- 更改用户的密码
- 编辑用户或组的属性

[**查看本地用户和组的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>网络

**网络**允许您管理计算机或服务器上的网络设备和设置。

### <a name="features"></a>功能

网络支持以下功能:

- 浏览和搜索现有网络适配器
- 查看网络适配器的详细信息
- 编辑网络适配器的属性
- 创建[Azure 网络适配器 (预览功能)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**查看网络反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**Powershell**允许您通过 powershell 会话与计算机或服务器进行交互。

### <a name="features"></a>功能

PowerShell 支持以下功能:

- 在服务器上创建交互式 PowerShell 会话
- 从服务器上的 PowerShell 会话断开连接

[**查看 PowerShell 的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>进程

使用**进程**可以管理计算机或服务器上运行的进程。

### <a name="features"></a>功能

进程支持以下功能:

- 浏览和搜索正在运行的进程
- 查看进程详细信息
- 启动进程
- 结束进程
- 创建进程转储
- 查找进程句柄

[**查看有关流程的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D)。

## <a name="registry"></a>注册表

**注册表**允许您管理计算机或服务器上的注册表项和值。

### <a name="features"></a>功能

注册表中支持以下功能:

- 浏览注册表项和值
- 添加或修改注册表值
- 删除注册表值

[**查看注册表的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D)。

## <a name="remote-desktop"></a>远程桌面

通过**远程桌面**, 你可以通过交互式桌面会话与计算机或服务器进行交互。

### <a name="features"></a>功能

远程桌面支持以下功能:

- 启动交互式远程桌面会话
- 断开与远程桌面会话的连接
- 将 Ctrl + Alt + Del 发送到远程桌面会话

[**查看远程桌面的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D)。

## <a name="roles-and-features"></a>角色和功能

使用**角色和功能**可以管理服务器上的角色和功能。

### <a name="features"></a>功能

角色和功能支持以下功能:

- 浏览服务器上的角色和功能列表
- 查看角色或功能详细信息
- 安装角色或功能
- 删除角色或功能

[**查看有关角色和功能的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D)。

## <a name="scheduled-tasks"></a>计划任务

**计划任务**可用于管理计算机或服务器上的计划任务。

### <a name="features"></a>功能

计划任务中支持以下功能:

- 浏览任务计划程序库
- 编辑计划任务
- 启用 & 禁用计划任务
- 启动 & 停止计划任务
- 创建计划任务

[**查看计划任务的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D)。

## <a name="services"></a>Services

**服务**允许你在计算机或服务器上管理服务。

### <a name="features"></a>功能

服务支持以下功能:

- 浏览和搜索服务器上的服务
- 查看服务的详细信息
- 启动服务
- 暂停服务
- 编辑服务的属性

[**查看服务的反馈和建议功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D)。

## <a name="settings"></a>设置

**设置**是用于管理计算机或服务器上的设置的中心位置。

### <a name="features"></a>功能

- 查看和修改用户和系统环境变量
- 查看用于监视警报的配置[Azure Monitor](azure-monitor.md)
- 查看和修改电源配置
- 查看和修改远程桌面设置
- 查看和修改基于角色的访问控制设置
- 查看和修改 Hyper-v 主机设置 (如果适用)

## <a name="storage"></a>存储

**存储**允许您管理计算机或服务器上的存储设备。

### <a name="features"></a>功能

存储支持以下功能:

- 浏览和搜索服务器上的现有磁盘
- 查看磁盘详细信息
- 创建卷
- 初始化磁盘
- 创建、附加和分离虚拟硬盘 (VHD)
- 使磁盘脱机
- 格式化卷
- 调整卷的大小
- 编辑卷属性
- 删除卷
- 安装配额管理
- 管理文件服务器资源管理器配额[存储-> 创建/更新配额](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)

[**查看有关存储的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>存储迁移服务

**存储迁移服务**允许你将服务器和文件共享迁移到 Azure 或 Windows Server 2019, 无需应用或用户更改任何内容。
[大致了解存储迁移服务](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>存储迁移服务需要 Windows Server 2019。

## <a name="storage-replica"></a>存储副本

使用**存储副本**来管理服务器到服务器的存储复制。
[了解有关存储副本的详细信息](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>系统见解

**System Insights**在 Windows Server 中以本机方式引入了预测分析, 以帮助你更深入地了解服务器的功能。
[获取系统见解的概述](http://aka.ms/systeminsights)

>[!NOTE]
>系统见解需要 Windows Server 2019。

## <a name="updates"></a>更新

使用**更新**可以在计算机或服务器上管理 Microsoft 和/或 Windows 更新。

### <a name="features"></a>功能

更新支持以下功能:

- 查看可用的 Windows 或 Microsoft 更新
- 查看更新历史记录的列表
- 安装更新
- 联机查看 Microsoft 更新中的更新
- 管理[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)集成

[**查看反馈和建议的更新功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>虚拟机

请参阅[通过 Windows 管理中心管理虚拟机](manage-virtual-machines.md)

## <a name="virtual-switches"></a>虚拟交换机

**虚拟交换机**可用于管理计算机或服务器上的 hyper-v 虚拟交换机。

### <a name="features"></a>功能

虚拟交换机支持以下功能:

- 在服务器上浏览和搜索虚拟交换机
- 创建新的虚拟交换机
- 重命名虚拟交换机
- 删除现有虚拟交换机
- 编辑虚拟交换机的属性

[**查看虚拟交换机的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
