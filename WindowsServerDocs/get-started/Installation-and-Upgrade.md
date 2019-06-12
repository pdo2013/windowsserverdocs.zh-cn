---
title: Windows Server 安装和升级
description: 如何安装、 升级或迁移到 Windows Server 的较新版本。
ms.prod: windows-server
ms.date: 05/14/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 140f67a9dab5cf1f10cdb0c5c51a031a0dfb9dd3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443553"
---
# <a name="windows-server-installation-and-upgrade"></a>Windows Server 安装和升级

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2 和 Windows Server 2008

要查找 Windows 服务器 2019年？ 请参阅[安装，请升级或迁移到 Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md)。

> [!IMPORTANT]
> Windows Server 2008 R2 和 Windows Server 2008 的扩展支持将于 2020 年 1 月结束。 [了解您的升级选项](#upgrading-from-windows-server-2008-r2-or-windows-server-2008)。

是时候移动到较新版本的 Windows Server 了吗？ 根据你现在正在运行的内容，你有很多选择实现这一点。

## <a name="installation"></a>安装

如果你想要在同一硬件上移动到较新版本的 Windows Server，始终有效的一种方法是**全新安装**，通过该方法，你只需在同一硬件上的旧操作系统上直接安装较新的操作系统即可，这样可删除先前的操作系统。 这是最简单的方法，但你将需要先备份你的数据，然后计划重新安装你的应用程序。 要注意几个方面，例如系统要求，因此务必查看有关 [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558)、[Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) 和 [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx) 的详细信息。

从任何预发布版本（如 Windows Server 2016 Technical Preview) 移动到已发布版本 (Windows Server 2016) 始终需要进行全新安装。

## <a name="migration-recommended-for-windows-server-2016"></a>迁移（建议对 Windows Server 2016 执行此操作）

