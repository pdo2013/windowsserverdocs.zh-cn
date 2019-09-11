---
title: 通过 Windows 管理中心管理超聚合基础结构
description: 通过 Windows 管理中心管理超聚合基础结构（Project Honolulu）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 5df035b448b80aa147067004c6a2f14aa03a9684
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869085"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>通过 Windows 管理中心管理超聚合基础结构

>适用于：Windows Admin Center、Windows Admin Center 预览版

## <a name="what-is-hyper-converged-infrastructure"></a>什么是超聚合基础结构

超聚合基础结构将软件定义的计算、存储和网络整合到一个群集中，以提供高性能、经济高效且易于缩放的虚拟化。 此功能是在 Windows Server 2016 中引入的，其中[存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)、[软件定义的网络](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)和[hyper-v](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)。

> [!Tip]
> 想要获取超聚合基础结构？ Microsoft 建议合作伙伴[提供这些 Windows Server 软件定义](https://microsoft.com/wssd)的解决方案。 它们针对我们的参考体系结构进行设计、汇编和验证，以确保兼容性和可靠性，使你能够快速启动和运行。

> [!IMPORTANT]
> 本文中所述的某些功能仅在 Windows 管理中心预览版中提供。 [如何实现获取此版本？](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>什么是 Windows Admin Center

[Windows 管理中心](../understand/windows-admin-center.md)是下一代 windows Server 管理工具，是传统 "内置" 工具（如服务器管理器）的后续版本。 它是免费的，可以在没有 Internet 连接的情况下安装和使用。 可以使用 Windows 管理中心来管理和监视运行 Windows Server 2016 或 Windows Server 2019 的超聚合基础结构。

![超聚合群集仪表板](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>关键功能

适用于超聚合基础结构的 Windows 管理中心的亮点包括：

- **用于计算、存储和不久网络的统一单窗格。** 查看虚拟机、主机服务器、卷、驱动器等，使其在一个专门构建的、一致的互连体验中。
- **创建和管理存储空间和 Hyper-v 虚拟机。** 完全简单的工作流，用于创建、打开、调整和删除卷;和创建、启动、连接到和移动虚拟机;还有很多。
- **强大的群集范围内监视。** 在群集中的每个服务器上，仪表板图形内存和 CPU 使用率、存储容量、IOPS、吞吐量和延迟，并在不正确的情况下出现明确的警报。
- **软件定义的网络（SDN）支持。** 管理和监视虚拟网络、子网、将虚拟机连接到虚拟网络和监视 SDN 基础结构。

Microsoft 正在积极开发超聚合基础结构的 Windows 管理中心。 它将接收可改善现有功能并添加新功能的频繁更新。

## <a name="before-you-start"></a>开始之前

若要在 Windows 管理中心中将群集作为超聚合基础结构管理，需要运行 Windows Server 2016 或 Windows Server 2019，并启用 Hyper-v 和存储空间直通。 此外，它还可以通过 Windows 管理中心启用并管理软件定义的网络。

> [!Tip]
> Windows 管理中心还为支持任何工作负荷的任何群集（适用于 Windows Server 2012 及更高版本）提供常规用途的管理体验。 如果这听起来更合适，将群集添加到 Windows 管理中心时，请选择 "[**故障转移群集**](manage-failover-clusters.md)"，而不是 "**超聚合群集**"。

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>为 Windows 管理中心准备 Windows Server 2016 群集

超聚合基础结构的 Windows 管理中心依赖于发布 Windows Server 2016 后添加的管理 Api。 你需要执行以下两个步骤，然后才能在 Windows Server 2016 群集中管理 Windows 管理中心：

1. 验证群集中的每个服务器是否已[为 Windows server 2016 （KB4103723）](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)或更高版本安装了2018-05 累积更新。 若要下载并安装此更新，请参阅 "**设置** > " "**更新 & 安全** > "**Windows 更新**，然后选择 "**联机检查 Microsoft 更新中的更新**"。
2. 在群集上以管理员身份运行以下 PowerShell cmdlet：

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> 在群集中的任何服务器上，只需运行一次 cmdlet。 你可以在 Windows PowerShell 中本地运行它，或使用凭据安全服务提供程序（CredSSP）来远程运行它。 根据你的配置，可能无法从 Windows 管理中心中运行此 cmdlet。

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>为 Windows 管理中心准备 Windows Server 2019 群集

如果群集运行 Windows Server 2019，则不需要上述步骤。 只需按照下一部分所述将群集添加到 Windows 管理中心即可！

### <a name="configure-software-defined-networking-optional"></a>配置软件定义的网络（可选） ###

可以将运行 Windows Server 2016 或2019的超聚合基础结构配置为使用软件定义的网络（SDN），步骤如下：

1. 准备 OS 的 VHD，这是安装在超聚合基础结构主机上的操作系统。 此 VHD 将用于所有 NC/SLB/GW Vm。
2. 从中[https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress)下载 SDN Express 下的所有文件夹和文件。
3. 使用部署控制台准备不同的 VM。 此 VM 应能够访问 SDN 主机。 此外，VM 应已安装 RSAT Hyper-v 工具。
4. 将已下载的 SDN Express 的所有内容复制到部署控制台 VM。 并共享此**SDNExpress**文件夹。 请确保每个主机都可以访问**SDNExpress**共享文件夹，如配置文件第8行中所定义：
   ```
    \\$env:Computername\SDNExpress
   ```
5. 将 OS 的 VHD 复制到部署控制台 VM 上的**SDNExpress**文件夹下的**images**文件夹中。
6. 在环境设置中修改 SDN Express 配置。 根据环境信息修改 SDN Express 配置后，完成以下两个步骤。
7. 以管理员权限运行 PowerShell 以部署 SDN：

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

部署需要大约30到45分钟。

## <a name="get-started"></a>立即开始行动

部署超聚合基础结构后，可以使用 Windows 管理中心对其进行管理。

### <a name="install-windows-admin-center"></a>安装 Windows Admin Center

如果尚未安装，请下载并安装 Windows 管理中心。 若要启动和运行，最快的方法是将其安装在 Windows 10 计算机上，并远程管理服务器。 此操作所需时间不到五分钟。 [立即下载](https://aka.ms/windowsadmincenter)或[了解有关其他安装选项的详细信息](../deploy/install.md)。

### <a name="add-hyper-converged-cluster"></a>添加超聚合群集

将群集添加到 Windows 管理中心：

1. 单击 "所有连接" 下的 " **+ 添加**"。
2. 选择添加**超聚合群集连接**。
3. 键入群集的名称，并在出现提示时键入要使用的凭据。
4. 单击 "**添加**" 完成操作。

群集将添加到连接列表。 单击它以启动仪表板。

![添加超聚合群集连接](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>添加启用了 SDN 的超聚合群集（Windows 管理中心预览）

最新的 Windows 管理中心预览版支持为超聚合基础结构定义的软件网络管理。 通过将网络控制器 REST URI 添加到超聚合群集连接，可以使用超聚合群集管理器来管理 SDN 资源和监视 SDN 基础结构。

1. 单击 "所有连接" 下的 " **+ 添加**"。
2. 选择添加**超聚合群集连接**。
3. 键入群集的名称，并在出现提示时键入要使用的凭据。
4. 选中 **"配置网络控制器**以继续"。
5. 输入**网络控制器 URI** ，然后单击 "**验证**"。
6. 单击 "**添加**" 完成操作。

群集将添加到连接列表。 单击它以启动仪表板。

![添加启用了 SDN 的超聚合群集连接](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> 当前不支持具有 Kerberos 身份验证的 SDN 环境 Northbound 通信。

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>Windows Server 2016 和 Windows Server 2019 的管理是否有差异？

是。 超聚合基础结构的 Windows 管理中心接收频繁的更新，这些更新可改善 Windows Server 2016 和 Windows Server 2019 的体验。 但是，某些新功能仅适用于 Windows Server 2019 –例如，切换开关以进行重复数据删除和压缩。

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>能否使用 Windows 管理中心来管理其他用例（而非超聚合）的存储空间直通，例如聚合横向扩展文件服务器（SoFS）或 Microsoft SQL Server？

超聚合基础结构的 Windows 管理中心并不提供特定于存储空间直通的其他用例的管理或监视选项–例如，无法创建文件共享。 但是，仪表板和核心功能（如创建卷或替换驱动器）适用于任何存储空间直通的群集。

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>故障转移群集与超聚合群集之间有何区别？

通常，术语 "超聚合" 指的是在相同的群集服务器上运行 Hyper-v 并存储空间直通，以虚拟化计算和存储资源。 在 Windows 管理中心的上下文中，单击 "连接" 列表中的 " **+ 添加**" 时，可以选择添加**故障转移群集连接**还是**超聚合群集连接**：

- **故障转移群集连接**是故障转移群集管理器桌面应用程序的后续版本。 它为支持任何工作负荷的任何群集（包括 Microsoft SQL Server）提供了一种熟悉的通用管理体验。 它适用于 Windows Server 2012 及更高版本。

- **超聚合群集连接**是针对存储空间直通和 hyper-v 定制的全新体验。 它采用仪表板并重点使用图表和警报进行监视。 它适用于 Windows Server 2016 和 Windows Server 2019。

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>为什么需要 Windows Server 2016 的最新累积更新？

超聚合基础结构的 Windows 管理中心依赖于自 Windows Server 2016 发布以来开发的管理 Api。 这些 Api 已添加到[Windows Server 2016 （KB4103723）的2018-05 累积更新](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)中，可在5月8日的2018内使用。

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>使用 Windows Admin Center 需要多少费用？

Windows Admin Center 不会在 Windows 以外产生额外费用。

你可以使用 windows 管理中心（可单独下载），其中包含 Windows Server 或 Windows 10 的有效许可证，无需额外付费，它在 Windows 补充 EULA 下获得许可。

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center 是否需要 System Center？

否。

### <a name="does-it-require-an-internet-connection"></a>是否需要 Internet 连接？

否。

尽管 Windows 管理中心提供了与 Microsoft Azure 云强大且方便的集成，但超聚合基础结构的核心管理和监视体验完全在本地。 它可以在没有 Internet 连接的情况下安装和使用。

## <a name="things-to-try"></a>要尝试的功能

如果刚开始使用，以下是一些快速教程，可帮助您了解如何组织 Windows 管理中心的超聚合基础结构和工作原理。 请毋庸置疑，在生产环境中小心。 这些视频是使用 Windows 管理中心版本1804和 Windows Server 2019 的内部预览版本记录的。

### <a name="manage-storage-spaces-direct-volumes"></a>管理存储空间直通卷

<ul>
               <li>（0:37）<a href="https://youtu.be/o66etKq70N8">如何创建三向镜像卷</a></li>
               <li>（1:17）<a href="https://youtu.be/R72QHudqWpE">如何创建镜像加速奇偶校验卷</a></li>
               <li>（1:02）<a href="https://youtu.be/j59z7ulohs4">如何打开卷并添加文件</a></li>
               <li>（0:51）<a href="https://youtu.be/PRibTacyKko">如何打开重复数据删除和压缩</a></li>
               <li>（0:47）<a href="https://youtu.be/hqyBzipBoTI">如何扩展卷</a></li>
               <li>（0:26）<a href="https://youtu.be/DbjF8r2F6Jo">如何删除卷</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>创建卷，三向镜像</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>创建卷，镜像加速奇偶校验</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>打开卷并添加文件</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>启用重复数据删除和压缩</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>展开卷</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>删除卷</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>创建新的虚拟机

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. 在 "虚拟机" 工具的顶部，选择 "**清单**" 选项卡，然后单击 "**新建**" 以创建新的虚拟机。
3. 输入虚拟机名称，并在第1代和第2代虚拟机之间进行选择。
4. 然后，Uou 可以选择要最初在其中创建虚拟机的主机，或使用推荐的主机。
5. 选择虚拟机文件的路径。 从下拉列表中选择一个卷，或者单击 "**浏览**" 以使用文件夹选取器选择一个文件夹。 虚拟机配置文件和虚拟硬盘文件将保存在选定卷或路径的`\Hyper-V\[virtual machine name]`路径下的单个文件夹中。
6. 选择虚拟处理器数量，无论是要启用嵌套虚拟化、配置内存设置、网络适配器、虚拟硬盘，还是选择要从 .iso 映像文件或网络安装操作系统。
7. 单击 **“创建”** 创建虚拟机。
8. 创建虚拟机并将其显示在虚拟机列表中后，可以启动虚拟机。
9. 启动虚拟机后，可以通过 VMConnect 连接到虚拟机的控制台以安装操作系统。 从列表中选择虚拟机，单击 "**更多** > **连接**" 下载 .rdp 文件。 在远程桌面连接应用中打开 .rdp 文件。 由于这将连接到虚拟机的控制台，因此需要输入 Hyper-v 主机的管理员凭据。

[了解有关 Windows 管理中心的虚拟机管理的详细信息](manage-virtual-machines.md)。

### <a name="pause-and-safely-restart-a-server"></a>暂停和安全重新启动服务器

1. 从 "**仪表板**" 中，从左侧导航栏中选择 "**服务器**"，或单击仪表板右下角的 "**查看服务器 >** " 链接。
2. 在顶部，从 "**摘要**" 切换到 "**清单**" 选项卡。
3. 通过单击其名称来选择服务器，以打开 "**服务器**详细信息" 页。
4. 单击 "**暂停服务器" 进行维护**。 如果能够安全地继续，这会将虚拟机移动到群集中的其他服务器。 发生这种情况时，服务器将产生状态排出。 如果需要，可以在虚拟机上观看虚拟机 **> 清单**"页上，它们的主机服务器在网格中清楚地显示。 所有虚拟机都已移动后，服务器状态将**暂停**。
5. 单击 "**管理服务器**" 以访问 Windows 管理中心中的所有每服务器管理工具。
6. 单击 "**重新启动**"，然后单击 **"是"** 。 你将会返回到连接列表。
7. 返回到**仪表板**，当服务器关闭时，该服务器将被着色为红色。
8. 备份后，再次导航**服务器**页面，然后单击 "**从维护中恢复服务器**"，将服务器状态设置回 "简单"。 同时，虚拟机将向后移动–无需用户操作。

### <a name="replace-a-failed-drive"></a>更换有故障的驱动器

1. 驱动器出现故障时，**仪表板**的 "左上方**警报**" 区域中会出现警报。
2. 你还可以从左侧导航栏中选择 "**驱动器**"，或单击右下角的磁贴上的 "**查看驱动器 >** " 链接来浏览驱动器并查看其状态。 在 "**清单**" 选项卡中，网格支持排序、分组和关键字搜索。
3. 在**仪表板**中，单击警报以查看详细信息，如驱动器的物理位置。
4. 若要了解详细信息，请单击**驱动器**详细信息页的 "**前往驱动器**" 快捷方式。
5. 如果硬件支持，可以单击 "**打开/关闭指示灯**" 来控制驱动器指示灯。
6. 存储空间直通自动停用和撤离故障驱动器。 发生此情况时，驱动器状态为 "已停用"，其存储容量条为空。
7. 删除发生故障的驱动器，并插入其替换。
8. 在**驱动器 > 清单**中，将显示新的驱动器。 此时，警报会被清除，卷将恢复为 "正常" 状态，并且存储将重新平衡到新驱动器上–无需用户操作。

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>管理虚拟网络（使用 Windows 管理中心预览的启用了 SDN 的 HCI 群集）

1. 从左侧的导航栏中选择 "**虚拟网络**"。
2. 单击 "**新建**" 创建新的虚拟网络和子网，或选择现有的虚拟网络并单击 "**设置**" 修改其配置。
3. 单击现有虚拟网络以查看 VM 与虚拟网络子网和访问控制列表（应用到虚拟网络子网）的连接。

![管理虚拟网络](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>将虚拟机连接到虚拟网络（使用 Windows 管理中心预览的启用了 SDN 的 HCI 群集）

1. 从左侧导航栏中选择 "**虚拟机**"。
2. 选择现有虚拟机 > 单击 "**设置**" > 在 "**设置**" 中打开 "**网络**" 选项卡。
3. 配置 "**虚拟网络**" 和 "**虚拟子网**" 字段，将虚拟机连接到虚拟网络。

你还可以在创建虚拟机时配置虚拟网络。

![将虚拟机连接到虚拟网络](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>监视软件定义的网络基础结构（使用 Windows 管理中心预览的启用了 SDN 的 HCI 群集）

1. 从左侧导航栏中选择 " **SDN 监视**"。
2. 查看有关网络控制器、软件负载均衡器、虚拟网关的运行状况的详细信息，并监视虚拟网关池、公用和专用 IP 池使用情况以及 SDN 主机状态。

![监视 SDN 基础结构](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>反馈

这就是您的反馈！ 经常更新的最重要的好处是，倾听正在进行的工作以及需要改进的内容。 下面是一些使我们知道你的想法的方法：

- [在 UserVoice 上提交功能请求并为其投票](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Microsoft 技术社区的 Windows 管理中心论坛](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 推文`@servermgmt`

### <a name="see-also"></a>请参阅

- [Windows Admin Center](../understand/windows-admin-center.md)
- [存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [软件定义的网络](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
