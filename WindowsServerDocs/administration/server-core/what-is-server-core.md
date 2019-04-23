---
title: 什么是服务器核心？
description: 了解如何在 Windows Server 中的服务器核心安装选项
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885598"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>什么是 Windows Server 中的服务器核心安装选项？

> 适用于：Windows Server （半年频道） 和 Windows Server 2016

服务器核心选项是要部署的 Windows Server Standard 或 Datacenter 版本时可用的最小安装选项。 服务器核心包括大多数但并非所有服务器角色。 Server Core 具有更小的磁盘空间，因此由于较小的代码库来减小攻击面。 

## <a name="server-core-vs-server-with-desktop-experience"></a>服务器 (Core) 与带有桌面体验的服务器 
在安装 Windows Server，安装仅的服务器角色，你选择-这将有助于减少适用于 Windows Server 的总体内存占用。 但是，带桌面体验安装服务器仍然会安装许多服务和特定使用方案通常不需要其他组件。 

这就是服务器核心派上用场： 服务器核心安装消除了任何服务以及使用通常不是必需的特定支持的其他功能的服务器角色。 例如，HYPER-V 服务器不需要图形用户界面 (GUI)，因为可以从命令行使用 Windows PowerShell 或使用 HYPER-V 管理器远程管理的 HYPER-V 的几乎所有方面。 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>服务器核心区别-而无需修饰的核心功能
完成服务器核心上安装系统和登录第一次后，您在为一点惊喜。 具有桌面体验安装选项的服务器和 Server Core 的主要区别是 Server Core 不包括以下 GUI shell 包：

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

换而言之，没有**没有桌面**在 Server Core 中，通过设计。 在保持同时支持传统的业务应用程序和基于角色的工作负荷所需的功能，服务器核心不具有传统的桌面界面。 相反，服务器核心旨在通过命令行、 PowerShell 或 GUI 工具远程管理 (如[RSAT](../../remote/remote-server-administration-tools.md)或[Windows Admin Center](../../manage/windows-admin-center/overview.md))。

除了用户界面，服务器核心也不同于具有桌面体验的服务器通过以下方式：

- 服务器核心不具有任何可访问性工具
- 服务器核心安装没有 OOBE （扩展的-全新安装体验）
- 无音频支持

下表显示了哪些应用程序时可用*本地*服务器核心和带有桌面体验的服务器。 **重要**:在大多数情况下，应用程序的列为"不可用"下面可以远程运行从 Windows 客户端计算机并用于管理服务器核心安装。

> [!NOTE]
> 此列表适用于快速参考-它并不旨在将完整的列表。


| 应用程序                     | 服务器核心     | 服务器（提供桌面体验） |
|------------------------------------|-----------------|--------------------------------|
| 命令提示符                     | 有空       | 有空                      |
| Windows PowerShell/ Microsoft .NET | 有空       | 有空                      |
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
| Windows 更新 (GUI)                 | 不可用 | 有空                      |
| Windows 资源管理器                   | 不可用   | 有空                      |
| 任务栏                            | 不可用   | 有空                      |
| 任务栏通知              | 不可用   | 有空                      |
| 任务管理器                            | 有空       | 有空                      |
| Internet Explorer 或 Edge          | 不可用   | 有空                      |
| 内置帮助系统               | 不可用   | 有空                      |
| Windows 10 Shell                   | 不可用   | 有空                      |
| Windows Media Player               | 不可用   | 有空                      |
| PowerShell                         | 有空       | 有空                      |
| PowerShell ISE                     | 不可用   | 有空                      |
| PowerShell IME                     | 有空       | 有空                      |
| Mstsc.exe                          | 不可用   | 有空                      |
| 远程桌面服务            | 有空       | 有空                      |
| Hyper-V 管理器                    | 不可用  | 有空                      |


详细了解什么*是*包含在服务器核心中，请参阅[角色、 角色服务和功能包括在 Windows Server 的服务器核心](server-core-roles-and-services.md)。 并了解解决*不是*包含在服务器核心中，请参阅[角色、 角色服务和功能未包含在服务器核心](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>开始使用服务器核心
使用以下信息来安装、 配置和管理 Windows Server 的服务器核心安装选项。

Server Core 安装： 
- [角色、 角色服务和功能包含在服务器核心](server-core-roles-and-services.md)
- [角色、 角色服务和功能不在服务器核心](server-core-removed-roles.md)
- [安装服务器核心安装选项](../../get-started/getting-started-with-server-core.md)
- [使用 SConfig 工具配置服务器核心](../../get-started/sconfig-on-ws2016.md)

使用 Server Core:
- [使用 Windows PowerShell 或命令行的基本服务器核心管理任务](server-core-administer.md)
- [管理 Server Core](server-core-manage.md)
- [修补的服务器核心](server-core-servicing.md)
- [配置内存转储文件](server-core-memory-dump.md)