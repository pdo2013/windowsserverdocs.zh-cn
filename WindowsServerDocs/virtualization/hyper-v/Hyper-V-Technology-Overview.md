---
title: HYPER-V 技术概述
description: 介绍了 Hyper-v 的目标是什么，如何获取其主要功能和常见用法。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 9ae3c9dce36ad7d67a19ce167c9cb875b3c91810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864308"
---
# <a name="hyper-v-technology-overview"></a>HYPER-V 技术概述

>适用于：Windows Server 2016 中，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

HYPER-V 是 Microsoft 的硬件虚拟化产品。 它允许您创建和运行软件版本的计算机，称为*虚拟机*。 每个虚拟机的作用类似于运行的操作系统和程序的完整计算机。 当您需要计算资源时，虚拟机为您提供更大的灵活性、 帮助节省时间和金钱，和是更有效地使用比只需在物理硬件上运行操作系统的硬件。

HYPER-V 在其自己独立的空间，这意味着可以在同一时间在同一硬件上运行多个虚拟机中运行每个虚拟机。 您可能想要执行此以避免发生问题，例如故障会影响其他工作负载，或者为不同的用户、 组或服务访问提供到不同的系统操作。

## <a name="some-ways-hyper-v-can-help-you"></a>某些方面的 HYPER-V 可帮助你

HYPER-V 可以帮助你：

- **建立或扩展私有云环境。** 提供将移到或扩展共享资源的使用更加灵活、 按需 IT 服务并调整利用率需求变化。

- **更有效地使用您的硬件。** 合并服务器和工作负荷到更少、 功能更强大的物理计算机可以使用更少的电源和物理空间。

- **提高业务连续性。** 计划和非计划停机对工作负载的影响降至最低。

- **建立或扩展虚拟桌面基础结构 (VDI)。** 使用与 VDI 的集中式桌面策略可以帮助您提高业务灵活性和数据安全性，以及简化法规遵从性和管理桌面操作系统和应用程序。 若要向用户提供个人虚拟机或虚拟机池在同一服务器上部署的 HYPER-V 和远程桌面虚拟化主机 （RD 虚拟化主机）。

- **使开发和测试效率更高。** 重现无需购买和维护如果仅使用物理系统需要的所有硬件的不同计算环境。

## <a name="hyper-v-and-other-virtualization-products"></a>HYPER-V 和其他虚拟化产品

Windows 和 Windows Server 中的 HYPER-V 将替换较旧的硬件虚拟化产品，如 Microsoft Virtual PC、 Microsoft Virtual Server 和 Windows Virtual PC。 HYPER-V 提供网络、 性能、 存储和安全功能在这些较旧的产品中不可用。

