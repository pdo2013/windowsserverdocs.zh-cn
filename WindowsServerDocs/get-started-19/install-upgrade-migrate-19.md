---
title: 安装 | 升级 | 迁移到 Windows Server 2019
description: 如何全新安装、就地升级或迁移到 Windows Server 2019。
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
ms.openlocfilehash: 1fd955a640832eb161666f74b93d91bb2c3eff11
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810817"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>安装 | 升级 | 迁移到 Windows Server 2019

>适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

> [!IMPORTANT]
> Windows Server 2008 R2 和 Windows Server 2008 的扩展支持将于 2020 年 1 月结束。 [了解升级选项](http://aka.ms/upgradecenter)。

是时候移动到较新版本的 Windows Server 了吗？ 根据你现在正在运行的内容，你有很多选择来实现这一点。

## <a name="clean-install"></a>全新安装
如果你想要在同一硬件上从较旧版本的 Windows Server 移动到 Windows Server 2019，应进行全新安装  ，通过该方法，你只需在同一硬件上的旧操作系统上直接安装较新的操作系统即可，这样可删除先前的操作系统。 这是最简单的方法，但需要先备份数据，并计划重新安装应用程序。 要注意几个方面，例如系统要求，因此务必查看有关 [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124)、[Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558)、[Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) 和 [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx) 的详细信息。

## <a name="in-place-upgrade"></a>就地升级

如果希望保留相同的硬件和所有已设置的服务器角色，而不平展服务器，则需执行就地升级  ，从旧操作系统升级到新操作系统，同时使设置、服务器角色和数据保持不变。 例如，如果服务器正在运行 Windows Server 2012 R2，可以升级到 Windows Server 2016 或 Windows Server 2019。 但是，并非每个更低版本的操作系统都拥有升级到每个更高版本的操作系统的路径。 请参阅下图以了解可用的升级路径：

![Windows Server 就地升级路径关系图](media/upgrade-paths.png)

有关升级的分步指南，请访问 [Windows Server 升级中心](http://aka.ms/upgradecenter)：

[![Windows Server 升级中心的屏幕截图](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>群集操作系统滚动升级

群集操作系统滚动升级允许管理员将群集节点的操作系统从 Windows Server 2012 R2 和 Windows Server 2016 进行升级，且无需中断 Hyper-V 或横向扩展文件服务器工作负荷。 利用此功能可以避免出现可能影响服务级别协议的故障时间。 [群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) 中对这一新增功能进行了更详细地讨论。

## <a name="migration"></a>迁移

Windows Server 迁移可帮助你每次从运行 Windows Server 的源计算机将一个角色或功能移动到运行相同或更高版本的 Windows Server 的目标计算机。 出于这些目的，迁移被定义为将一个角色或功能及其数据移动到其他计算机，而不是在同一计算机上升级此功能。 

## <a name="license-conversion"></a>许可证转换
在某些操作系统发行版中，可以使用简单的命令和相应的许可证密钥，通过一个步骤将发行版的特定版本转换成同一发行版的另一个版本。 这称为“许可证转换”  。 例如，如果服务器正在运行 Windows Server 2016 Standard，可以将其转换为 Windows Server 2016 Datacenter。 请记住，尽管可以从 Server 2016 Standard 升级到 Server 2016 Datacenter，但不能逆转此过程，转而从 Datacenter 转换为 Standard。 在某些版本的 Windows Server 中，你还可使用相同命令和相应的密钥在 OEM、批量许可和零售版本之间自由地转换。


 
 