Windows Server 迁移文档可帮助你从运行 Windows Server，相同或较新版本的另一台目标计算机运行 Windows Server 的源计算机一次迁移一个角色或功能。 出于这些目的，迁移被定义为将一个角色或功能及其数据移动到其他计算机，而不是在同一计算机上升级此功能。 这是将你的现有工作负荷和数据移动到较新版本 Windows Server 的建议方式。 若要开始，请[服务器角色升级和迁移矩阵](https://go.microsoft.com/fwlink/?LinkId=828595)适用于 Windows Server。

## <a name="cluster-os-rolling-upgrade"></a>群集操作系统滚动升级
群集操作系统滚动升级是 Windows Server 2016 中的新增功能，管理员利用此功能可以将群集节点的操作系统从 Windows Server 2012 R2 升级到 Windows Server 2016，而无需停止 Hyper-v 或横向扩展文件服务器工作负荷。 利用此功能可以避免出现可能影响服务级别协议的故障时间。 [群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) 中对这一新增功能进行了更详细地讨论。

## <a name="license-conversion"></a>许可证转换
在某些操作系统发行版中，可以使用简单的命令和相应的许可证密钥，通过一个步骤将发行版的特定版本转换成同一发行版的另一个版本。 这称为**许可证转换**。 例如，如果服务器正在运行 Windows Server 2016 Standard，可以将其转换为 Windows Server 2016 Datacenter。 在某些版本的 Windows Server 中，你还可使用相同命令和相应的密钥在 OEM、批量许可和零售版本之间自由地转换。

## <a name="upgrade"></a>升级
如果你想要保留相同硬件以及你已设置的所有服务器角色，同时不扁平化服务器，则可选择**升级**，有许多方法来执行此操作。 在典型升级中，可从更低版本的操作系统升级到更高版本的操作系统，并且使设置、服务器角色和数据保持完整。 例如，如果你的服务器运行的是 Windows Server 2012 R2，你可以将它升级到 Windows Server 2016。 但是，并非每个更低版本的操作系统都拥有升级到每个更高版本的操作系统的路径。
 
>[!NOTE]
>升级最适合用于虚拟机，其中进行成功升级不需要特定 OEM 硬件驱动程序。
 
可以从操作系统评估版升级到零售版，从早期的零售版升级到较新版本，在某些情况下，还可以从操作系统批量授权版升级到普通零售版。

开始升级之前，先看看此页面上的表，以便了解如何从你所在的位置升级到你要到达的位置。

有关适用于 Windows Server 2016 Technical Preview 的安装选项之间的差异，包括每个选项安装的功能、安装后可用的管理选项，请参阅 [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598)。

>[!NOTE]
>每当迁移到或升级到任何版本的 Windows Server 时，都应查看并了解[支持生命周期策略](https://support.microsoft.com/lifecycle)以及该版本的时间范围，并且作出相应的计划。 你可以[搜索生命周期信息](https://support.microsoft.com/lifecycle)，以便了解你感兴趣的特定 Windows server 版本。
 
 
## <a name="upgrading-to-windows-server-2016"></a>升级到 Windows Server 2016
有关详细信息，包括重要注意事项和升级限制、Windows Server 2016 版本之间的许可证转换，以及评估版到零售版之间的转换，请参阅 [Windows Server 2016 的受支持升级路径](https://go.microsoft.com/fwlink/?LinkId=828602)。
 
>[!NOTE]
>注意：不支持从“服务器核心安装”切换到“带桌面安装的服务器”的升级（反之亦然）。 如果你正在升级或转换的更低版本操作系统是服务器核心安装，则结果仍将是更高版本操作系统的服务器核心安装。
 
从先前 Windows Server 零售版到 Windows Server 2016 零售版本的受支持升级路径的快速参考表：


|如果你正在运行这些版本，则：|可以升级到这些版本：|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016（使用群集操作系统滚动升级功能）|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### <a name="license-conversion"></a>许可证转换
可以将 Windows Server 2016 Standard（零售版）转换为 Windows Server 2016 Datacenter（零售版）。

可以将 Windows Server 2016 Essentials（零售版）转换为 Windows Server 2016 Standard（零售版）。

可以将 Windows Server 2016 Standard 的评估版转换为 Windows Server 2016 Standard（零售版）或 Datacenter（零售版）。

可以将 Windows Server 2016 Datacenter 的评估版转换为 Windows Server 2016 Datacenter（零售版）。
 
## <a name="upgrading-to-windows-server-2012-r2"></a>升级到 Windows Server 2012 R2
有关详细信息，包括重要注意事项和升级限制、Windows Server 2012 R2 版本之间的许可证转换，以及评估版到零售版之间的转换，请参阅 [Windows Server 2012 R2 的升级选项](https://technet.microsoft.com/library/dn303416.aspx)。

从先前 Windows Server 零售版到 Windows Server 2012 R2 零售版本的受支持升级路径的快速参考表：

|如果运行的是：|可以升级到这些版本：|
|-------------------------|---------------------------|
|带有 SP1 的 Windows Server 2008 R2 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Enterprise SP1|Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter|
|带有 SP1 的 Windows Server 2008 R2 Standard|Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter|
|Windows Web Server 2008 R2 SP1|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### <a name="license-conversion"></a>许可证转换
可以将 Windows Server 2012 Standard（零售版）转换为 Windows Server 2012 Datacenter（零售版）。

可以将 Windows Server 2012 Essentials（零售版）转换为 Windows Server 2012 Standard（零售版）。

可以将 Windows Server 2012 Standard 的评估版转换为 Windows Server 2012 Standard（零售版）或 Datacenter（零售版）。

## <a name="upgrading-to-windows-server-2012"></a>升级到 Windows Server 2012
有关详细信息，包括重要注意事项和升级限制，以及评估版到零售版之间的转换，请参阅[Windows Server 2012 的评估版和升级选项](https://technet.microsoft.com/library/jj574204.aspx)。
 
从先前 Windows Server 零售版到 Windows Server 2012 零售版本的受支持升级路径的快速参考表：

|如果运行的是：|可以升级到这些版本：|
|--------------------------|--------------------------|
|带有 SP2 的 Windows Server 2008 Standard 或带有 SP2 的 Windows Server 2008 Enterprise|Windows Server 2012 Standard、Windows Server 2012 Datacenter|
|带有 SP2 的 Windows Server 2008 Datacenter|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|带有 SP1 或 Windows Server 2008 R2 Enterprise SP1 的 Windows Server 2008 R2 Standard|Windows Server 2012 Standard、Windows Server 2012 Datacenter|
|带有 SP1 的 Windows Server 2008 R2 Datacenter|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>许可证转换
可以将 Windows Server 2012 Standard（零售版）转换为 Windows Server 2012 Datacenter（零售版）。

可以将 Windows Server 2012 Essentials（零售版）转换为 Windows Server 2012 Standard（零售版）。

可以将 Windows Server 2012 Standard 的评估版转换为 Windows Server 2012 Standard（零售版）或 Datacenter（零售版）。

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>从 Windows Server 2008 R2 或 Windows Server 2008 升级

如中所述[升级 Windows Server 2008 和 Windows Server 2008 R2](modernize-windows-server-2008.md)，Windows Server 2008 R2/Windows Server 2008 的扩展的支持将在 2020 年 1 月结束。 若要不确保在支持任何间隙，您需要升级到支持的版本的 Windows Server 或重新托管在 Azure 中通过将移到[专用 Windows Server 2008 R2 虚拟机](uploading-specialized-WS08-image-to-azure.md)。 请查看[Windows Server 迁移指南](https://go.microsoft.com/fwlink/?linkid=872689)有关信息和计划迁移/升级注意事项。

对于在本地服务器，是没有直接的升级路径从 Windows Server 2008 R2 到 Windows Server 2016 或更高版本。 相反，升级到 Windows Server 2012 R2 的第一次，然后[升级到 Windows Server 2016](#upgrading-to-windows-server-2016)。

如你计划升级，请注意的中间步骤升级到 Windows Server 2012 R2 的以下指导原则。

  - 无法从 32 位到 64 位体系结构进行就地升级，或从一个内部版本到另一个 (如 fre 到 chk) 的类型。

  - 以相同的语言仅支持就地升级。 不能从一种语言升级到另一个。

  - 无法迁移到 Windows Server 2012 R2 服务器 GUI （Windows Server 中称为"完整桌面的服务器使用"） 从 Windows Server 2008 服务器核心安装。 您可以切换到与完整的桌面，但只能在 Windows Server 2012 R2 上的服务器已升级的服务器核心安装。 Windows Server 2016 及更高版本*不这样做*支持从服务器核心切换到完整的桌面，因此请该交换机，然后再升级到 Windows Server 2016。
  
有关详细信息，请查看[评估版升级为 Windows Server 2012 和选项](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\))，其中包括特定于角色的升级的详细信息。

