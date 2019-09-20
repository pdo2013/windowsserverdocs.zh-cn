---
title: Windows Server 升级概述 |Microsoft Docs
description: 了解一些常规 Windows Server 升级信息，以及在执行实际升级之前应考虑的事项。
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124787"
---
# <a name="overview-about-windows-server-upgrades"></a>Windows Server 升级概述

升级到较新版本的 Windows Server 的过程可能会有很大差异，具体取决于你开始使用的操作系统和所采用的路径。 我们使用以下术语来区分不同的操作，这些操作可能涉及到新的 Windows Server 部署。

- **升级.** 也称为 "就地升级"。 从较旧版本的操作系统移动到较新版本，同时保持在同一物理硬件上。 **本部分将介绍这种方法。**

    >[!Important]
    >公有或私有云公司也可能支持就地升级;但是，你必须咨询你的云提供商以了解详细信息。 此外，你将无法在配置为**从 VHD 启动**的任何 Windows Server 上执行就地升级。

- **安装.** 也称为 "全新安装"。 从较旧版本的操作系统移动到较新的版本，从而删除了较旧的操作系统。

- **迁移.** 通过传输到一组不同的硬件或虚拟机，你可以从较旧版本的操作系统移动到较新版本的操作系统。

- **群集操作系统滚动升级。** 升级群集节点的操作系统，而不停止 Hyper-v 或横向扩展文件服务器工作负荷。 利用此功能可以避免出现可能影响服务级别协议的故障时间。 有关详细信息，请参阅[群集操作系统滚动升级](../failover-clustering/cluster-operating-system-rolling-upgrade.md)

- **许可转换。** 使用简单的命令和相应的许可证密钥，通过一个步骤将发行版的特定版本转换为同一发行版的另一个版本。 我们称之为 "许可证转换"。 例如，如果服务器正在运行 Windows Server 2016 Standard，可以将其转换为 Windows Server 2016 Datacenter。

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>应将哪个版本的 Windows Server 升级到？

建议升级到最新版本的 Windows Server：Windows Server 2019。 运行最新版本的 Windows Server 允许你使用最新功能（包括最新的安全功能），并提供最佳性能。

但是，我们意识到并非总是可行。 你可以使用下图来确定你可以升级到哪个 Windows Server 版本，具体取决于你当前所处的版本：

![可用的就地升级路径](media/upgrade-paths.png)

通常，Windows Server 可通过至少一种升级，甚至是在两个版本中进行升级。 例如，可以将 Windows Server 2012 R2 和 Windows Server 2016 就地升级到 Windows Server 2019。

你还可以从操作系统的评估版升级到零售版，从较旧的零售版升级到较新的版本，或者在某些情况下从操作系统的批量许可版升级到普通零售版。 有关就地升级以外升级选项的详细信息，请参阅[Windows Server 的升级和转换选项](../get-started/supported-upgrade-paths.md)。
