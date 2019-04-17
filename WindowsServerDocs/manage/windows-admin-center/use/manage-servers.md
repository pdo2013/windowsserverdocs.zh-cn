---
title: 管理 Windows Admin Center 的服务器
description: 管理服务器使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93a40345d05a6230596832b2d455d36eee2401b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296679"
---
# 管理 Windows Admin Center 的服务器

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## 管理 Windows Server 的计算机

你可以添加单个服务器运行 Windows Server 2012 或更高版本到 Windows Admin Center 来管理一组全面的工具的服务器包括证书、 设备、 事件、 进程、 角色和功能，更新，虚拟机等。

![服务器连接概述屏幕](../media/manage-servers/server-overview.png)

## 将服务器添加到 Windows Admin Center

若要将服务器添加到 Windows Admin Center:

1. 单击下的所有连接的 **+ 添加**。
2. 选择要添加的**服务器连接**。
3. 键入服务器的名称，如果出现提示，若要使用的凭据。
4. 单击**提交**完成。

服务器将被添加到你在概述页面上的连接列表。 单击它以连接到服务器。

> [!NOTE]
> 你还可以为一个单独的连接，Windows Admin Center 中添加[故障转移群集](manage-failover-clusters.md)或[超融合群集](manage-hyper-converged.md)。

## 工具

以下工具是适用于服务器连接：

