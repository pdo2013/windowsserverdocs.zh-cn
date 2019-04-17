---
title: 升级 Windows Server 2008 和 Windows Server 2008 R2
description: Windows Server 2008 和 2008 R2 的服务即将结束。 了解如何在本地升级或重新托管到 Azure。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 4127eab613abb429a200f513a11b944e05da0f76
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339365"
---
# 升级 Windows Server 2008 和 Windows Server 2008 R2

Windows Server 2008 和 Windows Server 2008 R2 的扩展支持将于 2020 年 1 月 14 日结束。 有两种现代化路径可以使用：本地升级或通过在 Azure 中重新托管来进行迁移。 **如果在 Azure 中重新托管，则可免费迁移现有的服务器映像。**

![描述从 Windows Server 2008 升级的升级路径流程图](media/WS08_upgrade_paths.png)


## 本地升级
如果需要使服务器保留在本地，并且运行的是 Windows Server 2008 或 Windows Server 2008 R2，则需要先[升级到 Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2)，然后才能[升级到 Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016)。 升级时，仍可选择通过重新托管迁移到 Azure。

有关本地升级选项的更多信息，请参阅[从 Windows Server 2008 R2 或 Windows Server 2008 升级](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008)。

如果你运行的是 Windows Server 2003，则需要[升级到 Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10))。 有关本地升级选项的更多信息，请参阅 [Windows Server 2008 的升级路径](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10))。


## 迁移到 Azure
将本地 Windows Server 2008 和 Windows Server 2008 R2 服务器迁移到 Azure 后，你可以继续在虚拟机上运行这些服务器。 在 Azure 中，你将确保服务器的兼容性并提高其安全性，并可在工作中增加云创新。 迁移到 Azure 的好处包括：

- Azure 中的安全更新。
- 获取额外三年的 Windows Server 2008 R2 或 2008 关键和重要安全更新，且不会产生任何附加费用。 
- Azure 中的免费升级。
- 在你准备就绪后可以采用更多云服务。
- 通过将 SQL Server 迁移到 Azure 托管实例或虚拟机，可以获取额外三年的 Windows Server 2008 R2 或 2008 关键安全更新，且不会产生任何附加费用。 
- 将现有的 SQL Server 和 Windows Server 许可证用于云，从而节省成本，这是 Azure 特有的。

<a href="uploading-specialized-WS08-image-to-azure.md"><img src="media/WS08-image-banner-small.png"></a>

若要开始迁移，请参阅[将 Azure Windows Server 2008/2008 R2 专用映像上传至 Azure](uploading-specialized-WS08-image-to-azure.md)。

为了帮助你理解如何分析现有的 IT 资源、评估你所拥有的资源、确定将特定服务和应用程序移至云或将工作负载保留在本地并升级到 Windows Server 最新版本的好处，请参阅 [Windows Server 迁移指南](https://go.microsoft.com/fwlink/?linkid=872689)。


## 与 Windows Server 并行升级 SQL Server 2008/2008 R2

![SQL Server 徽标](media/sqlr2.jpg)

如果正在运行 SQL Server 2008/2008 R2，则可以升级到 SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) 或 [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017)。


## 其他资源
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)