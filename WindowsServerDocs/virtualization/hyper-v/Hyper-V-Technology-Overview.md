---
title: Hyper-v 技术概述
description: 介绍什么是 Hyper-v，如何获取它、主要功能和常见用途。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 053f92f1ef07a2e574c93412626ee792d4d982e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366782"
---
# <a name="hyper-v-technology-overview"></a>Hyper-v 技术概述

>适用于：Windows Server 2016，Microsoft Hyper-V Server 2016，Windows Server 2019，Microsoft Hyper-V 服务器2019

Hyper-v 是 Microsoft 的硬件虚拟化产品。 它允许您创建和运行计算机的软件版本（称为*虚拟机*）。 每个虚拟机的行为类似于一台运行操作系统和程序的完整计算机。 当你需要计算资源时，虚拟机为你带来了更大的灵活性，有助于节省时间和资金，并且比只是在物理硬件上运行一个操作系统更有效。

Hyper-v 在各自隔离的空间中运行每个虚拟机，这意味着你可以同时在同一个硬件上运行多个虚拟机。 你可能希望执行此操作以避免问题（如故障影响其他工作负荷），或为不同的人员、组或服务访问不同的系统。

## <a name="some-ways-hyper-v-can-help-you"></a>Hyper-v 可帮助你

Hyper-v 可帮助你：

- **建立或扩展私有云环境。** 通过移动或扩展共享资源的使用，并根据需求变化调整利用率，提供更灵活的按需 IT 服务。

- **更有效地使用硬件。** 将服务器和工作负载合并到更少、功能更强大的物理计算机上，以减少能耗和物理空间。

- **改善业务连续性。** 最大程度地降低工作负荷的计划和非计划停机时间的影响。

- **建立或扩展虚拟桌面基础结构（VDI）。** 使用具有 VDI 的集中式桌面策略可帮助你提高业务灵活性和数据安全性，还可简化法规遵从性并管理桌面操作系统和应用程序。 在同一台服务器上部署 Hyper-v 和远程桌面虚拟化主机（RD 虚拟化主机），以使用户可以使用个人虚拟机或虚拟机池。

- **提高开发和测试效率。** 再现不同的计算环境，而无需购买或维护只使用物理系统时所需的所有硬件。

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-v 和其他虚拟化产品

Windows 和 Windows Server 中的 hyper-v 替代了较旧的硬件虚拟化产品，如 Microsoft Virtual PC、Microsoft Virtual Server 和 Windows Virtual PC。 Hyper-v 提供了在这些较旧的产品中不可用的网络、性能、存储和安全功能。