| 工具 | 说明 |
| ---- | ----------- |
| [概述](#overview) | 查看服务器详细信息和控件服务器状态 |
| [Active Directory](#active-directory-preview) | 管理 Active Directory |
| [备份](#backup) | 查看和配置 Azure 备份 |  
| [证书](#certificates) | 查看和修改证书 |
| [容器](#containers) | 视图容器 |
| [设备](#devices) | 查看和修改设备 |
| [DHCP](#dhcp) | 查看和管理 DHCP 服务器配置 |
| [DNS](#dns) | 查看和管理 DNS 服务器配置 |
| [事件](#events) | 查看事件 |
| [文件](#files) | 浏览文件和文件夹 |
| [防火墙](#firewall) | 查看和修改防火墙规则 |
| [已安装的应用](#installed-apps) | 查看并删除已安装的应用 |
| [本地用户和组](#local-users-and-groups) | 查看和修改本地用户和组 |
| [网络](#network) | 查看和修改网络设备 |
| [PowerShell](#powershell) | 与服务器通过 PowerShell 进行交互 |
| [进程](#processes) | 查看和修改正在运行的进程 |
| [注册表](#registry) | 查看和修改注册表项 |
| [远程桌面](#remote-desktop) | 与通过远程桌面的服务器交互 |
| [角色和功能](#roles-and-features) | 查看和修改角色和功能 |
| [计划任务](#scheduled-tasks) | 查看和修改计划的任务 |
| [服务](#services) | 查看和修改服务 |
| [设置](#settings) | 查看和修改服务 |
| [存储](#storage) | 查看和修改存储设备 |
| [存储迁移服务](#storage-migration-service) | 迁移到 Azure 或 Windows Server 2019 的服务器和文件共享 |
| [存储副本](#storage-replica) | 使用存储副本管理服务器到服务器存储复制 |
| [系统见解](#system-insights) | 系统见解将提高你的服务器的运行时的见解。 |
| [更新](#updates) | 查看已安装并检查新的更新 |
| [虚拟机](manage-virtual-machines.md) | 查看和管理虚拟机 |
| [虚拟交换机](#virtual-switches) | 查看和管理虚拟交换机 |

## 概述

**概述**允许你查看 CPU、 内存和网络性能的当前状态，以及执行的操作和修改目标计算机或服务器上的设置。

### 功能

在服务器管理器概述支持以下功能：

- 查看服务器详细信息
- 查看 CPU 活动
- 查看内存活动
- 查看网络活动
- 重启服务器
- 关闭服务器
- 启用在服务器上的磁盘指标
- 在服务器上编辑计算机 ID
- 查看使用超链接 （需要 IPMI 兼容 BMC） BMC IP 地址。

[**查看反馈和建议的功能使服务器概述**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D)。

## Active Directory （预览版）

**Active Directory**是可用于[扩展源](../configure/using-extensions.md)早期预览版。

### 功能

下面的 Active Directory 管理可供选择：

- 创建用户
- 创建组
- 搜索用户、 计算机和组
- 详细信息窗格中的用户、 计算机和当在网格中选择的组
- 全局网格中操作的用户、 计算机和组 （禁用/启用、 删除）
- 重置用户密码
- 用户对象： 配置基本属性 & 组成员身份
- 计算机对象： 配置到一台计算机的委派
- 组对象： 管理成员身份 （一次添加/删除 1 用户）  

[**查看反馈和 Active directory 的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D)。

## 备份

**备份**可以直接向 Microsoft Azure 备份你的服务器的从损坏、 攻击或灾难保护 Windows server。
[了解有关 Azure 备份。](https://aka.ms/windows-admin-center-backup)

[提供反馈以进行 Windows Admin Center 中的备份](https://aka.ms/backup-wac-feedback)

### 功能

在备份中支持以下功能：

- 查看你的 Azure 备份状态的概述
- 配置备份项目和计划
- 开始或停止备份作业
- 查看备份作业历史记录和状态
- 查看恢复点和恢复数据
- 删除备份数据

## 证书

**证书**可以管理计算机或服务器上的证书存储。

### 功能

在证书中支持以下功能：

- 浏览和搜索现有的证书
- 查看证书的详细信息
- 导出证书
- 续订证书
- 请求新证书
- 删除证书

[**查看反馈和证书的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D)。

## 容器

**容器**可以查看 Windows Server 容器主机上的容器。 对于运行 Windows Server Core 容器，你可以查看事件日志和访问容器的 CLI。

[**查看反馈和适用于容器的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D)。

## 设备

**设备**可以管理计算机或服务器上的连接的设备。

### 功能

在设备支持以下功能：

- 浏览和搜索设备
- 查看设备详细信息
- 禁用设备
- 在设备上的驱动程序更新

[**查看反馈和设备的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D)。

## DHCP

**DHCP**允许您管理计算机或服务器上的连接的设备。

### 功能

- 创建/配置/查看 IPV4 和 IPV6 范围
- 创建地址排除项和配置开始菜单和结束 IP 地址
- 创建地址预订并配置客户端 MAC 地址 (IPV4) DUID 和 IAID (IPV6)

[**查看反馈和 DHCP 的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D)。

## DNS

**DNS**允许您管理计算机或服务器上的连接的设备。

### 功能

- 查看 DNS 正向查找区域、 反向查找区域和 DNS 记录的详细信息
- 创建正向查找区域 (主要、 辅助或存根)，并配置正向查找区域属性
- 创建主机 （A 或 AAAA），DNS 记录 CNAME 或 MX 类型
- 配置 DNS 记录属性
- 创建 IPV4 和 IPV6 反向查找区域 (主要、 辅助和存根)，配置反向查找区域属性
- 创建 PTR，CNAME 类型的 DNS 记录下反向查找区域。

[**查看反馈和 DHCP 的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D)。

## 事件

**事件**允许你管理计算机或服务器上的事件日志。

### 功能

事件支持以下功能：

- 浏览和搜索事件
- 查看事件详细信息
- 清除事件日志中
- 导出事件日志中

[**查看反馈和事件的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D)。

## 文件

**文件**可以管理文件和计算机或服务器上的文件夹。

### 功能

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
- 添加、 编辑或删除的文件共享
- 修改用户和组的文件共享上的权限

[**查看反馈和文件的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D)。

## 防火墙

**防火墙**允许你管理防火墙设置和计算机或服务器上的规则。

### 功能

防火墙支持以下功能：

- 查看防火墙设置的概述
- 查看传入防火墙规则
- 查看传出防火墙规则
- 搜索防火墙规则
- 查看防火墙规则详细信息
- 创建新的防火墙规则
- 启用或禁用防火墙规则
- 删除防火墙规则
- 编辑防火墙规则的属性

[**查看反馈和防火墙的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D)。

## 已安装的应用

**安装应用**可以列出并卸载安装的应用程序。

[**查看反馈和安装应用的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D)。

## 本地用户和组

**本地用户和组**允许你管理安全组和本地计算机或服务器存在的用户。

### 功能

本地用户和组中支持以下功能：

- 视图和搜索用户和组
- 创建新用户或组
- 管理用户的组成员身份
- 删除用户或组
- 更改用户的密码
- 编辑用户或组的属性

[**查看本地用户和组的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## 网络

**网络**允许您管理网络设备和计算机或服务器上的设置。

### 功能

在网络中支持以下功能：

- 浏览和搜索现有的网络适配器
- 查看网络适配器的详细信息
- 编辑的网络适配器的属性
- 创建[Azure 网络适配器 （预览功能）](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**查看网络的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## PowerShell

**PowerShell**允许你与计算机或通过 PowerShell 会话的服务器进行交互。

### 功能

在 PowerShell 中支持以下功能：

- 在服务器上创建交互式 PowerShell 会话
- 从服务器上的 PowerShell 会话中断开连接

[**查看反馈和建议的功能的 PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## 进程

**进程**允许你管理的计算机或服务器上运行的进程。

### 功能

在进程中支持以下功能：

- 浏览和搜索正在运行的进程
- 查看流程详细信息
- 启动进程
- 结束进程
- 创建进程转储
- 查找过程句柄

[**查看反馈和建议的进程的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D)。

## 注册表

**注册表**允许你管理注册表项和计算机或服务器上的值。

### 功能

在注册表中支持以下功能：

- 浏览注册表项和值
- 添加或修改注册表值
- 删除注册表值

[**查看反馈和注册表的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D)。

## 远程桌面

**远程桌面**允许你与计算机或通过交互式桌面会话的服务器进行交互。

### 功能

远程桌面支持以下功能：

- 开始菜单的交互的远程桌面会话
- 断开与远程桌面会话
- 向远程桌面会话发送 Ctrl + Alt + Del

[**查看反馈和远程桌面的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D)。

## 角色和功能

**角色和功能**允许你管理角色和功能在服务器上。

### 功能

角色和功能中支持以下功能：

- 浏览的角色和功能在服务器上的列表
- 查看角色或功能的详细信息
- 安装的角色或功能
- 删除某个角色或功能

[**查看反馈和建议的角色和功能的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D)。

## 计划任务

**计划任务**允许你管理的计算机或服务器上的计划的任务。

### 功能

任务计划支持以下功能：

- 浏览任务计划程序库
- 编辑计划的任务
- 启用 & 禁用计划任务
- 开始菜单 & 停止计划任务
- 创建计划的任务

[**查看反馈和计划任务的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D)。

## 服务

**服务**允许你管理计算机或服务器上的服务。

### 功能

在服务中支持以下功能：

- 浏览和搜索服务器上的服务
- 查看服务的详细信息
- 启动服务
- 暂停服务
- 编辑服务的属性

[**查看反馈和服务的建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D)。

## 设置

**设置**是中央位置来管理计算机或服务器上的设置。

### 功能

- 查看和修改用户和系统环境变量
- 查看用于监视[Azure 监视器](azure-monitor.md)发出的警报的配置
- 查看和修改电源配置
- 查看和修改远程桌面设置
- 查看和修改基于角色的访问控制设置
- 查看和修改 HYPER-V 主机设置，如果适用

## 存储

**存储**允许你管理计算机或服务器上的存储设备。

### 功能

在存储中支持以下功能：

- 浏览和搜索服务器上的现有磁盘
- 查看磁盘详细信息
- 创建卷
- 初始化磁盘
- 创建、 连接和断开虚拟硬盘 (VHD)
- 脱机磁盘
- 格式化的卷
- 调整卷的大小
- 编辑卷属性
- 删除卷
- 安装配额管理

[**查看存储的反馈和建议的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## 存储迁移服务

**存储迁移服务**允许你迁移服务器和文件共享的 Azure 或 Windows Server 2019，而无需应用或用户更改任何内容。
[获取存储迁移服务概述](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>存储迁移服务需要 Windows Server 2019。

## 存储副本

使用**存储副本**管理服务器到服务器存储复制。
[了解有关存储副本的详细信息](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## 系统见解

**系统见解**将在本机预测分析引入 Windows Server，以帮助您增加你的服务器的运行时的见解。
[获取系统见解的概述](http://aka.ms/systeminsights)

>[!NOTE]
>系统见解需要 Windows Server 2019。

## 更新

**更新**允许你管理计算机或服务器上的 Microsoft 和/或 Windows 更新。

### 功能

更新支持以下功能：

- 查看可用的 Windows 或 Microsoft 更新
- 查看更新历史记录的列表
- 安装更新
- 联机检查来自 Microsoft 更新的更新
- 管理[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)集成

[**查看反馈和建议的功能更新**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## 虚拟机

请参阅[管理虚拟机使用 Windows Admin Center](manage-virtual-machines.md)

## 虚拟交换机

**虚拟交换机**允许你管理计算机或服务器上的 HYPER-V 虚拟交换机。

### 功能

在虚拟交换机中支持以下功能：

- 浏览和搜索服务器上的虚拟交换机
- 创建一个新的虚拟交换机
- 重命名的虚拟交换机
- 删除现有的虚拟交换机
- 编辑虚拟交换机的属性

[**查看反馈和建议的功能的虚拟交换机**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
