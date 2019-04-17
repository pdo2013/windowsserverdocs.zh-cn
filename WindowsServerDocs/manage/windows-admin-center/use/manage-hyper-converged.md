---
title: 管理超级集成基础架构使用 Windows Admin Center
description: 管理超级集成基础架构使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9ce4381735746ace6358aa2cb30f8b341c576054
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262840"
---
# 管理超级集成基础架构使用 Windows Admin Center

>适用于：Windows Admin Center、Windows Admin Center 预览版

## 什么是超级集成基础架构

软件定义的计算、 存储和网络到一个群集以提供高性能、 经济高效、 和易于扩展虚拟化，整合了有关超级集成基础架构。 使用[存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)、[软件定义的网络](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking)和[HYPER-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)的 Windows Server 2016 中引入了此功能。

> [!Tip]
> 想要获取超级集成基础架构？ Microsoft 建议从我们的合作伙伴这些[Windows Server 软件定义](https://microsoft.com/wssd)的解决方案。 它们设计、 组装和验证针对参考体系结构以确保兼容性和可靠性，以便获取和快速运行。

> [!IMPORTANT]
> 本文中所述的功能的一些是仅在 Windows Admin Center 预览版中可用。 [如何获取此版本？](http://aka.ms/windowsadmincenter)

## 什么是 Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md)是适用于 Windows Server，传统的"框中"工具如服务器管理器中的后续产品的下一代管理工具。 它是免费和可以安装和使用未连接到 Internet。 你可以使用 Windows Admin Center 来管理和监视运行 Windows Server 2016 或 Windows Server 2019 的超级集成基础架构。

![超聚合群集仪表板](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## 关键功能

Windows Admin Center 的超级集成基础架构的亮点包括：

- **统一的单窗格的控制台的计算、 存储和很快网络。** 查看你的虚拟机、 主机服务器、 卷、 驱动器和一个专门构建、 一致、 相互连接体验中的详细信息。
- **创建和管理存储空间和 HYPER-V 虚拟机。** 从根本上简单的工作流创建、 打开、 调整大小，并删除卷。创建、 开始菜单、 连接和移动虚拟机;以及更多。
- **强大监视群集范围。** 在仪表板图形内存和 CPU 使用率、 存储容量、 IOPS、 吞吐量和延迟以实时，通过在群集中，清除警报时出现不正确的每个服务器。
- **软件定义网络 (SDN) 支持。** 管理和监视虚拟网络，子网，将虚拟机连接到虚拟网络，并监视 SDN 基础结构。

Windows Admin Center 的超级集成基础架构是由 Microsoft 正在积极开发。 它将接收频繁的更新，以改善现有功能添加新功能。

## 开始之前

若要为 Windows Admin Center 中的超级集成基础架构管理群集，它需要运行 Windows Server 2016 或 Windows Server 2019，并且具有 HYPER-V 和存储空间直通启用。 （可选），它还可以具有软件定义的网络启用和通过 Windows Admin Center 进行管理。

> [!Tip]
> Windows Admin Center 还提供了一个常规用途的管理体验的 Windows Server 2012 及更高版本支持任何工作负荷，提供任何群集。 如果这听起来更适合，群集添加到 Windows Admin Center 时，请选择[**故障转移群集**](manage-failover-clusters.md)，而不是**超融合群集**。

### 为 Windows Admin Center 准备 Windows Server 2016 群集

Windows Admin Center 的超级集成基础架构取决于管理 Windows Server 2016 已发布后添加的 Api。 你可以管理 Windows Server 2016 群集使用 Windows Admin Center 之前，你将需要执行这两个步骤：

1. 验证群集中的每个服务器是否安装了[2018年-05 累积更新适用于 Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)或更高版本。 若要下载和安装此更新，请转到**设置** > **更新 & 安全** > **Windows 更新**和选择**检查联机来自 Microsoft 更新的更新**。
2. 在群集上以管理员身份运行以下 PowerShell cmdlet:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> 只需一次，群集中的任何服务器上运行 cmdlet。 可以在 Windows PowerShell 中本地运行它，或使用凭据安全服务提供程序 (CredSSP) 远程运行。 具体取决于你的配置，你可能无法在 Windows Admin Center 内运行此 cmdlet 从。

### 为 Windows Admin Center 准备 Windows Server 2019 群集

如果群集运行 Windows Server 2019，上述步骤不是必需的。 只需将群集添加到 Windows Admin Center 在下一节中所述，并且你正在正确运行 ！

### 配置软件定义网络 （可选） ###

你可以配置运行 Windows Server 2016 或 2019年执行以下步骤，使用软件定义网络 (SDN) 你超级集成基础架构：

1. 准备操作系统上的相同操作系统超级集成基础架构主机安装的 VHD。 此 VHD 将用于所有网关 NC/SLB Vm。
2. 下载所有文件夹和文件下 SDN Express 从[https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress)。
3. 准备使用部署控制台的不同 VM。 此 VM 应该能够访问 SDN 主机。 此外，虚拟机应具有安装 RSAT HYPER-V 工具。
4. 复制到部署控制台 VM 为 SDN Express 下载的所有内容。 和共享此**SDNExpress**文件夹。 确保每个主机可以访问**SDNExpress**共享的文件夹，在配置文件行 8 中定义：
```
    \\$env:Computername\SDNExpress
```
5. 将操作系统的 VHD 复制到部署主机虚拟机上**SDNExpress**文件夹下的**映像**文件夹。
6. 修改 SDN Express 配置与你的环境设置。 修改具体取决于你的环境的信息 SDN 快速配置之后，请完成以下两个步骤。
7. 使用管理员权限以部署 SDN 运行 PowerShell:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

部署需要大约 30-45 分钟。

## 入门

超级集成基础架构部署之后，你可以对使用 Windows Admin Center 进行管理。

### 安装 Windows Admin Center

如果尚未准备好，下载并安装 Windows Admin Center。 最快方式来获取和运行是 Windows 10 计算机上安装和远程管理服务器。 此操作将不超过 5 分钟。 [立即下载](https://aka.ms/windowsadmincenter)或[了解有关其他安装选项的详细信息](../deploy/install.md)。

### 添加超融合群集

若要将群集添加到 Windows Admin Center:

1. 单击下的所有连接的 **+ 添加**。
2. 选择要添加**超融合群集连接**。
3. 键入的群集名称以及出现提示时，要使用的凭据。
4. 单击**添加**完成。

群集将添加到你的连接列表。 单击它以启动在仪表板。

![添加超融合群集连接](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### 添加 SDN 启用超融合群集 （Windows Admin Center 预览版）

最新的 Windows Admin Center 预览版支持软件定义的网络管理超级集成基础架构。 通过添加超融合群集连接到网络控制器 REST URI，你可以使用超聚合群集管理器监视 SDN 基础结构并管理 SDN 的资源。

1. 单击下的所有连接的 **+ 添加**。
2. 选择要添加**超融合群集连接**。
3. 键入的群集名称以及出现提示时，要使用的凭据。
4. 检查**配置网络控制器**以继续。
5. 输入**网络控制器 URI** ，然后单击**验证**。
6. 单击**添加**完成。

群集将添加到你的连接列表。 单击它以启动在仪表板。

![添加 SDN 启用超融合群集连接](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> 目前不支持具有 Northbound 通信的 Kerberos 身份验证的 SDN 环境。

## 常见问题

### 是否有管理 Windows Server 2016 和 Windows Server 2019 之间的差异？

是。 Windows Admin Center 的超级集成基础架构接收频繁的更新改进 Windows Server 2016 和 Windows Server 2019 的体验。 但是，某些新功能将仅适用于 Windows Server 2019-例如，用于重复数据删除和压缩的切换开关。

### 可以使用 Windows Admin Center 管理存储空间直通对于其他使用情况 （不超聚合），如聚合横向扩展文件服务器 (SoFS) 或 Microsoft SQL Server？

Windows Admin Center 的超级集成基础架构不提供管理或专为存储空间直通其他用例的监视选项-例如，它不能创建的文件共享。 但是，仪表板和核心功能，例如创建卷或更换驱动器，适用于任何存储空间直通群集。

### 故障转移群集和超融合群集之间的区别是什么？

一般情况下，的术语"超聚合"是指在同一个运行 HYPER-V 和存储空间直通群集服务器虚拟化计算和存储资源。 在 Windows Admin Center 的上下文中，当你单击 **+ 添加**从连接列表中，你可以选择之间添加**故障转移群集连接**或**超融合群集连接**：

- **故障转移群集连接**是故障转移群集管理器的桌面应用程序的后续产品。 它提供支持任何工作负荷，包括 Microsoft SQL Server 任何群集的熟悉、 常规用途的管理体验。 它是适用于 Windows Server 2012 及更高版本。

- **超融合群集连接**是专为存储空间直通和 HYPER-V 定制的全新体验。 它采用仪表板并重点使用图表和警报进行监视。 它仅适用于 Windows Server 2016 和 Windows Server 2019。

### 为什么 Windows Server 2016 中需要最新的累积更新？

Windows Admin Center 的超级集成基础架构取决于管理 Api 开发自 Windows Server 2016 发布。 这些 Api 添加[2018年-05 累积更新适用于 Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)，从 2018 年 5 月 8 日起可用。

### 使用 Windows Admin Center 需要多少费用？

Windows Admin Center 不会在 Windows 以外产生额外费用。

你可以使用 Windows Server 或 Windows 10 的无额外费用的有效许可证使用 Windows Admin Center （可单独下载）-它通过 Windows Supplemental eula 许可。

### Windows Admin Center 是否需要 System Center？

不需要。

### 它确实需要连接到 Internet？

不需要。

尽管 Windows Admin Center 提供强大的 Microsoft Azure 云、 核心管理和监视体验超级集成基础架构的方便集成是完全在本地。 它可以安装和使用未连接到 Internet。

## 若要尝试的操作

如果你刚开始，下面是一些可帮助你了解如何为超级集成基础架构的 Windows Admin Center 组织和工作原理的快速教程。 请练习良好判断，请小心处理生产环境。 使用 Windows Admin Center 版本 1804年和 Windows Server 2019 Insider Preview 版本记录了这些视频。

### 管理存储空间直通卷

<ul>
               <li>(0:37)<a href="https://youtu.be/o66etKq70N8">如何创建三向镜像卷</a></li>
               <li>(1:17)<a href="https://youtu.be/R72QHudqWpE">如何创建镜像加速奇偶校验卷</a></li>
               <li>(1:02)<a href="https://youtu.be/j59z7ulohs4">如何打开一个卷，并添加文件</a></li>
               <li>(0:51)<a href="https://youtu.be/PRibTacyKko">如何启用重复数据删除和压缩</a></li>
               <li>(0:47)<a href="https://youtu.be/hqyBzipBoTI">如何扩展卷</a></li>
               <li>(0:26)<a href="https://youtu.be/DbjF8r2F6Jo">如何删除卷</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>创建三向镜像的卷</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>创建卷，镜像加速奇偶校验</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>打开卷，并添加文件</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>启用重复数据删除和压缩</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>扩展卷</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>删除卷</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### 创建新的虚拟机

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 在虚拟机工具的顶部，请选择**清单**选项卡，然后单击**新建**创建新的虚拟机。
3. 输入的虚拟机名称和第 1 和 2 代虚拟机之间进行选择。
4. 然后，你可以选择哪台主机最初上创建虚拟机或使用推荐的主机。
5. 选择虚拟机文件的路径。 从下拉列表中选择一个卷，或单击**浏览**选择使用文件夹选取器的文件夹。 虚拟机配置文件和虚拟硬盘文件将保存在单个文件夹下`\Hyper-V\[virtual machine name]`所选的卷或路径的路径。
6. 是否需要启用嵌套虚拟化、 配置内存设置、 网络适配器、 虚拟硬盘和选择是否想要从.iso 映像文件或从网络安装操作系统，请选择虚拟处理器的数量。
7. 单击**创建**以创建虚拟机。
8. 一旦虚拟机将创建并显示在虚拟机列表中，你可以启动虚拟机。
9. 一旦启动虚拟机，你可以连接到虚拟机的控制台通过 VMConnect 安装操作系统。 从列表中选择虚拟机中，单击**更多** > **Connect**下载.rdp 文件。 远程桌面连接应用中打开.rdp 文件。 由于这连接到虚拟机的主机，你将需要输入 HYPER-V 主机管理员凭据。

[了解有关使用 Windows Admin Center 的虚拟机管理详细信息](manage-virtual-machines.md)。

### 暂停和安全地重新启动服务器

1. 从**仪表板**中，从左侧或通过单击右下角的仪表板中的磁贴上**查看服务器 >** 链接导航中选择**服务器**。
2. 在顶部，从**摘要**切换到**清单**选项卡。
3. 通过单击其名称以打开**服务器**详细信息页面上选择一个服务器。
4. 单击**暂停服务器以进行维护**。 如果它是安全继续，这将移动到群集中的其他服务器的虚拟机。 服务器必须将状态清空而发生这种情况。 如果需要，你可以观看移动在**虚拟机 > 库存**页面上，其中其主机服务器清楚地显示在网格中的虚拟机。 当所有虚拟机已都移动时，服务器状态将**已暂停**。
5. 单击要访问 Windows Admin Center 中的所有每个服务器管理工具的**管理服务器**。
6. 单击**重新启动**，然后****。 你将返回踢到连接列表。
7. 又重新打开**仪表板**中，为红色服务器关闭时。
8. 备份后，重新定位**服务器**页，然后单击**从维护恢复服务器**的服务器状态设置回只需保持。 此时，虚拟机将后退 – 不不需要任何用户操作。

### 更换故障的驱动器

1. 当一个驱动器出现故障，在**仪表板**的左上方**通知**区域中会显示一个警告。
2. 你还可以从左侧导航中选择**驱动器**，或单击右下角浏览驱动器以及自行查看其状态中的磁贴上的**视图驱动器 >** 链接。 在**清单**选项卡中，网格支持排序、 分组和关键字。
3. 从**仪表板**中，单击警报以查看详细信息，如驱动器的物理位置。
4. 若要了解详细信息，单击**驱动器**详细信息页面**转到驱动器**的快捷方式。
5. 如果你的硬件支持，你可以单击**打开/关闭的光**来控制驱动器的指示器光。
6. 存储空间直通自动淘汰和 evacuates 故障的驱动器。 当此情况时，驱动器状态已停用，并且其存储容量栏为空。
7. 删除失败的驱动器，并将插入其替换。
8. 在**驱动器 > 清单**中将显示新的驱动器。 时，会清除警报、 卷将返回到正常状态，修复和存储到新驱动器将重新平衡 – 不不需要任何用户操作。

### 管理虚拟网络 （SDN 启用 HCI 群集使用 Windows Admin Center 预览版）

1. 从左侧导航中选择**虚拟网络**。
2. 单击**新建**创建新的虚拟网络和子网，或选择一个现有的虚拟网络并单击要修改其配置的**设置**。
3. 单击了现有的虚拟网络，以查看虚拟机连接到虚拟网络子网和访问控制列表应用到虚拟网络子网。

![管理虚拟网络](../media/manage-hyper-converged/manage-virtual-networks.png)

### 将虚拟机连接到虚拟网络 （SDN 启用 HCI 群集使用 Windows Admin Center 预览版）

1. 从左侧导航中选择**虚拟机**。
2. 选择现有的虚拟机 > 单击**设置**> 在**设置**中打开**网络**选项卡。
3. 配置要连接到虚拟网络的虚拟机的**虚拟网络**和**虚拟子网**的字段。

创建虚拟机时，你还可以配置虚拟网络。

![将虚拟机连接到虚拟网络](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### 监视软件定义的网络基础结构 （使用 Windows Admin Center 预览版支持 SDN 的 HCI 群集）

1. 从左侧导航中选择**SDN 监视**。
2. 查看有关运行状况的网络控制器、 软件负载平衡器，虚拟网关的详细的信息和监视虚拟网关池、 公钥和私钥 IP 池的使用情况和 SDN 主机状态。

![监视器 SDN 基础结构](../media/manage-hyper-converged/sdn-monitoring.png)

## 反馈

它是所有有关你的反馈 ！ 频繁更新的最重要的一点是听到正在处理的内容和需要改进的内容。 下面是一些方法，让我们知道你正在考虑：

- [提交并在 UserVoice 上的功能请求进行表决](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Windows Admin Center 论坛上 Microsoft 技术社区](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 到推文 `@servermgmt`

### 另请参阅

- [Windows Admin Center](../understand/windows-admin-center.md)
- [存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [软件定义的网络](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
