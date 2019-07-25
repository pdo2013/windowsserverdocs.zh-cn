---
title: 什么是服务器核心？
description: 了解 Windows Server 中的服务器核心安装选项
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: ce00bc973b7b750e33326cdec24193ba537b5294
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476477"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Windows Server 中的服务器核心安装选项是什么？

> 适用于：Windows Server 2019、Windows Server 2016 和 Windows Server (半年频道)

"服务器核心" 选项是在部署 Windows Server Standard 或 Datacenter 版本时可用的最小安装选项。 服务器核心包括大多数但并非全部服务器角色。 服务器核心的磁盘占用量较小, 因此, 由于基本代码较小, 因此攻击面更小。 

## <a name="server-core-vs-server-with-desktop-experience"></a>服务器 (内核) vs Server 与桌面体验 
当你安装 Windows Server 时, 你只安装所选的服务器角色, 这有助于降低 Windows Server 的总体占用量。 但是, 具有桌面体验安装选项的服务器仍将安装许多服务和其他组件, 这些组件通常不需要用于特定的使用方案。 

这就是服务器核心的重头戏: 服务器核心安装消除了对某些常用服务器角色的支持不是必需的任何服务和其他功能。 例如, Hyper-v 服务器不需要图形用户界面 (GUI), 因为你可以使用 Windows PowerShell 或从命令行使用 Hyper-v 管理器远程管理 Hyper-v 的所有方面。 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>不带基本的服务器核心差异-核心功能
当你在系统上完成服务器核心安装并首次登录时, 你会感到惊讶。 具有桌面体验安装选项和服务器核心的服务器之间的主要区别在于, 服务器核心不包括以下 GUI shell 包:

- Microsoft-服务器-Shell-包
- Microsoft-Windows-服务器-Gui 管理包
- Microsoft-Windows-服务器-Gui-包
- "Windows-Cortana-PAL-桌面包"

换句话说, 在设计上, 服务器核心中**没有桌面**。 虽然维护支持传统的业务应用程序和基于角色的工作负荷所需的功能, 但服务器核心没有传统的桌面界面。 相反, 服务器核心旨在通过命令行、PowerShell 或 GUI 工具 (如[RSAT](../../remote/remote-server-administration-tools.md)或[Windows 管理中心](../../manage/windows-admin-center/overview.md)) 进行远程管理。

除了无 UI, 服务器核心还不同于具有桌面体验的服务器:

- 服务器核心没有任何辅助工具
- 没有用于设置服务器核心的 OOBE (全新体验)
- 无音频支持

下表显示了在具有桌面体验的服务器核心与服务器上*本地*可用的应用程序。 **重要提示**:大多数情况下, 在下面列出为 "不可用" 的应用程序可以从 Windows 客户端计算机远程运行, 并用于管理服务器核心安装。

> [!NOTE]
> 此列表用于快速参考-它不是完整列表。


| 应用程序                     | 服务器核心     | 服务器（提供桌面体验） |
|------------------------------------|-----------------|--------------------------------|
| 命令提示符                     | 有空       | 有空                      |
| Windows PowerShell/Microsoft .NET | 有空       | 有空                      |
| Perfmon                        | 不可用  | 有空                      |
| Windbg (GUI)                         | 受支持       | 受支持                      |
| Resmon                         | 不可用   | 有空                      |
| Regedit.exe                            | 有空       | 有空                      |
| Dism.exe                         | 有空       | 有空                      |
| Disksnapshot                   | 不可用   | 有空                      |
| Diskpart.exe                       | 有空       | 有空                      |
| Diskmgmt.msc                       | 不可用   | 有空                      |
| Devmgmt.msc                        | 不可用   | 有空                      |
| 服务器管理器                     | 不可用  | 有空                      |
| Mmc.exe                            | 不可用   | 有空                      |
| Eventvwr.msc                           | 不可用  | 有空                      |
| Wevtutil (事件查询)           | 有空       | 有空                      |
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
| PowerShell 输入法                     | 有空       | 有空                      |
| Mstsc                          | 不可用   | 有空                      |
| 远程桌面服务            | 有空       | 有空                      |
| Hyper-V 管理器                    | 不可用  | 有空                      |


有关服务器核心中包含*的内容的*详细信息, 请参阅[Windows Server-server core 中包含的角色、角色服务和功能](server-core-roles-and-services.md)。 有关服务器核心中*未*包含的内容的信息, 请参阅[不包含在服务器核心中的角色、角色服务和功能](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>开始使用服务器核心
使用以下信息来安装、配置和管理 Windows Server 的服务器核心安装选项。

服务器核心安装: 
- [服务器核心中包含的角色、角色服务和功能](server-core-roles-and-services.md)
- [不在服务器核心中的角色、角色服务和功能](server-core-removed-roles.md)
- [安装服务器核心安装选项](../../get-started/getting-started-with-server-core.md)
- [用 Sconfig.cmd 工具配置服务器核心](../../get-started/sconfig-on-ws2016.md)

使用服务器核心:
- [使用 Windows PowerShell 或命令行的基本服务器核心管理任务](server-core-administer.md)
- [管理服务器核心](server-core-manage.md)
- [修补服务器核心](server-core-servicing.md)
- [配置内存转储文件](server-core-memory-dump.md)