---
title: 服务器核心是什么？
description: 了解如何在 Windows Server 中的服务器核心安装选项
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718531"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>在 Windows Server 中的服务器核心安装选项是什么？

> 适用于： Windows Server （半年通道） 和 Windows Server 2016

部署 Windows Server Standard 或 Datacenter edition 时可用的最小的安装选项的服务器核心选项。 服务器核心包括大多数但并非所有服务器角色。 服务器核心具有更小的磁盘空间，因此由于基本较小的代码来减小攻击面。 

## <a name="server-core-vs-server-with-desktop-experience"></a>服务器 （核心） vs 服务器与桌面体验 
在安装 Windows Server 时，您将安装您选择-这有助于降低的总占用的 Windows Server 的服务器角色。 但是，与桌面体验安装选项的服务器仍安装许多服务和特定的使用方案通常不需要其他组件。 

这是服务器核心其中发挥作用： 服务器核心安装无任何服务，通常不是非常重要的某些支持的其他功能使用的服务器角色。 例如，HYPER-V 服务器不需要图形用户界面 (GUI)，因为您可以从命令行使用 Windows PowerShell 或远程使用 HYPER-V 管理器管理的 HYPER-V 的几乎所有方面。 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>服务器核心差异-不修饰的核心功能
完成对系统和登录的服务器核心安装第一次后，您正在为意外情况发生位。 桌面体验安装选项的服务器和服务器核心的主要区别是，服务器核心不包括下列 GUI 命令行管理程序包：

- Microsoft Windows 的服务器的命令行管理程序-程序包
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

换句话说，没有**任何桌面**服务器核心设计。 同时支持传统的业务应用程序和基于角色的工作负载所需的功能，服务器核心没有传统桌面界面。 相反，服务器核心旨在通过命令行、 PowerShell 中或 （如[RSAT](../../remote/remote-server-administration-tools.md)或[Windows Admin Center](../../manage/windows-admin-center/overview.md)） GUI 工具远程管理。

除了没有用户界面，服务器核心还与桌面体验服务器从以下几方面有所不同：

- 服务器核心没有任何辅助工具
- 设置服务器核心没有 OOBE （出的-体验）
- 没有音频的支持

下表显示了哪些应用程序在可用*本地*服务器核心 vs 与桌面体验的服务器上。 **重要说明**： 在大多数情况下，列出"不可用"下可以远程运行 Windows 客户端计算机上并用于管理您的服务器核心安装应用程序。

> [!NOTE]
> 此列表适用于快速参考-它不能的完整列表。


| 应用程序                     | 服务器核心     | 服务器（提供桌面体验） |
|------------------------------------|-----------------|--------------------------------|
| 命令提示符处                     | 有空       | 有空                      |
| Windows PowerShell / Microsoft.NET | 有空       | 有空                      |
| Perfmon.exe                        | 不可用  | 有空                      |
| Windbg (GUI)                         | 受支持       | 受支持                      |
| Resmon.exe                         | 不可用   | 有空                      |
| Regedit                            | 有空       | 有空                      |
| Fsutil.exe                         | 有空       | 有空                      |
| Disksnapshot.exe                   | 不可用   | 有空                      |
| Diskpart.exe                       | 有空       | 有空                      |
| Diskmgmt.msc                       | 不可用   | 有空                      |
| Devmgmt.msc                        | 不可用   | 有空                      |
| 服务器管理器                     | 不可用  | 有空                      |
| Mmc.exe                            | 不可用   | 有空                      |
| Eventvwr                           | 不可用  | 有空                      |
| Wevtutil （事件查询）           | 有空       | 有空                      |
| Services.msc                       | 不可用   | 有空                      |
| 控制面板                      | 不可用   | 有空                      |
| Windows Update (GUI)                 | 不可用 | 有空                      |
| Windows 资源管理器                   | 不可用   | 有空                      |
| 任务栏                            | 不可用   | 有空                      |
| 任务栏的通知              | 不可用   | 有空                      |
| Taskmgr                            | 有空       | 有空                      |
| Internet Explorer 或边缘          | 不可用   | 有空                      |
| 内置的帮助系统               | 不可用   | 有空                      |
| Windows 10 命令行管理程序                   | 不可用   | 有空                      |
| Windows Media Player               | 不可用   | 有空                      |
| PowerShell                         | 有空       | 有空                      |
| PowerShell ISE                     | 不可用   | 有空                      |
| PowerShell IME                     | 有空       | 有空                      |
| Mstsc.exe                          | 不可用   | 有空                      |
| 远程桌面服务            | 有空       | 有空                      |
| Hyper-V 管理器                    | 不可用  | 有空                      |


什么** 包括在服务器核心的详细信息，请参阅[角色、 角色服务和 Windows Server 的服务器核心中包括的功能](server-core-roles-and-services.md)。 有关哪些*不*包括在服务器核心的信息，请参阅[角色、 角色服务和功能不包括在服务器核心](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>开始使用服务器核心
使用以下信息来安装、 配置和管理的 Windows Server 的服务器核心安装选项。

服务器核心安装： 
- [角色、 角色服务和服务器核心中包括的功能](server-core-roles-and-services.md)
- [角色、 角色服务和功能不在服务器核心](server-core-removed-roles.md)
- [安装的服务器核心安装选项](../../get-started/getting-started-with-server-core.md)
- [配置与 SConfig 工具的服务器核心](../../get-started/sconfig-on-ws2016.md)

使用服务器核心：
- [使用 Windows PowerShell 或命令行的基本服务器核心管理任务](server-core-administer.md)
- [管理服务器核心](server-core-manage.md)
- [修补服务器核心](server-core-servicing.md)
- [配置内存转储文件](server-core-memory-dump.md)