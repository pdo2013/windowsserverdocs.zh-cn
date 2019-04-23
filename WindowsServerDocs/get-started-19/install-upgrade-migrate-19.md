---
title: 安装 |升级 |迁移到 Windows Server 2019
description: 如何全新安装、 就地升级或迁移到 Windows Server 2019。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 58c363fc0a1e336519bc6ec4276651345cc2b5eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868708"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>安装 |升级 |迁移到 Windows Server 2019

>适用于：Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

> [!IMPORTANT]
> Windows Server 2008 R2 和 Windows Server 2008 的扩展支持将于 2020 年 1 月结束。 [了解您的升级选项](http://aka.ms/upgradecenter)。

是时候移动到较新版本的 Windows Server 了吗？ 根据你现在正在运行的内容，你有很多选择实现这一点。

## <a name="clean-install"></a>清理安装
如果你想要将移动从较旧版本的 Windows Server 到 Windows Server 2019 在同一硬件上，您应该做**全新安装**，其中只需直接覆盖旧的相同硬件上安装较新的操作系统因此删除以前的操作系统。 这是最简单的方法，但你将需要先备份你的数据，然后计划重新安装你的应用程序。 有需要注意的如系统要求，因此请确保检查的详细信息的一些事项[Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124)， [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558)， [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418)并[Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。

## <a name="in-place-upgrade"></a>就地升级
如果你想要保留相同的硬件和而无需平展服务器设置的所有服务器角色，您需要执行操作**就地升级**，通过从较旧转的操作系统到较新版本，使你的设置，服务器角色和数据保持不变。 例如，如果你的服务器正在运行 Windows Server 2012 R2，您可以升级到 Windows Server 2016 或 Windows Server 2019。 但是，并非每个更低版本的操作系统都拥有升级到每个更高版本的操作系统的路径。 请参阅下图以了解可用的升级路径：

![Windows Server 的就地升级路径关系图](media/upgrade-paths.png)

有关升级的分步指导，请访问[Windows Server 升级中心](http://aka.ms/upgradecenter):

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Windows Server 升级中心"></a>

## <a name="cluster-os-rolling-upgrade"></a>群集操作系统滚动升级
群集操作系统滚动升级使管理员能够从 Windows Server 2012 R2 和 Windows Server 2016 升级群集节点的操作系统，而无需停止 HYPER-V 或横向扩展文件服务器工作负荷。 利用此功能可以避免出现可能影响服务级别协议的故障时间。 [群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) 中对这一新增功能进行了更详细地讨论。

## <a name="migration"></a>迁移

当一个角色或功能一次移动到另一台运行 Windows Server，相同或较新版本的目标计算机运行 Windows Server 的源计算机时，Windows Server 迁移。 出于这些目的，迁移被定义为将一个角色或功能及其数据移动到其他计算机，而不是在同一计算机上升级此功能。 

## <a name="license-conversion"></a>许可证转换
在某些操作系统发行版中，可以使用简单的命令和相应的许可证密钥，通过一个步骤将发行版的特定版本转换成同一发行版的另一个版本。 这称为**许可证转换**。 例如，如果服务器正在运行 Windows Server 2016 Standard，可以将其转换为 Windows Server 2016 Datacenter。 在某些版本的 Windows Server 中，你还可使用相同命令和相应的密钥在 OEM、批量许可和零售版本之间自由地转换。


 
 