Hyper-v 和需要相同处理器功能的大多数第三方虚拟化应用程序不兼容。 这是因为处理器功能（称为硬件虚拟化扩展）设计为不共享。 有关详细信息，请参阅[虚拟化应用程序不能与 hyper-v、Device Guard 和 Credential guard 一起工作](https://support.microsoft.com/kb/3204980)。

## <a name="what-features-does-hyper-v-have"></a>Hyper-v 有哪些功能？

Hyper-v 提供了许多功能。 这是一种概述，按功能的提供或帮助进行分组。

**计算环境**-hyper-v 虚拟机包含与物理计算机相同的基本部分，例如内存、处理器、存储和网络。 所有这些部分都具有一些功能和选项，你可以根据不同需求配置不同的方式。 可以将存储和网络视为各自的类别，因为可以通过多种方式对其进行配置。

**灾难恢复和备份**-对于灾难恢复，hyper-v 副本会创建虚拟机的副本，这些副本应存储在其他物理位置，因此你可以从副本还原虚拟机。 对于备份，Hyper-v 提供了两种类型。 一个使用已保存状态，另一个使用卷影复制服务（VSS），因此你可以为支持 VSS 的程序进行应用程序一致的备份。

**优化**-每个受支持的来宾操作系统都具有一组自定义的服务和驱动程序（称为*integration services*），使你可以更轻松地使用 hyper-v 虚拟机中的操作系统。

可**移植性**功能，如实时迁移、存储迁移以及导入/导出功能，可更轻松地移动或分发虚拟机。

**远程连接**-Hyper-v 包含虚拟机连接，这是一种用于 Windows 和 Linux 的远程连接工具。 与远程桌面不同，此工具提供控制台访问权限，因此即使在操作系统尚未启动的情况下，也可以看到来宾发生了什么情况。

**安全**安全启动和受防护的虚拟机可帮助防止恶意软件和对虚拟机及其数据的其他未经授权的访问。

有关此版本中引入的功能的摘要，请参阅[Windows Server 上的 hyper-v 中的新增](What-s-new-in-Hyper-V-on-Windows.md)功能。 某些功能或部分限制了可配置的数量。 有关详细信息，请参阅[规划 Windows Server 2016 中的 hyper-v 可伸缩性](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。

## <a name="how-to-get-hyper-v"></a>如何获取 Hyper-v

Hyper-v 在 Windows Server 和 Windows 中提供，作为 Windows Server x64 版本可用的服务器角色。 有关服务器说明，请参阅[在 Windows server 上安装 hyper-v 角色](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。 在 Windows 上，它在某些64位版本的 Windows 中作为[功能](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)提供。 它还以可下载的独立服务器产品（ [Microsoft Hyper-V 服务器](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019)）提供。

## <a name="supported-operating-systems"></a>受支持的操作系统

许多操作系统将在虚拟机上运行。 通常，使用 x86 体系结构的操作系统将在 Hyper-v 虚拟机上运行。 但并不是所有可以运行的操作系统都进行了测试和支持。 有关支持的功能的列表，请参阅：

- [Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Windows Server 上的 Hyper-v 支持的 Windows 来宾操作系统](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Hyper-v 的工作方式

Hyper-v 是一种基于虚拟机监控程序的虚拟化技术。 Hyper-v 使用 Windows 虚拟机监控程序，这需要具有特定功能的物理处理器。 有关硬件的详细信息，请参阅[Windows Server 上的 Hyper-v 系统要求](System-requirements-for-Hyper-V-on-Windows.md)。

在大多数情况下，虚拟机监控程序管理硬件和虚拟机之间的交互。 此硬件监控程序控制的对硬件的访问为虚拟机提供了运行它们的隔离环境。 在某些配置中，虚拟机或虚拟机中运行的操作系统可以直接访问图形、网络或存储硬件。

## <a name="what-does-hyper-v-consist-of"></a>Hyper-v 包含哪些内容？

Hyper-v 具有协同工作的必需部件，因此你可以创建和运行虚拟机。 这些部件共同称为虚拟化平台。 当你安装 Hyper-v 角色时，它们将作为一个集进行安装。 必需的部分包括 Windows 虚拟机监控程序、Hyper-v 虚拟机管理服务、虚拟化 WMI 提供程序、虚拟机总线（VMbus）、虚拟化服务提供程序（VSP）和虚拟基础结构驱动程序（VID）。

Hyper-v 也提供管理和连接工具。 可以将它们安装在安装了 Hyper-v 角色的计算机上，也可以安装在未安装 Hyper-v 角色的计算机上。 这些工具包括：

- Hyper-V 管理器
- [Windows PowerShell 的 hyper-v 模块](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [虚拟机连接](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect)@no__t-名为 VMConnect @ no__t-2 的1sometimes
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>相关技术

这些是 Microsoft 中经常与 Hyper-v 一起使用的一些技术：

- [故障转移群集](../../failover-clustering/whats-new-in-failover-clustering.md)
- [远程桌面服务](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

各种存储技术：群集共享卷，SMB 3.0，存储空间直通

Windows 容器提供了另一种方法来实现虚拟化。 请参阅 MSDN 上的[Windows 容器](https://docs.microsoft.com/virtualization/windowscontainers/index)库。
