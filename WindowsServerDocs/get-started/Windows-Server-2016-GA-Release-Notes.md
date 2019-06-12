---
title: 发行说明 - Windows Server 2016 中的重要问题
description: 总结了需要解决方法的重要问题，以避免故障、挂起、安装失败、数据丢失。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: dec1ec184a147ef4fae64e9cc4384c0b6e4510b6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749539"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>发行说明:Windows Server 2016 中的重要问题

>适用于：Windows Server 2016

这些发行说明汇总了在 Windows Server 2016 操作系统系统中，包括避免或解决问题，方法，如果已知的最关键问题。 有关此版本中按设计的更改、新功能和修补程序的信息，请参阅 [Windows Server 2016 中的新增功能](whats-new-in-windows-server-2016.md)和特定功能团队提供的通知。 除非另有指定，否则，每个所报告的问题均适用于 Windows Server 2016 的所有版本和安装选项。

本文档将持续更新。 发现需要解决的严重问题，以及有可用的新的解决方法和修补程序时，它们将被添加到此文档中。

## <a name="express-updates-available-starting-in-november-2018-new"></a>Express （新） 2018 年 11 月开始有可用的更新

从 2018 年 11 月开始"更新 Tuesday"更新，Windows 将再次发布[快速更新](express-updates.md)Windows Server 2016。 如果使用 WSUS 和 System Center Configuration Manager (SCCM) 将再一次请参阅 Windows Server 2016 更新两个包： 完整的更新和快速更新。 如果你想要为服务器环境使用速成版，需要确认该服务器已经自 2017 年 11 月 (KB # 4048953) 以确保正确安装快速更新的完整更新。 如果在尝试从 2017年 11B 更新 (KB # 4048953) 以来尚未更新的服务器上的快速更新，您将看到占用带宽和 CPU 资源在无限循环中的重复的失败。 如果出现这种情况时，停止推入 Express 更新，并改为将推送新的完整更新，停止失败循环。

## <a name="server-core-installation-option"></a>服务器核心安装选项

[comment]: # (ID:370;提交者： amason;状态： 已注销)

使用服务器核心安装选项安装 Windows Server 2016 时，即使在未安装打印服务器角色的情况下，也会默认安装和启动打印后台处理程序。

若要避免此问题，请在首次启动后将打印后台处理程序设为禁用。

## <a name="containers"></a>容器

[comment]: # (ID:371;提交者： taylorb;状态： 已注销)
- 使用容器之前，安装[的 Windows 10 版本 1607年服务堆栈更新：2016 年 8 月 23 日](https://support.microsoft.com/en-us/kb/3176936)或任何更高版本的更新可用。 否则为会发生的问题数，在构建，包括失败启动或运行容器和类似"CreateProcess 在 Win32 中失败：RPC 服务器不可用。"

[comment]: # (ID:373;提交者： plang;状态： 已注销)
- NanoServerPackage OneGet 提供程序在 Windows 容器中无效。 若要解决此问题，请在其他电脑（而非容器）上使用 Find-NanoServerPackage 和 Save-NanoServerPackage 下载所需的程序包。 然后将程序包复制到容器并安装。

## <a name="device-guard"></a>Device Guard

[comment]: # (ID:369;提交者： nirb;状态： 已注销)
如果使用代码完整性的基于虚拟化的保护或受防护的虚拟机（使用代码完整性的基于虚拟化的保护），则应意识到这些技术与一些设备和应用程序不兼容。 应先在实验室中测试此类配置，再启用生产系统上的功能。 否则将导致意外的数据丢失或停止错误。

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (ID:375;提交者： wgries;状态： 已注销)
如果尝试在 Windows Server 2016 上运行 Microsoft Exchange 2016 CU3，将在 IIS 主机进程 W3WP.exe 中遇到错误。 此时没有解决办法。 应推迟在 Windows Server 2016 上部署 Exchange 2016 CU3，直到可使用受支持的修补程序。

## <a name="remote-server-administration-tools-rsat"></a>远程服务器管理工具 (RSAT)

[comment]: # (ID:374;提交者： ryanpu;状态： 已注销)
如果您运行的版本早于周年更新中，Windows 10 和使用 HYPER-V 和虚拟机已启用虚拟受信任的平台模块 （包括受防护的虚拟机），然后再安装的版本为 Windows 提供 RSATServer 2016 中，尝试启动这些虚拟机将失败。

若要避免此问题，安装 RSAT 前，请将客户端计算机升级到 Windows 10 周年更新（或更高版本）。 如果已发生此问题，请卸载 RSAT，将客户端升级到 Windows 10 周年更新，然后重装 RSAT。

## <a name="shielded-virtual-machines"></a>受防护的虚拟机

[comment]: # (ID:369;提交者： nirb;状态： 已注销)  
- 请确保安装了所有可用更新后再在生产中部署受防护的虚拟机。

- 如果使用代码完整性的基于虚拟化的保护或受防护的虚拟机（使用代码完整性的基于虚拟化的保护），则应意识到这些技术与一些设备和应用程序不兼容。 应先在实验室中测试此类配置，再启用生产系统上的功能。 否则将导致意外的数据丢失或停止错误。

## <a name="start-menu"></a>“开始”菜单

[comment]: # (ID:372;提交者： samli;状态： 已注销)
此问题将影响与具有桌面体验的服务器选项一起安装的 Windows Server 2016。

如果安装任何应用程序上添加的文件夹内的快捷方式项**启动**菜单中，直到你注销并再次登录快捷方式不起作用。

返回到主 [Windows Server 2016](Windows-Server-2016.md) 集线器。

## <a name="storport-performance"></a>Storport 性能

相比 Windows Server 2012 R2，运行新安装的 Windows Server 2016 时某些系统可能会出现存储性能降低的情况。  Windows Server 2016 的开发过程涉及大量更改，以改进此平台的安全性和可靠性。 其中部分更改，例如默认启用 Windows Defender，会导致输入/输出路径拉长，进而导致在特定工作负载和模式中输入/输出性能降低。 Microsoft 不建议禁用 Windows Defender，因为它是系统的重要保护层。  

## <a name="copyright"></a>版权

本文档按“原样”提供。 本文档中表达的信息和视图（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。  

本文档未向你提供针对任何 Microsoft 产品的任何知识产权的任何法律权限。 你可以复制和使用本文档作为内部参考之用。  

&copy;2016 Microsoft Corporation。 保留所有权利。  

Microsoft、Active Directory、Hyper-V、Windows 和 Windows Server 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。  

本产品包含图形过滤器软件；该软件在一定程度上是以独立 JPEG 小组所做工作为基础的。  

1.0