HYPER-V 和需要相同的处理器功能的大多数第三方虚拟化应用程序不兼容。 这是因为处理器功能，称为硬件虚拟化扩展，旨在不能共享。 有关详细信息，请参阅[虚拟化应用程序不起作用的 HYPER-V、 Device Guard 和 Credential Guard](https://support.microsoft.com/kb/3204980)。

## <a name="what-features-does-hyper-v-have"></a>HYPER-V 有哪些功能？

HYPER-V 提供了许多功能。 这是一个概述，按功能提供或帮助你进行分组。

**计算环境**-HYPER-V 虚拟机包括为物理计算机，如内存、 处理器、 存储和网络相同的基本部分。 所有这些部分的功能和选项，您可以配置不同的方式以满足不同的需求。 存储和网络可以每个被视为其自己的类别，因为许多方式可以配置它们。

**灾难恢复和备份**-对于灾难恢复，HYPER-V 副本创建的虚拟机，用于存储在另一个物理位置，以便您可以从副本中还原虚拟机的副本。 对于备份，HYPER-V 提供了两种类型。 一个使用已保存的状态和其他使用卷影复制服务 (VSS)，因此您可以进行应用程序一致备份的程序的支持 vss。

**优化**-每个受支持的来宾操作系统具有一组自定义的服务和驱动程序，称为*integration services*，，使其更易于使用的 HYPER-V 虚拟机中的操作系统。

**可移植性**-等功能实时迁移、 存储迁移，并导入/导出轻松地移动或分配虚拟机。

**远程连接**-HYPER-V 包括虚拟机连接，适用于 Windows 和 Linux 的远程连接工具。 因此与不同的远程桌面，这个工具即可通过控制台访问权限，可以看到发生在来宾中即使操作系统不尚未启动。

**安全**-安全启动和受防护的虚拟机帮助保护免受恶意软件和其他未经授权的访问虚拟机和其数据。

在此版本中引入的功能的摘要，请参阅[什么是 Windows Server 上的 HYPER-V 中的新增功能](What-s-new-in-Hyper-V-on-Windows.md)。 某些功能或部分具有数量可以配置限制。 有关详细信息，请参阅[规划 Windows Server 2016 中的 HYPER-V 可伸缩性](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。

## <a name="how-to-get-hyper-v"></a>如何获取 HYPER-V

HYPER-V 是 Windows Server 和 Windows 中, 作为服务器角色适用于 x64 版本的 Windows Server。 有关服务器的说明，请参阅[Windows Server 上安装 HYPER-V 角色](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。 在 Windows，则可以用作[功能](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)在某些 64 位版本的 Windows。 此外，还可以作为可下载的独立服务器产品， [Microsoft HYPER-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019)。

## <a name="supported-operating-systems"></a>受支持的操作系统

许多操作系统将虚拟机上运行。 一般情况下，操作系统使用的 x86 体系结构将在 HYPER-V 虚拟机运行。 测试和 Microsoft 支持，但是并非所有可以运行的操作系统。 有关支持的功能列表，请参阅：

- [Windows 上的 HYPER-V 的受支持的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [支持的 Windows 来宾操作系统为 Windows Server 上的 HYPER-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>HYPER-V 的工作原理

HYPER-V 是基于虚拟机监控程序的虚拟化技术。 HYPER-V 使用 Windows 虚拟机监控程序，这需要具有特定功能的物理处理器。 有关硬件的详细信息，请参阅[System requirements for Windows Server 上的 HYPER-V 要求](System-requirements-for-Hyper-V-on-Windows.md)。

在大多数情况下，虚拟机监控程序管理硬件和虚拟机之间的交互。 此虚拟机监控程序控制访问硬件为虚拟机提供了运行它们的隔离的环境。 在某些配置中，虚拟机或虚拟机中运行的操作系统具有直接访问图形、 网络或存储硬件。

## <a name="what-does-hyper-v-consist-of"></a>HYPER-V 的包含什么？

HYPER-V 要求部分协同工作，因此可以创建并运行虚拟机。 总之，这些部分都称为虚拟化平台。 它们作为一组安装时安装 HYPER-V 角色。 所需的部件包括 Windows 虚拟机监控程序、 HYPER-V 虚拟机管理服务、 虚拟化 WMI 提供程序、 虚拟机总线 (VMbus)、 虚拟化服务提供商 (VSP) 和虚拟基础结构驱动程序 (VID)。

HYPER-V 还包含用于管理和连接工具。 您可以将它们安装 HYPER-V 角色安装了和的计算机上，而无需安装了 HYPER-V 角色在同一计算机上。 这些工具包括：

- Hyper-V 管理器
- [适用于 Windows PowerShell 的 HYPER-V 模块](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [虚拟机连接](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect)\(有时称为 VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>相关技术

这些是经常与 Hyper-v： 使用 Microsoft 的某些技术

- [故障转移群集](../../failover-clustering/whats-new-in-failover-clustering.md)
- [远程桌面服务](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

各种存储技术： 群集共享卷，SMB 3.0 存储空间直通

Windows 容器提供虚拟化的另一种方法。 请参阅[Windows 容器](https://docs.microsoft.com/virtualization/windowscontainers/index)MSDN 上的库。
